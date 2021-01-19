+++ 
title = "Introdução ao Linux"
date = 2021-01-16T23:44:53-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Uma visão geral sobre sistemas operacionais, kernel e shell"
categoria = [
    "Linux",
]
+++


## O que é Linux?

Linux é um componente de um sistema operacional, é o núcleo do sistema, este é chamado de kernel.

## Hardware

Tudo começa com o hardware, o componente mais baixo nível de um computador, que é todo componente físico, interno ou externo, que determina do que um dispositivo é capaz e como você pode usá-lo. De maneira resumida é a implementação física do sistema

## Sistema Operacional

Sistema Operacional é um programa (software) que gerencia recursos de software e hardware.  É formado por um conjunto de rotinas que oferece serviços aos usuários e às suas aplicações. Ele tem a função de fornecer instruções ao hardware, capacitando a realização das operações de um equipamento. 

Dentro do sistema operacional temos os componentes abaixo:

## Kernel

Acima do hardware, fica o kernel, este é quem vai contatar o hardware diretamente. Portanto ele é basicamente o coração e cérebro do sistema, é toda implementação lógica de como o sistema vai gerenciar a memoria, como vai ser o desempenho no sistema.

Vale ressaltar que o linux é um kernel open source, significa que todo o seu código está disponível para visualização, utilização e até modificação de acordo com a necessidade.

## Shell

Shell é o interpretador de comandos, é uma interface para os usuários de comunicarem com o kernel, ou seja, o shell envia os comandos para o kernel

Um exemplo disso é quando abrimos o terminal e digitamos um comando `cd ~`, esse comando é interpretado pelo shell, e ele entende que esse comando para mudar de pasta.

De forma resumida, digita o comando no terminal, o shell lê, o kernel responde e o shell trás o resultado da resposta.

O shell nativo do linux é o bash, mas também existem outros como o zsh. A sintaxe de ambos são muito similares, a diferença fica mais no quesito recursos.


Hardware - Kernel - Shell         
:--------------------------------------------------:
![](https://almeidamatheus.netlify.app/uploads/21/01/linux-kernel-shell.png)

## Distribuições

Os sistemas operacionais que utilizam o Kernel Linux são chamados de distribuições Linux. 

Imagine que o kernel linux é um sorvete, as distribuições seriam uma barraquinha de sorvete, cada barraquinha tem o seu jeito proprio de fazer o sorvete, uma forma diferente, um sabor diferente.

As distribuições do mercado estão divididas em dois grupos:

### Enterprise

São distribuições voltadas para empresas, comumente instalada como servidores. A principal característica é o fato dessas serem muito estáveis, suas atualizações tem foco em segurança para possíveis falhas

Exemplo: Red Hat, CentOS, Suse, Ubuntu Server

### Desktop

São distribuições voltada para os sistema de usuários finais, comumente instalado em computadores pessoais. Produtividade, objetividade, desempenho são algumas caracteristicas importantes. No mais apresenta interface gráfica e suporte para programas convencionais como editores de vídeo, navegador, jogosl, etc.

Exemplo: Debian, Mint, Fedora, Manjaro

## Interface Gráfica

É a interface gráfica que é responsável pela aparência do sistema operacional, tendo um contato direto com o usuário, exibindo todas as informações da melhor maneira possível.

Uma parte muito importante é a interface gráfica, que terá um contato direto com o usuário, exibindo todas as informações da melhor maneira possível.

Existem muitas interfaces disponíveis no mundo Linux, mas entre as mais famosas podemos encontrar:

- GNOME
- KDE
- XFCE
- Cinnamon

Cada interface apresenta suas devidas diferenças, algumas tem foco mais no desempenho para hardware fracos, outras focam na liberdade de customização, outras em produtividade.

## Comandos Linux

Existem alguns comandos que podemos ver o que foi dito acima

Através do comando `hostnamectl`  podemos ver o sistema operacional e sua versão; kernel e sua versão; arquitetura.

```
	 Static hostname: mint
         Icon name: computer-laptop
           Chassis: laptop
        Machine ID: b76cc7b1bbdc489e93909d**********
           Boot ID: 53a2db7d576d42feaff5bd**********
  Operating System: Linux Mint 20
            Kernel: Linux 5.4.0-26-generic
      Architecture: x86-64
```

1. **Mint** - Distribuição linux
2. **20** - Versão da Distribuição linux
3. **Linux –** Nome do Kernel
4. **5.4.0-26 –** Versão do Kernel
5. **x86_64** – Versão da arquitetura (64 bit)

Observação: esse comando funciona em distribuições baseadas em GNU systemd, todavia existem comandos com resultados semelhantes como: `uname -a` , `lsb_release -a`, `cat /etc/*-release`.

Para ver a interface gráfica basta digitar o comando `echo $XDG_CURRENT_DESKTOP`

```
X-Cinnamon
```

Para ver informações do hardware de maneira resumida basta utilizar o comando `inxi`

```
CPU: Single Core AMD Ryzen 5 3600 (-UP-) 
Speed: 3593 MHz 
Kernel: 5.4.0-26-generic x86_64 
Up: 36m Mem: 775.0/2100.9 MiB (36.9%) 
Storage: 15.00 GiB (55.2% used) 
Procs: 187 
Shell: bash 5.0.16 
inxi: 3.0.38
```



