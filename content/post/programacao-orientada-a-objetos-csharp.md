+++
title = "Programação orientada a objetos com C#"
date = 2021-04-10T23:29:36-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Os 4 pilares essenciais da POO na teoria e na prática"
categoria = [
    "Programação",
]
+++

## Definição

Programação orientada orientada a objeto (POO), do inglês Object Oriented Programming (OOP), é um paradigma aplicado na programação que consiste na interação entre diversas unidades chamadas de objetos.

**Classe:** Uma classe define os membros e a estrutura que os objetos deste tipo de classe devem seguir.

**Objeto:** Objetos são instâncias de classes, que determinam qual informação um objeto contém e como ele pode manipulá-la. Em resumo é uma abstração de algum modelo real dentro do nosso sistema.

No objeto podem existir **atributos e propriedades** (variáveis) que definem a característica do objeto, como o nome, idade, etc. Existe também os **métodos** (funções), que definem as ações de um objeto, como pagar, depositar, etc.

Um programa pode criar vários objetos da mesma classe.

> As classes na orientação a objetos funcionam como um molde para os objetos. Os objetos são criados a partir de uma classe e muitos deles podem ser feitos da mesma classe.

Observação: Neste artigo irei utilizar a linguagem C# como exemplo, mas a essência é a mesma em outras linguagens, só mudando um a sintaxe.

## Benefícios

- **Reutilização de código:** Com POO, não precisamos repetir o código de criação dos campos basta reutilizar a classe.

- **Organização do código:** Encontrar e organizar o código se torna muito mais simples.

- **Manutenção**: Com o código centralizado nas classes as manutenções e alterações para novos comportamentos são pontuais.

## Os 4 pilares

A programação orientada a objetos pode ser definida por quatro pilares principais, sendo eles herança, encapsulamento, abstração e polimorfismo.

- **Encapsulamento**: Agrupar variável e funções em pequenas e reutilizáveis partes do código.

- **Abstração**: Reduzir a exposição dos objetos escondendo os detalhes da implementação e expondo somente o necessário

- **Herança**: Reaproveitar o código, para trazer características de um objeto pai para um filho.

- **Polimorfismo**: Reutilizar a mesma propriedade do objeto pai, porém com algumas alterações.

### Abstração

Abstração é um conceito que oculta os detalhes da implementação e exibe apenas a funcionalidade para o usuário.

Dessa forma, reduz a complexidade do código simplificando e focando no que realmente é importante para a aplicação específica mostrando somente aquilo que é relevante necessário.

Exemplo na vida real: Ao clicar no interruptor você quer que a luz acenda ou apaga, não precisa saber o caminho da energia elétrica para acontecer essa ação, portanto isso é uma abstração.

### Encapsulamento

É um princípio que consiste principalmente em agrupar dados (variáveis e métodos) que fazem sentido estar juntos e também em ocultar os detalhes de implementação de um componente dentro de uma classe, expondo apenas operações seguras e que o mantenha em um estado consistente.

#### Tipos de encapsulamento

- **Público:** Indica que todos as outras classes tem acesso a esse atributo, função ou classe.

- **Privado:** Indica que somente a classe que foram declarados tem acesso ao atributo, função ou subclasse.

- **Protegido:** Indica que somente a própria classe ou classes herdadas (subclasses) dela que podem ser acessadas.

#### Getters, setters e construtores

Em C# os dados do objeto são expostos por meio de getter e setters de propriedades.

- **Getter:** Método “GET”, significa “pegar”, ou seja, obter o valor da variável.

- **Setter:** Método “SET”, altera a variável para novos valores.

Para garantir a obrigatoriedade de que o objeto receba dados dependências no momento de sua instanciação devemos util zar o construtor.

- **Construtor:** Método de construção de atributos necessários quando o Objeto é instanciado.

#### Ordem sugerida para implementação de membros

1. Atributos privados
2. Propriedades auto-implementadas
3. Construtores
4. Propriedades customizadas
5. Métodos da classe

Confira um exemplo utilizando os conceitos de abstração e encapsulamento na prática em C#:

![POO - Encapsulation](https://almeidamatheus.netlify.app/uploads/21/04/poo1.png)

A ideia do programa acima é criar alguns atributos privados para serem utilizados somente na classe posto e 2 propriedades auto-implementadas com a função get pública e o set privado, ou seja, fora da classe é possível obter os valores mas não altera-los.

Existe um construtor da classe, então quando for criar um objeto da classe deve-se se passar o parâmetro exigido, que no caso é o `proprietario` do carro.

Temos uma propriedade customizada (`Modelo`), que retorna o `modelo` e também modifica o valor do `modelo`, desde que satisfação a condição estipulada (diferente de nulo e maior que 1 caractere).

Temos também a função `Abastecer` que recebe como parâmetro o valor, e esse valor utilizamos para calcular quantos litros de tanque será enchido, considerando que o valor do litro é 2 reais.

Vale ressaltar que toda vez que essa função é invocada, é acrescentado mais um no valor do membro estático `VeiculosAbastecidos`, como trata-se de uma operação estática, sempre que obter o seu valor, ele irá sempre ter o mesmo resultado independente de objeto, já que não é atrelado especificamente a um objeto.

### Herança

Herança é um conceito que possibilita uma classe herda as propriedades e métodos de outra classe, dessa forma você pode extender e criar variações de uma classe semelhante e relacionada, sendo assim evita a duplicação de código.

A ideia é a classe derivada "filha" possa herdar propriedades e métodos de uma classe "pai", dessa forma você pode extender, criar variações, fazendo assim melhor reuso do código.

Imagine exista uma classe chamada `Veiculo` que contém várias informações como `motor`, `rodas`, `airbag`. Supondo que você queira cadastrar um carro de auto escola, ao invés de criar uma classe com todos os elementos de um veiculo, é mais interessante herdar as características em comum da classe `Veiculo` e incrementar novas informações, dessa forma aplica o conceito herança de uma forma eficaz melhorando a legibilidade e evitando a duplicação de características comuns entre as classes. Nesse caso a classe `Veiculo` e a classe `CarroAutoEscola` são entidades com relacionamento pai e filho respectivamente.

Exemplo utilizando conceitos de herança na prática:

![POO - Inheritance](https://almeidamatheus.netlify.app/uploads/21/04/poo2.png)

Sintaxe:
- : (estende)
- base (referência para a superclasse)

### Polimorfismo

É uma funcionalidade no qual objetos relacionados se comportam de maneiras distinta em razão da capacidade de invocar métodos comuns que possuem comportamento específicos para cada tipo do objeto.Portanto podemos invocar métodos comuns entre os objetos, onde cada objeto possuirá um comportamento diferente.

O C# faz uso de método virtuais (com a palavra-chave virtual) que podem ser reimplementados (com a palavra-chave override) nas classes filhas.

Exemplo utilizando conceitos de polimorfismo na prática:

![POO - Polymorphism](https://almeidamatheus.netlify.app/uploads/21/04/poo3.png)

A ideia do programa acima é criar uma classe `Carreta` que tem como base a classe `Veiculo`, nela temos a adição de um atributo (`Carroceria`) e tem a modificação da método `Rodas`, este retorna o valor retornado pela classe base com a adição de 4.

Sintaxe:

- base (reaproveitar a operação da superclasse e adicionar algo).

- virtual (prefixo que indica que valor que será sobrescrito).

- override (prefixo que indica que esse valor irá sobrepor o virtual).