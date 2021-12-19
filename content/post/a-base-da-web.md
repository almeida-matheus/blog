+++ 
title = "A base da WEB"
date = 2021-01-09T23:30:34-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Entenda os conceitos por trás de uma requisição WEB"
categoria = [
    "Redes",
]
+++

## IP

O endereço IP (Internet Protocol) é um número de identificação de cada dispositivo que está conectado a uma rede.

## URL

A URL (Uniform Resource Locator) é o endereço virtual de uma aplicação na WEB.

Para melhor exemplificar vamos levar em consideração o site atual: [https://blog.almeidamatheus.me/post/a-base-da-web/](https://blog.almeidamatheus.me/post/a-base-da-web/)

A URL sempre inicia com o protocolo (https://). Seguida pelo domínio (blog.almeidamatheus.me), o blog é o sub-dominio do almeidamatheus.me. Após isso é especificado o caminho para um recurso (/post/a-base-da-web), o recurso recurso é algo concreto na aplicação que queremos acessar.

Vale a pena ressaltar que depois do domínio pode vir a porta, se não for definida é utilizada a porta padrão desse protocolo (blog.almeidamatheus.me:443).

## DNS

Como vimos anteriormente, as maquinas são referenciadas na rede pelo seu número de endereço IP, mas para entrar nesse site não precisou especificar o IP mas sim o domínio, isso é por causa do DNS.

O servidor DNS (Domain Name System) são os responsáveis por localizar e traduzir o nome de um domínio para o endereço de IP correspondente.

Isto é bem útil porque embora as maquinas se comuniquem por números, com letras facilita a navegação do usuário, já que é mais fácil gravar palavras do que números.

## Portas TCP e UDP

Em um computador temos apenas um endereço IP e geralmente há vários serviços sendo utilizados simultaneamente, como por exemplo um navegador, um download via bit torrent, um servidor de arquivos FTP. Todas essas aplicações fazem uso da internet e o computador sabe quais dados pertence a determinados serviços graça ao número da porta que cada serviço utiliza, dessa forma esses serviços podem ser utilizados ao mesmo tempo sem entre em conflito.

Utilizando uma analogia da vida real é como se um serviço de entrega tivesse que entregar um pacote de dados em um endereço do prédio, nesse caso o prédio seria o IP e o número do apartamento seria a porta.

É importante frisar que um serviço pode utilizar mais de uma porta, e a porta do protocolo **HTTP** é 80 e a do **HTTPS** é 443.

Como as portas padrões são conhecidas pelo navegador, elas podem ser omitidas ao escrevermos uma URL.

## HTTP e HTTPS

De maneira resumido HTTP e HTTPS são protocolos que definem as regras da comunicação entre o cliente servidor.

Uma comunicação com HTTP sempre é iniciada pelo cliente que manda uma requisição ao servidor esperando por uma resposta.

Para saber mais sobre esses protocolos confira [essa postagem](https://blog.matheustech.com.br/post/protocolos-http-e-https/).


## **Métodos de requisição HTTP**

O protocolo HTTP define um conjunto de métodos de requisição responsáveis por indicar a ação a ser executada para um dado recurso, sendo os principais métodos são **GET** e **POST**.

### GET

As requisições do tipo **GET** são recomendadas para obter dados de um determinado recurso. 

A requisição é enviada como uma string na URL, isso porque os parâmetros são passados no cabeçalho (header); tem uma limitação de caracteres e no quesito performance é ligeiramente mais rápido que o método **POST**.

Exemplo de uso: Uma ferramenta de busca de postagens ou uma listagem de todos os produtos cadastrados.
### POST

Já as requisições **POST** são comumente utilizadas para enviar dados para serem processados no servidor.

Os parâmetros da requisição é encapsulada junto ao corpo (body) da requisição; não tem limitação de caracteres e no quesito performance é ligeiramente mais lento que o método **GET**.

Exemplo de uso: formulário para cadastro de usuário.

Outros metódos:
- **PUT**: É utilizado para alterar o recurso inteiro. Se o recurso já existir, ele deve ser atualizado. Se não existir, pode ser criado.
- **PATCH**: É utilizado para alterar um dado específico do recurso.
- **DELETE**: É utiliziado para deletar a informação especificada.
- **HEAD**: Funciona semelhante ao GET, porém, retorna somente os cabeçalhos de uma resposta.
- **TRACE**: Devolve a mesma requisição que for enviada veja se houve mudança e/ou adições feitas por servidores intermediários.
- **OPTIONS**: Retorna os métodos HTTP suportados pelo servidor para a URL especificada.
- **CONNECT**: Converte a requisição de conexão para um túnel TCP/IP transparente, geralmente para facilitar a comunicação criptografada com SSL HTTPS através de um proxy HTTP não criptografado.
## **Códigos de resposta HTTP**

Os códigos de status das respostas HTTP indicam se uma requisição HTTP foi corretamente concluída ou não. 

Essas respostas são agrupadas em cinco classes:

- Respostas de informação (100 - 199)
- Respostas de sucesso (200 - 299)
- Redirecionamentos (300 - 399)
- Erros do cliente (400 - 499)
- Erros do servidor (500 - 599)

Sendo os mais comuns:

- 200: OK; o pedido está OK

- 201: Created; o pedido foi preenchido e um novo recurso foi criado

- 400: Bad Request; o pedido não pode ser cumprido devido à sintaxe inválida

- 401: Unauthorized; o cliente deve se autenticar para obter a resposta solicitada.

- 403: Forbidden; o cliente não tem direitos de acesso ao conteúdo portanto o servidor está rejeitando dar a resposta. Diferente do código 401, aqui a identidade do cliente é conhecida.

- 404: Not Found; a página solicitada não pôde ser encontrada

- 500: Internal Server Error; uma mensagem de erro genérica do lado do servidor

- 503: Service Unavailable: o servidor não está pronto para manipular a requisição

- 504: Gateway Timeout; o servidor está atuando como um gateway e não obtém uma resposta a tempo.

## Requisição HTTP

Com todos os conceitos anteriores em mente confira um exemplo de uma requisição comum.

### **HTTP Request / Response**

- O cliente (navegador) entra em contato com um servidor DNS para descobrir o ip e onde o site está hospedado
- Em seguida, envia um HTTP request (requisição) com o metódo **GET** no header para o servidor web na porta 80
- O servidor web recebe a requisição e se a página existir o servidor executa a aplicação
- O servidor retorna um HTTP response (resposta) para o cliente (navegador) com o header e body
- O cliente (navegador) recebe a resposta e exibe o conteúdo da aplicação requisitada caso o código 200 de sucesso seja retornado

Você pode checar todos os detalhes de uma requisição através da ferramenta de desenvolvedor presente nos navegadores através das teclas F12 ou CTRL + SHIFT + I

Confira a imagem abaixo um exemplo de requisição **HTTP**.

Requisição HTTP
:--------------------------------------------------:
![](https://almeidamatheus.netlify.app/uploads/21/01/web-http-headers-body.png)


