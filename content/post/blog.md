+++ 
title = "Como criar um blog utilizando o framework Hugo"
date = 2020-10-06T18:18:11-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Entenda como criar um blog utilizando o framework Hugo"
tags = [
    "diversos",
]
+++

## Sobre o Hugo

O framework [hugo](https://gohugo.io/) é um gerador de sites estáticos, desenvolvido na linguagem Go, sua principal vantagem é o desempenho comparado aos seus concorrentes como wordpress, jekyll, etc.

---

## 1º passo - Instalar o hugo

```
sudo apt-get install hugo
```

## 2º passo - criar um site

```
hugo new site nome_site
```

vá para a pasta criada: **cd nome_site**

## Extra - Estrutura do hugo

Digitando **ls** ou **tree** você poderá observar a estrutura da pasta criada

- pasta archetypes contém default.md: que é basicamente o cabeçalho de todas as páginas, nesse arquivo temos como exemplo o titulo, a data e rascunho (draft)
- arquivo config.toml: é nele que voce modifica a estrura básica do site, como o titulo, menus de navegação, url do site, etc
- pasta content: é onde tem o conteudo do site, por exemplo, onde fica a pasta do site, a página sobre, etc
- pasta layout: é onde fica os arquivos HTML, responsavel pela estrutura do site
- pasta static: é onde fica os arquivos reponsáveis pela estilização e ações por meios de scripts, arquivos de CSS e javascript respectivamente 
- pasta themes: é nessa pasta onde fica o tema adicionado

## 3º passo - Preparar o ambiente

Como iremos hospedar o site na netlify, então temos que adicionar o arquivo netlify.toml no projeto, para isso devemos:

Criar o arquivo netlify.toml

```
touch netlify.toml
```

Modificar o arquivo (nesse exemplo eu utilizei o Visual Studio Code para editar)

```
code netlify.toml
```

Adicionar o código abaixo (alterar o hugo_version para a versão do seu hugo, para ver isso digite **hugo —version**)

```
[build]
publish = "public"
command = "hugo --gc --minify"

[context.production.environment]
HUGO_VERSION = "x.xx.x"
```

Agora devemos colocar o site no GitHub, primeiramente é preciso criar um repositório no GitHub

```
git init
git remote add origin https://github.com/xxxx/xxxx.git
git status
git add .
git commit -m "first commit"
git push origin master 
```

No Netlify devemos:

- Conectar sua conta do GitHub no Netlify
- Selecionar o repositório do site hugo

Devemos mudar o nome do dominio do site na Netlify, o caminho é esse abaixo:

Domain settings > Domain management > Options > Edit site name

## 4º passo - adicionar um tema

Primeiramente escolha um tema de sua preferência [clicando aqui](https://themes.gohugo.io/)

- Clique em Download
- No GitHub clique em Code e copie a url Https

O método git clone para instalar temas não é compatível com o Netlify. Se você fosse usar o clone do git, seria necessário remover recursivamente o subdiretório .git da pasta do tema e, portanto, impediria a compatibilidade com versões futuras do tema.

Uma abordagem melhor é instalar um tema como um submódulo git. Então iremos adicionar o seguinte comando para o tema ir na pasta themes

```
git submodule add https://github.com/xxxx/hugo-theme-xxxx themes/xxxx
```

Iremos copiar tudo que está na pasta exampleSite dentro do tema para a raiz do site que criamos (o comando abaixo é para ser utilizado na raiz do site)

```
cp -r themes/xxxx/exampleSite/* .
```

## 5º passo - customizar o site

### Configurar o config.toml ou config.yaml

Modifique os dados do site através do config conforme o necessário

Importante: no arquivo config.toml deve alterar o atributo baseURL para o nome de domínio que você configurou na netilify

```
baseURL = "https://xxxx.netlify.com"
```

### Configurar o arquivo archetypes/default.md

No arquivo default.md é interessante alterar o rascunho para falso (draft: false)

### Outras modificações

Caso você queira customizar o tema para além das customizações presentes no arquivo config, você deve copiar o arquivo junto com o seu caminho para a pasta raiz do site, exemplo:

Arquivo a ser modificado: /themes/xxxx/static/css/style.css

Arquivo recriado com as suas alterações em: /static/css/style.css

## 6º passo - adicionar uma página ou post

```
hugo new post/post-exemplo.md
```

O caminho da postagem irá estar em /content/post/post-exemplo.md

Agora você pode editar o post utilizando a linguagem de marcação [Markdown](https://www.markdownguide.org/basic-syntax/) (nesse exemplo eu utilizei o Visual Studio Code para editar)

```
code /content/post/post-exemplo.md
```

## 7º passo - rodar o projeto

Rode o projeto na sua máquina local e cheque se está tudo certo como deveria antes de hospedar na nuvem

```
hugo server -D
```

-D é para mostrar os posts que estão com o rascunho ativado (draft: true)

## 8º passo - atualizar o projeto

```
git status
git add .
git commit -m " config blog added"
git push origin master 
```

Feito isso é só clicar no link do seu site hospedado na netlify e seu site estará funcionando perfeitamente

