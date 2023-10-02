+++
title = "O que é e como utilizar o aws-vault para gerenciar credenciais AWS"
date = 2022-03-20T00:47:11-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Gerencie suas credenciais AWS de forma segura com o aws-vault"
categoria = [
    "Nuvem",
]
+++

![AWS — Computação em Nuvem](https://miro.medium.com/v2/format:webp/1*d2PGRzWIbQbFFq2kmlzFqQ.png)

Nesse artigo irei abordar os principais pontos da ferramenta [aws-vault](https://github.com/99designs/aws-vault), que é utilizada para gerenciar credenciais da AWS de forma segura.

Para melhor compreensão sobre esse assunto é imprescindível o conhecimento da configuração e utilização das credenciais AWS com AWS CLI da forma tradicional, caso você não tenha recomendo conferir o seguinte artigo:

[Como gerar, configurar e utilizar credencias AWS com AWS CLI](https://medium.com/r/?url=https%3A%2F%2Falmeida-matheus.medium.com%2Fcomo-gerar-configurar-e-utilizar-credencias-aws-com-aws-cli-84b56403e79e)

# Considerações

Para demonstrar todos os processos na prática eu irei utilizar os seguintes recursos da AWS com o seus respectivos nomes de exemplo:
- **ID da conta**: 123456789012.
- **Nome da role**: ADMIN.
- **Nome da role com condição de MFA**: ADMIN-MFA.
- **Nome do usuário IAM**: matheus.

# Sobre a ferramenta

O aws-vault é uma ferramenta open source de linha de comando utilizada para armazenar e gerenciar suas credenciais AWS com facilidade e segurança.

Essa ferramenta foi projetado para atuar de forma complementar ao tradicional uso AWS CLI, portanto ele também utiliza-se das variáveis de ambiente e do arquivo `~/.aws/config` para configurar os perfis, mas não faz uso do arquivo `~/.aws/credentials` por questão de segurança, irei detalhar isso nos próximos tópicos.

Confira a documentação oficial clicando aqui.

# Como utilizar

> Pré requisito: Criar access key e secret key de um usuário IAM na AWS.

Salve suas credenciais com o comando `aws-vault add` seguido do nome do perfil:
```
aws-vault add user-account1
```

O comando acima irá te pedir para entrar com a access key e secret key e salvará no perfil `user-account1`.

> **Observação**: Caso esteja utilizando um usuário que acessa a AWS pelo Single Sign On (SSO) não é necessário a etapa acima, visto que o SSO não utiliza as credenciais de longo prazo e sim gera credenciais temporárias a sessão.

O próximo passo é configurar os perfis da AWS no arquivo `~/.aws/config`.
```
[user-account1]
region = us-east-1
output = json

[profile admin-prd]
source_profile = account1
role_arn= arn:aws:iam::123456789012:role/ADMIN
role_session_name = matheus
region = us-east-1
```

Nesse caso eu criei perfil `admin-prd` que está atrelado a uma role com base nas credenciais do perfil `user-account1`.

Para utilizar um perfil basta utilizar o comando `aws-vault exec` seguido do nome do perfil:
```
aws-vault exec admin-prd
```

Dessa forma os comandos subsequentes irão utilizar as credenciais da role assumida através do perfil `admin-prd`.

Essas credenciais por padrão expiram em 1 hora, mas isso pode ser alterado adicionando o argumento `--duration` seguido das horas exemplo: `aws-vault exec admin-prd --duration 12h`.

> **Opcional**: Para confirmar que você realmente assumiu a role, basta usar o seguinte comando `aws sts get-caller-identity` que retornará informações sobre o seu usuário e role.

Saída desse comando em json:
```
{
    "Credentials": {
        "AccessKeyId": "ASIA...",
        "SecretAccessKey": "...",
        "SessionToken": "...",
        "Expiration": "2022-02-19T00:43:30+00:00"
    }
}
```
# Principais comandos
- Adicionar credenciais ao armazenamento de chaves seguro:
`aws-vault add [profile]`

- Usar as credenciais temporárias de um perfil:
`aws-vault exec [profile]`

- Executar um comando usando credenciais temporárias de um perfil:
`aws-vault exec [profile] -- aws s3 ls`

- Fazer login no AWS Console pelo navegador com base em um perfil:
`aws-vault login [profile]`

- Listar os perfis disponíveis:
`aws-vault list`

- Rotacionar credenciais do seu usuário IAM que foram configuradas em um perfil:
`aws-vault rotate [profile]`

- Remover as credenciais do seu armazenamento de chaves seguro de um perfil específico:
`aws-vault remove [profile]`

- Remover sessões gerenciadas pelo aws-vault antes que elas expirem:
`aws-vault clear`

# Como funciona

O aws-vault armazena as credenciais do usuário no armazenamento de chaves seguro do seu sistema e a partir delas geram as credenciais temporárias.

Para gerar credenciais temporárias o aws-vault usa o serviço o AWS STS por meio das chamadas de API **GetSessionToken** ou **AssumeRole**.

Essas credenciais temporárias consistem em um ID de chave de acesso, (AccessKeyId) uma chave de acesso secreta (SecretAccessKey) e um token de segurança (SessionToken).

Com o aws-vault essas credenciais são automaticamente configuradas em variáveis de ambiente.

Com essas credenciais configuradas você pode acessar os recursos da AWS através do AWS CLI ou AWS SDK.

Confira as variáveis de ambiente que o aws-vault exporta automaticamente após seu uso com o seguinte do comando `aws-vault exec admin-prd -- env | grep AWS`:

```
AWS_VAULT=admin-prd
AWS_DEFAULT_REGION=us-east-1
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=ASIA...
AWS_SECRET_ACCESS_KEY=...
AWS_SESSION_TOKEN=...
AWS_SECURITY_TOKEN=...
AWS_SESSION_EXPIRATION=2022-02-19T11:16:27Z
```

Ao utilizar um perfil, automaticamente criará um nested subshell com as credenciais temporárias exportadas no seu ambiente Shell do usuário ou da role assumida, você pode confirmar isso com o comando `echo $SHLVL` para mostrar em qual "camada" você está.

# Vantagens

- Compatibilidade e praticidade: Compatível com configuração AWS CLI padrão.
- Credenciais temporárias: Exportação de credenciais simplificados.
- Criptografia: O armazenamento de credenciais é criptografado.
- Rotação de credenciais: Rotação de credenciais simplificada.

## Compatibilidade e Praticidade

Conforme dito anteriormente, o aws-vault funciona de forma complementar ao AWS CLI, isso implica que o aws-vault também lê os seus perfis configurados para o AWS CLI no `~/.aws/config` e essas configurações são compatíveis, ou seja, para configurar perfis utiliza-se dos mesmo campos mas também há a possibilidade de adições de outras campos para funcionalidades exclusivas do aws-vault.

Para assumir uma role ou usar um usuário IAM para obter suas credenciais é necessário ter um perfil configurado no arquivo `~/.aws/config` .
Confira um exemplo com um perfil de uma role que é autenticado com o MFA e um perfil para um usuário do AWS SSO:
```
[user-account1]
region = us-east-1
output = json

[profile admin-prd]
source_profile = default
role_arn= arn:aws:iam::123456789012:role/ADMIN-MFA
role_session_name = matheus
mfa_serial = arn:aws:iam::123456789012:mfa/matheus
region = us-east-1

[profile user-account2]
sso_start_url=https://account2.awsapps.com/start/
sso_region=us-east-1
sso_account_id=210987654321
sso_role_name=READ-ONLY
region=us-east-1
output=json
```

O aws-vault é fácil de instalar e usar e é compatível com o arquivo de configuração padrão.

No uso convencional do AWS CLI, você pode alternar de perfil usando o argumento `--profile` ou definindo a variável de ambiente `AWS_PROFILE`.

Exemplo: `aws s3 ls --profile admin-prd`

Para fazer a mesma tarefa com o aws-vault basta utilizar o comando `aws-vault exec profile`.

Exemplo: `aws-vault exec admin-prd -- aws s3 ls`

Além do mais, ao utilizar o aws-vault ele exporta automaticamente as variáveis de ambiente para você, conforme o próximo tópico.

## Credencias temporárias

No AWS CLI por padrão você pode mudar de perfil usando o argumento `--profile` nos comandos AWS CLI, porém dessa forma as credenciais temporárias não são exportadas.

Existem formas de exportas essas credenciais com os métodos tradicionais do AWS CLI, mas é relativamente trabalho e o aws-vault simplifica isso, você pode conferir esses métodos tradicionais neste artigo.

A grande vantagem de exportar credenciais temporárias é que além do uso do AWS CLI, também abre margem para serem usadas em ferramentas que usam o SDK da AWS por trás, como por exemplo o Terraform ou até mesmo IDE's para aplicações que consomem recursos da AWS.

Conforme o tópico de explicação do funcionamento da ferramenta, para gerar credenciais temporárias o aws-vault usa o serviço o AWS Security Token Service (STS) por meio das chamadas de API **GetSessionToken** ou **AssumeRole**.

Mesmo quando estiver utilizando um perfil com credenciais de usuário IAM, é utilizado o STS **GetSessionToken** para gerar credenciais temporárias, isso significa que suas credenciais de acesso de longo prazo (*access key* e *secret key*) nunca serão expostas no Shell, acarretando assim uma segurança maior.

As chaves de acesso de longo prazo, como aquelas associadas a usuários do IAM permanecem válidos até que sejam revogados manualmente. No entanto, as credenciais de segurança temporárias obtidas por meio de roles do IAM e outros recursos do AWS STS expiram após um curto período de tempo.

> Observação: Utilizando o argumento `--no-session` em conjunto você pula o processo de geração do token, sendo assim suas credenciais serão diretamente expostas, você pode confirmar isso com o comando `aws-vault exec admin --no-session -- env | grep AWS`.

## Armazenamento criptografado

Normalmente, quando você configura um novo usuário com o `aws configure` ele adiciona credenciais no arquivo `~/.aws/credentials`.

O primeiro problema com isso é que ele deixa suas chaves (access key e secret key) em texto puro, o que é um risco, pois qualquer um pode obter essa chave e acessar todos os recursos disponíveis.

Para evitar esse possível problema o aws-vault usa os controles do seu sistema para armazenar suas credenciais de forma segura, os Vaulting Backends.

Confira algumas das opções com base em seu sistema operacional:
- **Mac**: Keychain macOS.
- **Windows**: Windows Credential Manager.
- **Linux**: KWallet, Gnome Keyring.

Quando você faz uma operação exec no aws-vault é exigido uma senha, ìsso se deve ao mecanismo de segurança de onde está armazenado suas credenciais, para o aws-vault utilizar suas credenciais primeiro ele valida se o usuário que está invocando o aws-vault é realmente aquele que pertence às credenciais configuradas.

## Rotação de credencial

Rotacionar as credenciais de acesso programático (*access key* e *secret key*) periodicamente é uma boa prática de segurança, por que se por algum motivo essas credenciais vazarem não terá problema porque já foram desativadas.

Para rotacionar as credenciais no método padrão da AWS você deve ir no Console AWS e no serviço IAM você desativa e remove as credenciais antigas e então cria a nova e adicionar através do comando `aws configure`.

Com o aws-vault esse processo é simplificado, visto que você já adicionou as credenciais, para rotaciona-las basta utilizar apenas um comando que ele irá abstrair todo o processo convencional:
```
aws-vault rotate user-account1
```

O `user-account1` é o perfil que eu adicionei as credenciais anteriormente com o comando `aws-vault add user-account1`.

# Conclusão

O intuito desse foi abordar o uso das credenciais AWS no ponto de vista de segurança com o uso de uma ferramenta específica, o aws-vault, que além de demonstrar o seu funcionamento, fica o entendimento do porquê utiliza-la devido as suas vantagens.

A ideia é garantir as boas práticas de segurança quando tratamos de credenciais aliado a praticidade.

No "mundo" open source existem várias ferramentas para suprir uma determinada necessidade ou resolver um problema, pensando nisso irei recomendar outras ferramentas que tem essa mesma utilidade, parecido com o aws-vault por se tratar de um CLI tem o [AWSume](https://awsu.me/), já para aplicação com interface gráfica eu recomendo o [Leapp](https://www.leapp.cloud/).

Dessa forma podemos ter um melhor entendimento quando formos utilizar ferramentas de acesso programático, como AWS CLI e AWS SDK.

---

Se você tem interesse nos assuntos mencionados nesse artigo sinta-se a vontade para comentar ou me contactar no [Linkedin](https://www.linkedin.com/in/matheus-almeida-costa/).

Até a próxima!

Este artigo foi originalmente publicado no [Medium](https://almeida-matheus.medium.com/o-que-%C3%A9-e-como-utilizar-o-aws-vault-para-gerenciar-credenciais-da-aws-d8e73f669e8) :).