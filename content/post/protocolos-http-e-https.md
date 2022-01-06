+++ 
title = "Protocolos HTTP e HTTPS"
date = 2021-01-02T23:56:58-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Entenda o que HTTP e HTTPS e quais são as diferenças"
categoria = [
    "Redes",
]
+++

## Protocolos de comunicação

A internet é baseada na arquitetura cliente-servidor, onde há um cliente que solicita e um servidor que responde.

- Clientes geralmente são navegadores, mas podem ser qualquer tipo de programa ou dispositivo.
- Servidores geralmente são computadores na nuvem onde está armazenado a aplicação.

### HTTP

O Hypertext Transfer Protocol (HTTP) é um protocolo de comunicação utilizado na transferência de dados entre o cliente e o servidor, ele que define a forma de como os dados são trafegados na rede através das regras.

Ao se conectar na URL do site na WEB é possível ver os conteúdos que existem nesse site, e isso é possível graças a esse protocolo.

### HTTPS

o HTTPS realiza a mesma função do HTTP porém de forma mais segura, não é atoa que o seu significado é Hypertext Transfer Protocol Secure.

## Porque o HTTPS é mais seguro?

Antes de responder essa perguntar temos que entender os seguintes pontos abaixo.

### Criptografia assimétrica

Utiliza-se de chaves ligadas matematicamente sendo uma chave pública e uma chave privada. O que foi cifrado pela chave pública só pode ser decifrado pela chave privada. Isso garante que os dados cifrados pelo navegador (chave pública) só podem ser lidos pelo servidor (chave privada). Apesar de ser bem segura a única desvantagem é a lentidão do processo.

### Criptografia simétrica

Por outro lado, temos a criptografia simétrica, que usa a mesma chave para cifrar e decifrar os dados. Apesar do processo ser mais rápido que a chave assimétrica, não é tão segura como ela.

### Certificados

O HTTPS para garantir segurança usa criptografia baseada em chaves públicas e privadas e para gerar essas chaves publicas e privadas é preciso garantir a identidade de quem possui essas chaves e isso é feito a partir de um certificado digital, ou seja, um certificado digital é utilizado para identificar determinada entidade e ainda é utilizada para geração das chaves de criptografia.

O certificado digital prova a identidade de um site, onde temos informações sobre o seu domínio e a data de expiração desse certificado.

Além disso, o certificado ainda guarda a chave pública que é utilizada para criptografar (cifrar) os dados que são trafegados entre cliente e servidor.

Ainda é necessário que uma autoridade certificadora, que nada mais é que um órgão ou entidade confiável, garanta não apenas a identidade do site mas também a validade do certificado.

### Sobre o HTTPS

o HTTPS usa ambos os métodos de criptografia, assimétrica e simétrica.

No certificado, vem a chave pública para o cliente utilizar, responsável pela encriptação dos dados.

O servidor continua na posse da chave privada, que é responsável por descriptografar o dados.

Isso é seguro, porém lento como vimos anteriormente, e por isso o cliente gera uma chave simétrica ao vivo. Uma chave só para ele e o servidor com o qual está se comunicando naquele momento.

Essa chave exclusiva (e simétrica) é então enviada para o servidor utilizando a criptografia assimétrica (chave privada e pública) e então é utilizada para o restante da comunicação.

Portanto o HTTPS começa com  criptografia assimétrica para depois mudar para criptografia simétrica. Essa chave simétrica será gerada no início da comunicação e será reaproveitada nas requisições seguintes. 

Concluímos então que os navegadores em posse da chave pública criptografam as informações e as enviam para o servidor que as descriptografa com a chave privada.

### SSL/TLS

O HTTPS é mais seguro por causa dos certificados SSL/TLS, sendo SSL (Secure Sockets Layer) uma tecnologia para proteger uma conexão de Internet criptografando dados enviados entre um site e um navegador (ou entre dois servidores). Já o TLS (Transport Layer Security) é uma versão atualizada e mais segura do SSL.

O SSL/TLS é essencial sempre que houver informações sensíveis sendo transmitidas, como nomes de usuário, senhas e informações de pagamento.

Vale ressaltar que quando você instala um certificado SSL a transmissão de dados é configurada para ser feita via HTTPS. Ambas as tecnologias andam de mãos dadas e não funcionam uma sem a outra.

Quando o cliente faz uma requisição para o servidor que utiliza o HTTPS acontece as seguintes etapas: 

TLS SSL Handshake         
:--------------------------------------------------:
![tls-ssl-handshake](https://almeidamatheus.netlify.app/uploads/21/01/web-tls-ssl-handshake.png)

- Ocorre o SSL/TLS handshake para estabelecer uma conexão encriptada entre dois pontos usando SSL (semelhante ao 3-way TCP handshake).
- Com a conexão estabelecida o cliente envia para o servidor um “Client Hello” contendo as versões de TLS suportadas, as cipher suites suportadas e uma chave randômica.
- Então o servidor responde com “Server Hello” enviando uma cópia do certificado dele mesmo, o protocolo escolhido (sempre o protocolo mais atualizado), o cipher escolhido e outra chave randômica.
- Daí o navegador verifica se o certificado é original com o emissor do certificado. Precisa ser uma autoridade de certificação confiável caso contrário pode ocorrer aqueles erros de validação em certificados self-signed.
- Se estiver OK, o navegador envia uma mensagem ao servidor da web e troca as informações de criptografia necessárias: chaves (PKI) e código de hash.

A partir de então os dados compartilhados entre o navegador e o servidor da web são criptografados, diferente do HTTP em que os dados são transportados em texto puro para o servidor.

### De maneira resumida, confira a tabela abaixo sobre as principais diferenças entre HTTP e HTTPS

| Serviço              | HTTP      | HTTPS      |
|----------------------|-----------|------------|
| URL                  | http://   | https://   |
| Porta                | 80        | 443        |
| Camada               | Aplicação | Transporte |
| Certificado          | Não       | SSL/TLS    |
| Criptografia         | Não       | Sim        |
<!--| Validação do domínio | Não       | Sim        | -->
