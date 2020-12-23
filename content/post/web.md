+++ 
title = "A base da WEB"
date = 2020-10-08T20:41:47-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Entenda os protocolos da web e suas requições na teoria e na prática"
categoria = [
    "redes",
]
+++

# Rede Mundial de Computadores

A Rede Mundial de Computadores ou World Wide Web (www) trata da comunicação entre clientes e servidores da internet.

- Clientes geralmente são navegadores, mas podem ser qualquer tipo de programa ou dispositivo.
- Servidores geralmente são computadores na nuvem onde está armazenado a aplicação (site)

# O que é HTTP e HTTPS?

Hypertext Transfer Protocol e Hypertext Transfer Protocol Secure são protocolos de comunicação utilizados na transferência de dados entre o cliente e o servidor.<br>
Ao se conectar na URL do site na WEB é possível ver os conteúdos que existem nesse site, e isso é graças ao protocolo HTTP E HTTPS. Já que eles determinam como qualquer dado recebido ou enviado é transmitido.

# Qual a diferença entre HTTP e HTTPS?

Em poucas palavras, podemos dizer que ambos tem a mesma função, porém o HTTPS é uma versão do HTTP mais segura, não é atoa que o seu significado é Hypertext Transfer Protocol Secure.

## Confira a tabela abaixo com as principais diferenças

| Serviço              | HTTP      | HTTPS      |
|----------------------|-----------|------------|
| URL                  | http://   | https://   |
| Porta                | 80        | 443        |
| Camada               | Aplicação | Transporte |
| Certificado          | Não       | SSL/TLS    |
| Criptografia         | Não       | Sim        |
<!--| Validação do domínio | Não       | Sim        | -->

# Porque o HTTPS é mais seguro?

Por causa dos certificados SSL/TLS, sendo SSL (Secure Sockets Layer) uma tecnologia para proteger uma conexão de Internet criptografando dados enviados entre um site e um navegador (ou entre dois servidores). Já o TLS (Transport Layer Security) é uma versão atualizada e mais segura do SSL.

O SSL/TLS é essencial sempre que houver informações sensíveis sendo transmitidas, como nomes de usuário, senhas e informações de pagamento.

Vale ressaltar que quando você instala um certificado SSL a transmissão de dados é configurada para ser feita via HTTPS. Ambas as tecnologias andam de mãos dadas e não funcionam uma sem a outra.

# Como é o funcionamento desses protocolos?

## HTTP Request / Response

- O  cliente (navegador) entra em contato com um servidor DNS para descobrir o ip e onde o site está hospedado e, em seguida, envia um HTTP request (requisição) para o servidor web

- O servidor web recebe a requisição e se a página existir o servidor executa a aplicação para processar a requisição e retorna o código 200. Mas caso o servidor não conseguir encontrar a página solicitada, ele enviará uma mensagem de erro com o código 404 por não ter encontrado a pagina

- O servidor retorna um HTTP response (resposta) para o cliente (navegador)

- O cliente (navegador) recebe a resposta e exibe o conteúdo da aplicação requisitada caso o código 200 de sucesso seja retornado


Observação: se for **HTTPS**, antes de passar para a segunda etapa, irá ocorrer as seguintes etapas:

- Considerando que o site é protegido por SSL/TLS, ocorre o SSL/TLS handshake para estabelecer uma conexão encriptada entre dois pontos usando SSL (semelhante ao 3-way TCP handshake).
- Com a conexão estabelecida o cliente envia para o servidor um "Client Hello" contendo as versões de TLS suportadas, as cipher suites suportadas e uma chave randômica.
- Então o servidor responde com "Server Hello" enviando uma cópia do certificado dele mesmo, o protocolo escolhido (sempre o protocolo mais atualizado), o cipher escolhido e outra chave randômica.
- Daí o navegador verifica se o certificado é original com o emissor do certificado. Precisa ser uma autoridade de certificação confiável caso contrário pode ocorrer aqueles erros de validação em certificados self-signed
- Se estiver OK, o navegador envia uma mensagem ao servidor da web e troca as informações de criptografia necessárias: chaves (PKI) e código de hash

A partir de então os dados compartilhados entre o navegador e o servidor da web são criptografados.

![tls-ssl-handshake](/images/tls-ssl-handshake.png)

## Códigos de resposta HTTP

Lembra quando eu citei a seguinte etapa do HTTP request / response? "O servidor web recebe a requisição e se a página existir o servidor executa a aplicação para processar a requisição e retorna o código 200". Pois é, isso é só um cenário de vários possíveis, porque por exemplo, se o cliente fizer requisição de uma página que não existe no site, o servidor retornara uma mensagem de erro com o código 404 para o cliente (navegador). 

Confira abaixo os códigos de status das respostas **HTTP**

- Respostas de informação (100 - 199)
- Respostas de sucesso (200 - 299)
- Redirecionamentos (300 - 399)
- Erros do cliente (400 - 499)
- Erros do servidor (500 - 599)

# Métodos de requisição HTTP

O protocolo HTTP define um conjunto de métodos de requisição responsáveis por indicar a ação a ser executada para um dado recurso, sendo eles:

- **GET**: Requisita um representação do recurso especificado (O mesmo recurso pode ter várias representações, ao exemplo de serviços que retornam XML e JSON).
- **HEAD**: Retorna os cabeçalhos de uma resposta (sem o corpo contendo o recurso)
- **POST**: Envia uma entidade e requisita que o servidor aceita-a como subordinada do recurso identificado pela URI.
- **PUT**: Requisita que um entidade seja armazenada embaixo da URI fornecida. Se a URI se refere a um recurso que já existe, ele é modificado; se a URI não aponta para um recurso existente, então o servidor pode criar o recurso com essa URI.
- **DELETE**: Apaga o recurso especificado.
- **TRACE**: Ecoa de volta a requisição recebida para que o cliente veja se houveram mudanças e adições feitas por servidores intermediários.
- **OPTIONS**: Retorna os métodos HTTP que o servidor suporta para a URL especificada.
- **CONNECT**: Converte a requisição de conexão para um túnel TCP/IP transparente, usualmente para facilitar comunicação criptografada com SSL (HTTPS) através de um proxy HTTP não criptografado.
- **PATCH**: Usado para aplicar modificações parciais a um recurso.

Os principais métodos são **GET** e **POST**.

As requisições do tipo **GET** são recomendadas para obter dados de um determinado recurso. Como em um formulário de busca ou em uma listagem de todos os produtos cadastrados.

Já as requisições **POST** são mais utilizadas para enviar informações para serem processadas, como por exemplo, criar algum recurso, como um produto, ou um cliente.

Sendo que o método **GET** que quando utilizado, os parâmetros são passados no cabeçalho da requisição e por isso podem ser vistos pela URL. Já o método **POST** ao contrário do **GET**, envia os parâmetros no corpo da requisição **HTTP**, ou seja, escodem eles da URL.

Vale ressaltar que você pode checar tudo isso através da ferramenta de desenvolvedor dos navegadores, para ativar geralmente é com a tecla f12 ou ctrl + shift + i.

Confira a imagem abaixo um exemplo de requisição **HTTP**.

![htttp-headers-body](/images/htttp-headers-body.png)

# Interceptando requisições HTTP na prática

## Ferramenta

Irei utilizar o [Wireshark](https://www.wireshark.org/), que é uma ferramenta utilizada para analisar os tráfegos da rede, sendo categorizado como um sniffer, já que através dele podemos capturar todos os pacotes que estão circulando na rede.

## Alvo

O alvo desse exemplo é o meu antigo colégio, mais precisamente a página de login do sistemas de notas, perceba que a URL do site é precedido pelo protocolo **HTTP**.

![login-coltec](/images/login-coltec.png)

## Utilizando a ferramenta

Irei deixar o captura de dados ativada no Wireshark, e nesse meio tempo irei clicar no botão enviar do site para enviar o formulário contendo o login e senha para o servidor.

Devemos levar em consideração que a requisição é **HTTP** e o método utilizado para enviar o login e senha para o servidor é o **POST.** 

Então devemos filtrar os dados no Wireshark dessa forma: http.request.method=="POST"

![wireshark-coltec](/images/wireshark-coltec.png)

Selecionando esse pacote e expandindo as informações do formulário HTML iremos achar o login e senha em formato de texto sem criptografia alguma.

## Conclusão

Considerando que a rede wireless desse colégio é pública, ou seja, os professores e os alunos ficam conectados na mesma rede, basta deixar o Wireshark capturando todos os pacotes de rede e quando algum professor fazer o login no sistemas de nota do colégio, no mesmo momento irá aparecer esse pacote e dentro dele poderíamos visualizar as credenciais utilizados  pelo professor para fazer login. Por isso que é tão importante ter um site com a URL precedida do **HTTPS**, por que se o site estivesse seguro nada disso seria possível porque todos os dados estariam criptografados, podendo ser descriptografado somente com uma chave particular, que nesse caso, só o servidor iria ter.


