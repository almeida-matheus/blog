+++ 
title = "Protocolo SSH"
date = 2021-01-14T13:04:59-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Saiba como configurar o SSH de maneira funcional e segura"
categoria = [
    "Linux",
]
+++

## Definição

SSH significa secure shell, é um protocolo de rede criptografado utilizado para criar conexões seguras entre sistemas.

Seu principal uso é para fazer conexões remotas, embora seja possível fazer tunelamentos e redirecionamentos de portas TCP.

Roda na porta TCP de número 22 por padrão.

## Como funciona

- Cliente abre uma conexão para o Servidor
- Servidor responde com o protocolo e sua Public Host Key
- Cliente confirma que esse é o servidor correto
- Geração da Public Key
- 
    - Gerar um Large Prime Number
    - Acordar o método de encriptação (exemplo: AES)
    - Gerar uma Private Key
    - Combinar Private Key + método de encriptação + Large Prime
    - Number para gerar a Public Key
    - Realizada a troca de Public Keys (a chave publica do servidor pro cliente e chave publica do cliente pro servidor)
- Geração da Secret Session Key
- 
    - Combinar Private Key local + Public Key recebida + Large Prime Number para gerar a Secret Session Key
    - Ela será igual para ambos (tanto o cliente e servidor tera essa chave localmente)
- Essa Secret Session Key (simétrica) é usada para encriptar e decriptar a conexão

Todas essas etapas descritas acima é para estabelecer a conexão, antes de enviar qualquer dado, por isso o SSH é seguro, não é atoa que seu nome é secure shell.

Uma maneira de ver todas as etapas detalhadas de uma conexão SSH é passando o `-vvv` como argumento, exemplo: `ssh usuario@192.168.0.10 -vvv`

Observação: Neste artigo estou levando em consideracão que o usuário do servidor se chama usuario e que o ip é 192.168.0.10

## Configurar o SSH na prática

Para isso irei utilizar o OpenSSH.

### Servidor

1 - Instalar

```
apt install openssh-server
#ou
yum install openssh-server -y
```

2 - Iniciar o serviço

```
systemctl start sshd
```

(Por padrão abre a porta 22 )

### Testar funcionamento

No cliente basta usar o netcat no IP desse servidor

```
nc -v 192.168.0.10 22
```

(Se tiver um resultado como SSH - 2.0-OpenSSH_7.4 significa que o serviço está ativo)

No servidor basta usar o comando `ss` (antigo netstat) para confirma se está escutando na porta 22 com o TCP

```
ss -ln | grep 22
```

(Se não aparecer nada na tela é porque o serviço não está ativo)

Se o serviço não estiver ativo provavelmente é porque o firewall está bloqueando, nesse caso tem que criar uma exceção pra porta do SSH no firewall, outra opção é checar os possíveis erros no /var/log.

### Cliente

1 - Instalar

```
apt-get install openssh-client
#ou
yum install openssh-client 
```

2 - Conectar no ssh

```
ssh usuario@10.10.10.10
```

## Autenticação com chaves assimétricas

Conexão SSH            
:--------------------------------------------------:
![](https://almeidamatheus.netlify.app/uploads/21/01/ssh-conexao.png)


O cliente tem 2 chaves, uma pública e outra privada, a chave pública pode ser compartilhada com os outros, diferente da chave privada, que deve ficar no próprio dispositivo, de maneira confidencial.

Para o cliente poder conectar via SSH no servidor, a chave pública do cliente (id_rsa.pub) deve estar no arquivo authorized_keys do servidor, esse arquivo guarda as chaves dos clientes autorizados para a conexão SSH.

A chave pública e privada são complementares, isso porque quando o cliente for abrir uma conexão, ele envia sua chave privada e o servidor confere se essa chave privada complementa a chave pública autorizada, caso for, acontece a autenticação da conexão.

### Servidor

Local das chaves: /etc/ssh 

(ssh_host_rsa_key e ssh_host_rsa_key.pub)


Arquivo de configuração: /etc/ssh/sshd_config


Modificar no arquivo de configuração:

`PubkeyAuthentication yes`

O PubkeyAuthentication habilita a autenticação de chaves, já o AuthorizedKeysFile é o lugar onde vai ficar armazenada as chaves autorizadas, por padrão é em ~/.ssh/authorized_keys


Configuração SSH          
:--------------------------------------------------:
![](https://almeidamatheus.netlify.app/uploads/21/01/ssh-configuracao-chave.png)


Sempre que modificar esse arquivo de configuração tem que reiniciar o serviço SSH

```
systemctl restart sshd
```

### Cliente

Local das chaves: ~/.ssh

(id_rsa e id_rsa.pub)

Local de configuração do ssh: /etc/ssh/ssh_config

Sempre que configurar tem que reiniciar o serviço com o comando  `systemctl restart ssh`

Gerar chave no cliente com ssh-keygen

```
ssh-keygen -b 2048 -t rsa -v
```

Essa chave deve estar no authorized_keys do servidor, portanto basta utilizar o comando:

```
ssh-copy-id usuario@192.168.0.10
```

Ou simplesmente copiar a chave pública do cliente `~/.ssh/id_rsa.pub` e colocar no arquivo authorized_keys do servidor `~/.ssh/authorized_keys`

Após isso basta conectar normalmente

```
ssh usuario@192.168.0.10 -p 22
```

Observação: `-p` é a porta; se o local padrão da chave privada for alterado deve passar o caminho dela "path" pelo argumento `-i`

## Medidas de segurança

### Cliente

As chaves ficam no arquivo ~/.ssh e isso não é muito seguro porque alguém pode chegar e copiar esse arquivo, a ideia é salvar a chave privada em local seguro, para isso podemos usar o ssh-agent

O ssh-add manda as chaves para o ssh agent, que é um serviço que guarda as chaves criptografadas.

Para fazer isso basta utilizar o comando `ssh-add`  e para listar a chave por impressão digital basta utilizar o comando `ssh-add l`  , se quiser listar a chave de maneira completa basta utilizar o comando `ssh-add L` , feito isso já pode deletar a chave privada com o comando `rm id_rsa`

Para fica ainda mais seguro, utilizar um pen drive com a chave privada para fazer a conexão SSH é uma opção, assim utilizaria o seguinte comando:

```
ssh -i /mnt/id_rsa usuario@192.168.0.10
```

`-i` é para especificar o caminho da chave privada.

### Servidor

Além do método de autenticação por chaves assimétricas podemos configurar mais coisas no arquivo de configuração do SSH (sshd_config). Essa técnica é chamada de Hardening.

Desabilitar o acesso por senha:

**`PasswordAuthentication no`**

(Isso se a autenticação por chave tiver ativada)

Não permitir acesso como root:

**`PermitRootLogin no`**

Mudar a porta TCP do SSH:

**`Port 30022`**

Desabilitar a opção de acesso com senha vazia:

**`PermitEmptyPasswords no`**

Bloquear conexão de usuários ou grupos especificos:

**`DenyUsers nome_do_usuario12`**

**`DenyGroups nome_do_grupo12`**

(Embora  a opção de cima seja uma boa medida de segurança, a opção de baixo é melhor porque nega todo o resto)

Permitir alguns usuários ou grupos específicos:

**`AllowsUsers nome_do_usuario Domain\Users`**

**`AllowGroups nome_do_grupo`**

Utilizar a versão mais segura do protocolo:

**`Protocol 2`**

Definir um tempo limite de inatividade, ao passar desse tempo o usuário será automaticamente desconectado (terminal ocioso a partir do último comando):

**`ClientAliveInterval 360`**

**`ClientAliveCountMax 0`**

(Da pra fazer a mesma coisa utilizando o comando export TMOUT=360 no terminal)

Limitar o número de tentativas de conexão:

**`MaxAuthTries 6`**

Limitar a quantidade de  pessoas  que podem estar logadas simultaneamente utilizando shell via SSH:

**`MaxSessions 10`**

Limitar o número de conexões não autenticadas por um determinado IP:

**`MaxStartups 5:60:10`**

(Quando tiver 5 conexões não autenticadas, a partir daí irá rejeitar 60% das conexões do IP do cliente, quando chegar a 10 conexões não autenticadas desse mesmo endereço IP irá rejeitar todas, evitar bruteforce)

Autenticar com as contas do linux com todas restrições do PAM:

**`UsePAM yes`**

Mostrar quando ocorreu a ultima conexão do usuário:

**`PrintLasLog yes`**

Tempo do Inicio da autenticação e final da autenticação:

**`LoginGraceTime 60`**

Verificar se o usuário tem as devidas permissões:

**`StrictModes yes`**

banner de aviso quando for conectar:

**`Banner /etc/issue.net`**

(Deve digitar o banner no arquivo /etc/issue.net)

banner depois de conectado:

**`PrintMotd yes`**

(Deve digitar o banner no arquivo /etc/motd)

Para aplicar as modificações do arquivo de configuração devemos utilizar os seguintes comandos:

```
/etc/init.d/ssh restart
#ou
systemctl restart sshd.service
#ou
service ssh restart
```

Outras dicas de segurança:

Utilizar a versão do SSH mais atualizada possível, porque versões anteriores podem ter vulnerabilidades.

Não colocar nomes de usuários comuns, porque a chance dos nomes comuns estarem em dicionários "wordlist" de atacantes é alta.

Para garantir ainda mais a segurança deve utilizar programas externos, como um firewall e o fail2ban.



