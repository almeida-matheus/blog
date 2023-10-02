+++
title = "Como gerar, configurar e utilizar credenciais AWS com AWS CLI"
date = 2022-03-14T00:47:11-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Aprenda gerenciar credenciais da AWS com perfis, credenciais temporárias, assume role e outros"
categoria = [
    "Nuvem",
]
+++

![AWS — Computação em Nuvem](https://cdn-images-1.medium.com/v2/resize:fit:800/1*GAO5BQVLxF49G68d2csZfQ.png)


Nesse artigo irei abordar na prática os processos para configurar e utilizar credenciais da AWS de modo geral, isso inclui configuração perfis, credenciais temporárias com MFA, assume role, dentre outros.

# Introdução

As conexões com a AWS são feitas via API e existem diferentes interfaces de acesso, confira as 3 maneiras de acessar a AWS:
- **Console AWS**: Via navegador utilizando o protocolo HTTPS.
- **AWS CLI**: Via terminal utilizando um Shell (bash, zsh, etc).
- **AWS SDK**: Via funções de bibliotecas da AWS utilizando linguagens de programação.

Para acessar o Console AWS é necessário habilitar uma senha para a autenticação no login do [Portal IAM](https://console.aws.amazon.com/iam/home).

Já para acessar a AWS via CLI e SDK é necessário utilizar credenciais de acesso, sejam elas credenciais de longo prazo (*access key* e *secret key*) ou credenciais temporárias.

Essas credenciais podem ser geradas e utilizadas tanto pelo AWS CLI tanto pelo SDK, nesse artigo vou focar especificamente no AWS CLI.

O AWS Command Line Interface (AWS CLI) é uma ferramenta de código aberto que permite interagir com os serviços da AWS através do Shell, por meio de execução de comandos que implementam uma funcionalidade equivalente àquela fornecida pelo AWS Management Console.

Além do mais, com essa ferramenta há a possibilidade de desenvolver scripts para automatizar ações e extrações de dados do seu ambiente AWS.

Atualmente a versão 2 é a mais atualizada e provém suporte para as seguintes plataformas:
- Linux.
- macOS.
- Windows.
- Docker.

Confira os manuais da própria AWS com explicação de instalação do AWS CLI para cada uma dessas plataformas [clicando aqui](https://docs.aws.amazon.com/pt_br/cli/latest/userguide/getting-started-install.html).

**Considerações**

Para demonstrar todos os processos na prática eu irei utilizar os seguintes recursos da AWS com o seus respectivos nomes de exemplo:
- **ID da conta**: 123456789012.
- **Nome da role**: ADMIN.
- **Nome da role com condição de MFA**: ADMIN-MFA.
- **Nome do usuário IAM**: matheus.

# Armazenar credenciais

Para as credenciais da AWS serem utilizadas, por padrão elas devem estar em um arquivo específico ou em variáveis de ambiente, confira logo em seguida essas 2 opções.

## Opção 1  -  Os aquivos do AWS CLI

### Arquivo `~/.aws/credentials`

É o local onde ficam as suas credenciais, chave de acesso (*access key*) e chave secreta (*secret key*). Opcionalmente pode armazenar as credenciais temporárias, nesse caso além das chaves deve ter um token de sessão.

Exemplo com credenciais de longo prazo (*long-term credentials*):
```
[default]
aws_access_key_id = AKIA...
aws_secret_access_key = ...
```

Exemplo com credencias temporárias:
```
[default]
aws_access_key_id = ASIA...
aws_secret_access_key = ...
aws_session_token = ...
```

### Arquivo `~/.aws/config`

É o local onde você cria os perfis de acesso a AWS, em cada perfil você pode especificar configurações como: regiões, formato da saída de comandos, roles baseadas em outros perfis, MFA de usuário, perfis autenticados do SSO, dentre outros.

Exemplo de configuração de um perfil para uso de uma role:
```
[default]
region = us-east-1
output = json

[profile admin]
source_profile = default
role_arn = arn:aws:iam::123456789012:role/ADMIN
role_session_name = matheus
region = us-east-1
```
### Formato do conteúdo desses arquivos

Os dois arquivos têm formatos de cabeçalho ligeiramente diferentes. Em ambos os arquivos, o cabeçalho da seção do perfil padrão é `[default]`. Mas para perfis nomeados, em `~/.aws/config`, o cabeçalho da seção é `[profile PROFILE_NAME]`, enquanto em `~/.aws/credentials` o cabeçalho é apenas `[PROFILE_NAME]`.

Existem vários campos compatíveis com o AWS CLI para o arquivo `~/.aws/config`, nesse artigo irei abordar alguns deles, mas você pode conferir todos campos possíveis através da documentação oficial da própria AWS [clicando aqui](https://docs.aws.amazon.com/pt_br/cli/latest/userguide/cli-configure-files.html).

## Opção 2 - Variáveis de Ambiente

Variável de ambiente é um valor dinâmico atribuído no ambiente do seu sistema que pode ser utilizado pelo seu sistema operacional e programas.

Essas variáveis são guardadas em uma "lista" de chave-valor e no geral são usadas por aplicações que dependem de determinados valores para sua configuração, esse consumo e atribuição de valores é feito de uma maneira bem mais simples se compararmos com um arquivo por exemplo.

Exemplos de variáveis padrões do Unix:
- `USER`: Usuário autenticado no momento.
- `HOME`: Caminho completo do diretório raiz do usuário.

O AWS CLI consome determinadas variáveis para configurar o acesso por esse meio.

Confira as variáveis de ambientes essenciais para configurar suas credencias AWS:
- `AWS_ACCESS_KEY_ID`: Especifica uma chave de acesso (*access key*) associada a um usuário ou *role* do IAM.
- `AWS_SECRET_ACCESS_KEY`: Especifica a chave secreta (*secret key*) associada à chave de acesso (*access key*). Essencialmente, essa é a "senha" para a chave de acesso.
- `AWS_SESSION_TOKEN`: Especifica o valor de *token* de sessão que é necessário se você estiver usando credenciais de segurança temporárias obtidas diretamente das operações do AWS STS.

Existem outras variáveis de ambiente compátives com o AWS CLI para sua configuração, você pode conferir todas essas através da documentação oficial da própria AWS [clicando aqui](https://docs.aws.amazon.com/pt_br/cli/latest/userguide/cli-configure-envvars.html).

Por padrão o AWS CLI da prioridade para as variáveis de ambiente em relação aos arquivos mencionados, isso implica que mesmo que tenham access key e secret key no arquivo `~/.aws/credentials` se tiver variáveis de credenciais AWS exportadas (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`) os comandos do AWS CLI serão com base nas variáveis de ambiente.

# Utilizar credenciais de usuário do IAM

> **Pré requisito**: Criar access key e secret key de um usuário IAM .

![Acesso programático - Criação de credenciais de um usuário IAM](https://cdn-images-1.medium.com/v2/resize:fit:800/1*s-__NiAssxKK_5iGU4152A.png)

> **Observação**: Em todos os cenários de configuração, se você quiser saber qual é a identidade das credenciais ativas no momento, você pode usar o seguinte comando do AWS CLI: `aws sts get-caller-identity`.

## Cenário 1 - Uso padrão de credenciais

Para armazenar as credenciais automaticamente no local adequado e com o perfil padrão, você pode digitar comando abaixo:
```
aws configure
```

Em seguida basta preencher a *access key* e *secret key* e a região e output padrão.

Esse comando resultará os seguintes campos no arquivo `~/.aws/credentials`:
```
[default]
aws_access_key_id = AKIA...
aws_secret_access_key = ...
```

Ele irá criar um perfil `default` com as credenciais passadas, feito isso basta usar os comandos do AWS CLI normalmente, todos eles irão utilizar as credenciais desse perfil por padrão.

Exemplo de comando:
```
aws s3 ls
```

O comando acima retornará todos os buckets com base nas permissões do usuário detentor dessas credenciais configuradas.

Para confirmar o usuário a qual pertence essas credencias você pode utilizar o comando:

```
aws sts get-caller-identity
```
Este comando retornará como saída um json com informações do usuário:
```
{
    "UserId": "AROAYUIQW6FDNDFSGBJZQQ:matheus",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/matheus"
}
```
## Cenário 2 - Configurar perfis

É possível utilizar diferentes credenciais através de perfis, isto é útil quando você quer utilizar a AWS em múltiplas contas ou até mesmo usar a mesma conta com permissões diferentes.

Para isto basta utilizar o comando `aws configure` e passar o argumento `--profile` seguido do nome do perfil:
```
aws configure --profile home
aws configure --profile work
```

Esse comando resultará os seguintes campos no arquivo `~/.aws/credentials`:
```
[home]
aws_access_key_id = AKIA...
aws_secret_access_key = ...

[work]
aws_access_key_id = AKIA...
aws_secret_access_key = ...
```

Dessa forma para utilizar os comandos do AWS CLI agora você deve passar o argumento `--profile` seguido do nome do perfil:
```
aws s3 ls --profile home
```
O comado acimara irá listar os buckets S3 com as credenciais (permissões) do perfil `home`.

É possível utilizar comandos do AWS CLI sem necessariamente passar o perfil como argumento toda vez, para isso basta adicionar como valor da variável de ambiente `AWS_PROFILE` o nome do perfil, exemplo:
```
export AWS_PROFILE=home
```

Dessa forma quando for utilizar algum comando AWS CLI, por padrão, irá usar as credenciais do perfil `home`.

Você pode validar com o seguinte comando:
```
aws sts get-caller-identity
```
Esse comando retornará informações do usuário a qual pertence às credenciais utilizadas para esta requisição, que será o mesmo que foi utilizado na configuração do perfil `home` anteriormente.

## Cenário 3  -  Usar credenciais temporárias

Neste cenário iremos utilizar a chamada de API [GetSessionToken](https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html), que fornece credenciais temporárias com base nas credenciais e permissões existentes de um usuário.

Essa chamada de API normalmente é usada para fornecer um token MFA ou para fornecer credenciais com limite de tempo para fins de teste. Por exemplo, eu poderia fornecer credenciais que efetivamente permitem que você use meu usuário do IAM, mas apenas por um tempo limitado.

Por ser com base nas credenciais do usuário, você deve ter a *access key* e *secret key* configuradas no `~/.aws/credentials` ou nas variáveis de ambiente.

O comando abaixo irá gerar credenciais temporárias com o MFA:
```
aws sts get-session-token --serial-number arn:aws:iam::123456789012:mfa/matheus --token-code 111111
```

Argumentos:
- `--serial-number`: O arn do MFA do usuário.
- `--token-code`: O token do MFA momentâneo.

O resultado será algo como:
```
{
    "Credentials": {
        "AccessKeyId": "ASIA...",
        "SecretAccessKey": "...",
        "SessionToken": "...",
        "Expiration": "2022-02-11T00:43:30+00:00"
    }
}
```
A saída desse comando gera várias informações em um json. Para utilizar as credenciais você precisa da `AccessKeyId`, `SecretAccessKey` e `SessionToken`. Há também o campo `Expiration` com a data/hora no fuso horário UTC que indica quando essas credenciais temporárias do usuário irão expirar. Quando expirarem você pode recorrer à chamada de API **GetSessionToken** novamente.

Para utilizar as credenciais temporárias autenticadas com o MFA desse usuário existem 2 alternativas, as variáveis de ambiente e o arquivo `~/.aws/credentials`.

Nesse exemplo optarei por utilizar as variáveis e os comandos para atribuir valores de ambos são:
```
export AWS_ACCESS_KEY_ID=ASIA..
export AWS_SECRET_ACCESS_KEY=...
export AWS_SESSION_TOKEN=...
```

Os comandos acima irão exportar três variáveis de ambiente, você deve atribuir os valores de cada uma delas com os respectivos valores obtidos do objeto json da saída do comando do AWS CLI.

Feito isso, os próximos comandos do AWS CLI irão utilizar as mesmas policies desse usuário, porém com a autenticação do MFA ativa.

> **Observação**: Como boa prática, todos os usuários do IAM devem obrigatoriamente utilizar o MFA, isso pode ser feito com condição de exigência de MFA nas policies, portanto nesse cenário terá a necessidade de se autenticar com o MFA.

# Utilizar credenciais de roles assumidas

Uma role tem o objetivo principal de conceder permissões temporárias para realizar chamadas de API em uma conta AWS. Para usar uma role ela deve ser assumida.

Ao trabalhar com serviços da AWS como o EC2, podemos aproveitar as roles do IAM para ter permissões para desempenhar uma determinada função, isso também funciona quando você está em um ambiente local, porém há um detalhe, para assumir uma role nesse caso é necessário ter as credencias de acesso do usuário IAM devidamente configuradas conforme os passos anteriores.

## Cenário 1 - Assumir role com arquivo `~/.aws/credentials`

Confira a disposição dos arquivos AWS para este cenário:

Arquivo `~/.aws/credentials/`.
```
[default]
aws_access_key_id = AKIA...
aws_secret_access_key = ...
```

Arquivo `~/.aws/config`.
```
[default]
region = us-east-1
output = json

[profile admin]
role_arn = arn:aws:iam::123456789012:role/ADMIN
role_session_name = matheus
source_profile = default
region = us-east-1

[profile admin-mfa]
source_profile = default
role_arn= arn:aws:iam::123456789012:role/ADMIN-MFA
role_session_name = matheus
mfa_serial = arn:aws:iam::123456789012:mfa/matheus
region = us-east-1
```

Campos:
- `source_profile`: Nome do perfil que contém as credenciais para assumir a role.
- `role_arn`: O arn da role que será assumida no perfil.
- `role_session_name`: Um nome para identificar o usuário enquanto assume a role.
- `mfa_serial`: O arn do MFA do usuário.

Com esses arquivos devidamente configurados, agora quando você for utilizar um comando do AWS CLI, por "trás dos panos" irá assumir a role.

Neste caso para usar comandos AWS com base em permissões de uma role você teria que passar o argumento `--profile` seguido do nome do perfil:
```
aws sts get-caller-identity --profile admin
```

Este comando te retornará informações da *role* que você assumiu:
```
{
    "UserId": "AROAYUIQW6FDNDFSGBJZQQ:matheus",
    "Account": "123456789012",
    "Arn": "arn:aws:sts::123456789012:assumed-role/ADMIN/matheus"
}
```

> **Observação**: Se estiver um MFA atrelado no perfil, como o perfil `ADMIN-MFA`, ao utilizar algum comando do AWS CLI no primeiro momento irá te exigir o token MFA que será guardado em cache, dessa forma os comandos sucessores não irão te exigir mais até certo tempo.


## Cenário 2  -  Assumir role com STS AssumeRole
Em vez de deixar a AWS CLI fazer todo o trabalho, há uma maneira de assumir a role e obter suas credenciais com a chamada de API [STS AssumeRole](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html), confira o comando para isso:
```
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/ADMIN --role-session-name matheus
```
Argumentos:
- `--role-arn`: O arn do MFA do usuário.
- `--role-session-name`: Um nome para identificar o usuário enquanto assume a role.

Resultado da saída do comando acima:
```
{
    "Credentials": {
        "AccessKeyId": "ASIA...",
        "SecretAccessKey": "...",
        "SessionToken": "...",
        "Expiration": "2022-02-11T00:43:30+00:00"
    },
    "AssumedRoleUser": {
        "AssumedRoleId": "AROA...:matheus",
        "Arn": "arn:aws:sts::123456789012:assumed-role/ADMIN/matheus"
    }
}
```

A saída desse comando gera várias informações em um json. Para utilizar as credenciais você precisa da `AccessKeyId`, `SecretAccessKey` e `SessionToken`. Há também o campo `Expiration` com a data/hora no fuso horário UTC que indica quando as credenciais temporárias da role irão expirar. Quando expirarem você pode recorrer à chamada de API **AssumeRole** novamente.

Para utilizar as credenciais temporárias autenticadas com o MFA desse usuário existem 2 alternativas, as variáveis de ambiente e o arquivo `~/.aws/credentials`.

Nesse primeiro exemplo eu optarei por utilizar as variáveis e os comandos para atribuir valores de ambos são:
```
export AWS_ACCESS_KEY_ID=ASIA..
export AWS_SECRET_ACCESS_KEY=...
export AWS_SESSION_TOKEN=...
```

Os comandos acima irão exportar 3 variáveis de ambiente, você deve atribuir os valores de cada uma delas com os respectivos valores obtidos do objeto json da saída do comando do AWS CLI.

Feito isso, ao utilizar o comando `aws sts get-caller-identity` você verá o arn da *role* que você assumiu, para retornar a usuário IAM basta remover essas variáveis com o comando `unset` seguido do nome das mesmas.

## Cenário 3  -  Assumir role com STS AssumeRole e MFA ativo

Para assumir role com exigência de autenticação MFA iremos utilizar o mesmo comando do cenário anterior, porém com algumas adições, confira o comando para gerar credenciais temporárias:
```
aaws sts assume-role --role-arn arn:aws:iam::593277417112:role/ADMIN --role-session-name matheus --serial-number arn:aws:iam::123456789012:mfa/matheus --token-code 111111
```
Argumentos:
- `--role-arn`: O arn do MFA do usuário.
- `--role-session-name`: Um nome para identificar o usuário enquanto assume a role.
- `--serial-number`: O arn do MFA do usuário.
- `--token-code`: O token do MFA momentâneo.

O resultado do comando acima será semelhante ao que foi mencionado no cenário 2, porém com valores diferentes do objeto do json.

Para utilizar as credenciais dessa role irei configurar o arquivo `~/.aws/credentials` adicionando o seguinte texto:
```
[default]
aws_access_key_id = ASIA...
aws_secret_access_key = ...
aws_security_token= ...
```

Basta atribuir os valores de cada uma com os respectivos valores obtidos do objeto json da saída do comando do AWS CLI igual ao processo das variáveis de ambiente.

Ao utilizar o comando `aws sts get-caller-identity` você verá o arn da role que você assumiu, para retornar a usuário IAM basta remover o perfil `default` desse mesmo arquivo e configura-lo novamente.

## Bônus: Alterar a duração das credenciais

Como vimos, tanto a chamada de API **GetSessionToken** quanto a **AssumeRole** geram credenciais temporárias, que por padrão expiram em 1 hora.

Existe um argumento no qual podemos alterar o tempo de duração dessas credenciais, esse argumento é o `--duration-seconds` em que devemos passar logo em seguida os segundos de duração.

Confira um exemplo utilizando o **AssumeRole**:
```
aws sts assume-role --duration-seconds 43200 --role-arn arn:aws:iam::593277417112:role/ADMIN --role-session-name matheus
```

Com o comando acima irá gerar credenciais com o tempo de duração de 43200 segundos, isso poderá ser confirmado na chave `Expiration` do objeto na saída do json.

Esse tempo em segundos equivale a 12 horas, que é o tempo máximo permitido.

Vale ressaltar que por padrão as *roles* só permitem serem assumidas por no máximo uma hora, mas isso pode ser alterado conforme o campo "Maximum session duration" que está presente em cada *role*.

![AWS Management Console - Edição da duração da sessão de uma role](https://cdn-images-1.medium.com/v2/1*akqV_CeiaoxAiU8nrPPeEQ.png)

# Conclusão

O intuito desse artigo foi passar uma visão geral sobre as credenciais da AWS na prática.

Dessa forma podemos ter um melhor entendimento quando formos utilizar ferramentas de acesso programático, como AWS CLI e AWS SDK.

---

Se você tem interesse nos assuntos mencionados nesse artigo sinta-se a vontade para comentar ou me contactar no [Linkedin](https://www.linkedin.com/in/matheus-almeida-costa/).

Até a próxima!

Este artigo foi originalmente publicado no [Medium](https://almeida-matheus.medium.com/como-gerar-configurar-e-utilizar-credencias-aws-com-aws-cli-84b56403e79e) :).