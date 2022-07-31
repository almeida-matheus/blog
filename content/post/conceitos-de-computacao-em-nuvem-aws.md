+++
title = "Conceitos de computação em nuvem"
date = 2022-01-31T00:47:11-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Conceitos de computação em nuvem na AWS"
categoria = [
    "Nuvem",
]
+++

# O que é computação em nuvem

A computação em nuvem é a entrega de recursos de computação por meio de serviços que são acessados de maneira remota (*online*).

Esses recursos de computação incluem armazenamento, poder de processamento, bancos de dados, redes, análises, inteligência artificial, aplicativos de software, dentre outros.

Os serviços de nuvem estão em um datacenter fora do seu ambiente local (*on-premises*) que respondem às requisições vindas da Internet.

# As Vantangens da computação em nuvem

**Troque as despesas de capital por despesas variáveis:** Em vez de ter de investir em datacenters e servidores antes de saber como vai usá-los, você pode usar a computação em nuvem e pagar somente pelos recursos consumidos.
    - CAPEX (*capital expenditure*) e que diz respeito às despesas ou investimentos em bens de capital. Já OPEX (*operational expenditure*) refere-se às despesas operacionais. Com computação em nuvem você aumenta o OPEX em deterimento do CAPEX já que paga se pelo uso e não pela propriedade do bem físico.
**Beneficie-se de grandes economias de escala:** Quanto mais pessoas utilizam a nuvem, mais os provedores como a AWS podem alcançar maior economia de escala. O que se traduz em preços menores de pagamento conforme o uso.
**Pare de fazer suposições sobre capacidade:** Elimine as suposições ao determinar sua necessidade de capacidade de infraestrutura. Ao tomar uma decisão sobre a capacidade antes da implantação da aplicação, muitas vezes você acaba lidando com a ociosidade de recursos caros ou com limites de capacidade. Com a computação em nuvem você pode acessar o máximo ou o mínimo de capacidade possível, além de aumentar e reduzir a escala na vertical conforme a necessidade, com apenas alguns minutos de aviso prévio.
    - Aumenta ou diminui seus recursos facilmente conforme a demanda.
    - AWS oferece **baixo custo variável**, já que possui o modelo "*pay-as-you-go*", permitindo utilizar os serviços da Nuvem **sem precisar pagar adiantado** ou assinar algum termo de compromisso de uso.
**Aumente a velocidade e a agilidade:** No ambiente de computação em nuvem, novos recursos de TI estão ao alcance com apenas um clique, o que significa que o tempo necessário para disponibilizar esses recursos aos desenvolvedores é reduzido de semanas para apenas minutos. Isso aumenta significativamente a agilidade da organização porque o custo e tempo necessários para experimentar e desenvolver é consideravelmente mais baixo.
    - Menos tempo da equipe é necessário para lançar novas cargas de trabalho. e Maior produtividade para as equipes de desenvolvimento de aplicativos.
**Pare de investir dinheiro em administração e manutenção de datacenters:** Concentre-se em projetos que diferenciam seus negócios, não na infraestrutura. A computação em nuvem permite que você tenha como foco seus próprios clientes, em vez de centrar a atenção no pesado trabalho de montagem em rack, empilhamento e ativação dos servidores.
    - Um **recurso gerenciado** é quando um serviço ou algumas configurações da camada anterior de configuração não é administrada pelo usuário e sim pela própria provedora.
**Torne-se global em minutos:** Implante facilmente sua aplicação em várias regiões ao redor do mundo com apenas alguns cliques. Isso significa que você pode fornecer menor latência e melhor experiência aos seus clientes a um custo mínimo.
    - Você não precisa mais esperar a compra de um novo servidor para melhorar seu desempenho. Na Nuvem você escolhe um serviço, configura e já começa a usar. Enquanto que em um ambiente *on-premises* você precisa aguardar o hardware chegar na sua empresa, na Nuvem ele está literalmente na distância de alguns cliques.

# Conceitos da nuvem

## Escalabilidade

### **Vertical**

Capacidade de um sistema crescer seu escopo, seja em tamanho, capacidade ou poder computacional (desempenho) do recurso para suportar mais cargas.
    - Maior poder.
    - Aumentar ⇒ Scale up e Diminuir ⇒ Scale down.
Exemplos.
    - AWS: Atualizar instâcia t2.micro para t2.large.
    - Vida real: Subir o cargo do atendente.

### **Horizontal  ⇒ Elasticidade**

Elasticidade: A capacidade de um sistema crescer e diminuir com base na demanda.
    - Adicionar quantidade.
        - Com base tempo de uso ou volume de uso (*cores*, armazenamento, *throughput*).
    - Aumentar ⇒ Scale out e Diminuir ⇒ Scale in.
Exemplos.
    - AWS: Aumentar a quantidade da instancia com o AWS Auto Scaling Group.
    - Vida real: Contratar mais atendentes.

## Alta disponibilidade

Executar aplicação em pelo menos 2 zonas de disponibilidade

Reflete no ambiente com alta disponibilidade:

Distribuição **Multi AZ**.
    - AWS disponibiliza nossas instancia em duas ou mais zonas de disponibilidade.
Estratégia de **Disaster Recovery (DR)**.
    - Em caso de desastre natural ou em um incidente, o seu negócio não é afetado.
Tolerância a falhas.
    - Resiliência: A habilidade de um sistema permanecer em funcionamento, mesmo se um dos seus componentes falhar.
Exemplos.
    - AWS: **AWS Load Balancer com multi-az** conforme a demanda e o **Auto Scaling** cria um novo servidor em AZ diferente.
    - Vida real: Ter 2 *call centers* ao invés de 1, caso 1 fique sem energia.

# Modelos de computação em nuvem

Cada tipo de computação em Nuvem oferece diferentes níveis de controle, flexibilidade e
gerenciamento para que você possa selecionar o conjunto certo de serviços para as
suas necessidades. Confira os tipos em ordem crescente de abstração:

### Infraestrutura como serviço (IaaS)

O IaaS contém os componentes básicos da TI na nuvem, oferecendo acesso a recursos de rede, computadores virtuais e armazenamento de dados com alto nível de flexibilidade, gerenciamento e controle sobre os recursos de TI.

Prós:

- Controle total sobre os recursos provisionados. Semelhante a TI tradicional.
- Ideal para *lift-an-shift*. Migrar uma aplicação de *on-premises* para nuvem sem alterar a lógica ou modo de funcionamento.

Contras:

- Necessita de mais gerenciamento e configuração.
- Requer uma equipe maior especializada.

Exemplos:

- EC2 para gerenciar instancias de computadores na nuvem para **hospedar** aplicações.

### Plataforma como serviço (PaaS)

Com o PaaS, você não precisa mais gerenciar a infraestrutura (hardware e sistemas operacionais) e pode manter o foco na implantação e no gerenciamento de aplicativos.

Não é necessário se preocupar com o gerenciamento dos recursos de infraestrutura de tecnologia, permitindo que o foco do cliente seja, principalmente, no **desenvolvimento** das aplicações que vão ser executados sobre os recursos.

Prós:

- Sistema operacional gerenciado.
- Requer menos gerenciamento e manutenção.
- Escalabilidade de forma simples.
- Ambiente de desenvolvimento amigável.

Contras:

- Menor controle sobre os recurso.
- Customizações limitadas.

Exemplos:

- Hospedar aplicações no servidor do ElasticBeenStalk sem se preocupar em instalar servidor web, banco de dados, etc.

### Software como serviço (SaaS)

O SaaS oferece uma aplicação completa, executado e gerenciado pelo provedor de
serviços. Isso implica que o usuário final faz uso de aplicações sem se preocuparem em como foram construídas e implementadas. Diferente dos outros modelos, aqui o usuário não atua sobre a manutenção do serviço ou o gerenciamento da infraestrutura.

Prós:

- Totalmente gerenciado pelo provedor.
- Configuração e implantação rápida.
- Sem gerenciamento e manutenção (somente configuração).

Contras:

- Pouco customizável.
- Não tem controle sobre a infraestrutura.
- Integração com outros sistemas.

Exemplos:

- Adotar uma forma de comunicação em uma plataforma como o Microsoft Teams.
- Ao invés de um servidor de e-mail, usar serviços como Gmail e Outlook.

Modelos de computação em nuvem
:--------------------------------------------------:
![modelos-de-computação-em-nuvem](https://almeidamatheus.netlify.app/uploads/22/01/modelos-de-computacao-em-nuvem.jpg)

# Modelos de implantação de computação em nuvem

### **Nuvem**

Uma aplicação baseada na nuvem é uma aplicação totalmente implantada na nuvem.

Características da nuvem pública:

- Infraestrutura compartilhada entre os clientes.
- Mais barato (paga pelo o que usar).
- Escalabilidade sob demanda.
- Sem manutenção de hardware.

### **On-premises**

Este modelo de implantação é igual à infraestrutura de TI tradicional. A implantação de recursos *on-premises* com o uso de ferramentas de gerenciamento de recursos e virtualização de aplicações para fornecer recursos. Recursos esses que em grande maioria são dedicados, isto é, para uma demanda específica.  

Características da nuvem privada:

- Infraestrutura exclusiva.
- Maior customização.
- Requisitos de compliance.


### **Abordagem híbrida**

Uma implantação híbrida é uma maneira de conectar infraestrutura e aplicações entre recursos baseados na nuvem e recursos existentes que não se encontram na nuvem. O objetivo é estender e aumentar a infraestrutura de uma organização na nuvem e ao mesmo tempo conectar os recursos da nuvem ao sistema interno.

Características nuvem híbrida:

- Combinação de nuvem pública com privada.
- Permite balancear o custo com customização.
- Permite migração gradual para nuvem.
- Utilizado para estender um datacenter *on-premises* (DR e backups).

# Infraestrutura global

Serviços AWS, sejam eles de computação, armazenamento, redes, dentro outros, são suportados por uma infraestrutura que possui a seguinte base:

### Região

Uma região é a disponibilização de uma coleção de recursos AWS em uma localização geográfica, sendo ele composto por um conjunto de zonas de disponibilidade (AZ).

Para escolher uma região você deve se atentar aos seguintes pontos:

- Latência: Quanto mais próximo aos clientes menor será a latência.
- Serviços disponíveis: Nem todos serviços estão disponíveis em todas regiões.
- Custos: Em alguns recursos os custos variam conforme a região.
- Requisitos legais: Dados só podem estar no mesmo país.

Exemplo de região: `us-east-1`.

### Zona de disponibilidade (AZ)

Conjunto de data centers na mesma região separados fisicamente e isoladas, porém conectados para poder responder com baixa latência, alta taxa de rendimento (velocidade) e alta redundância.

Exemplo de região: `us-east-1a`.

### Pontos de presença (PoP)

Também conhecido como Edge Locations (Zona de borda), trata-se de uma infraestrutura de servidores, localizado próxima de uma ZD, que armazena os dados mais solicitados no cache, para entregar com menor latência uma requisição de consulta acelerando a distribuição de conteúdo (CDN).

Serve como cache dos seus dados, entregando requisições de leituras em menor latência quando o cliente não está proximo da região que você disponibilizou o serviço.

Infraestrutura global
:--------------------------------------------------:
![infraestrutura-global](https://almeidamatheus.netlify.app/uploads/22/01/infraestrutura-global.png)

# Modelo de responsabilidade compartilhada

Enquanto a AWS gerencia a segurança **da** nuvem, você é responsável pela segurança **na** nuvem.

Responsabilidade compartilhada
:--------------------------------------------------:
![responsabilidade-compartilhada](https://almeidamatheus.netlify.app/uploads/22/01/responsabilidade-compartilhada.png)

**Responsabilidade da AWS**: A AWS é responsável por proteger a infraestrutura que executa todos os serviços oferecidos na Nuvem AWS. Essa infraestrutura é composta por hardware, software, redes e instalações que executam os Serviços de nuvem AWS.

**Responsabilidade do cliente**: A responsabilidade do cliente será determinada pelos Serviços de nuvem AWS utilizados por ele. Confira alguns exemplos de serviços com as operações de configurações e as suas responsabilidades de segurança:

- Serviços categorizado como *Infrastructure as a Service* (IaaS) que exigem que o cliente execute todas as tarefas necessárias de configuração e gerenciamento da segurança.
    - Amazon Elastic Compute Cloud (Amazon EC2) que . Os clientes que implantam uma instância do EC2 são responsáveis pelo gerenciamento do sistema operacional convidado (o que inclui atualizações e patches de segurança), por qualquer utilitário ou software de aplicativo instalado pelo cliente nas instâncias, bem como pela configuração do firewall disponibilizado pela AWS (chamado de grupo de segurança) em cada instância.
- Serviços abstraídos como o Amazon S3 e o Amazon DynamoDB.
    - Por ser recursos gerenciados, a AWS opera a camada de infraestrutura, o sistema operacional e as plataformas, e os clientes acessam os *endpoints* para armazenar e recuperar dados.
    - Os clientes são responsáveis por gerenciar os dados deles (o que inclui opções de criptografia), classificando os ativos e usando as ferramentas de IAM para aplicar as permissões apropriadas.

**Controles compartilhados**: Controles que se aplicam à camada de infraestrutura e às camadas do cliente. Em um controle compartilhado, a AWS disponibiliza os requisitos de infraestrutura e o cliente deve disponibilizar sua própria implementação de controles dentro do uso de Serviços da AWS. Principais exemplos:

- **Gerenciamento de patches**: A AWS é responsável pela aplicação de patches e pela correção de falhas na infraestrutura, mas os clientes são responsáveis pela aplicação de patches em seu SO convidado e nos seus aplicativos.
- **Gerenciamento de configuração**: A AWS mantém a configuração dos dispositivos de infraestrutura, mas o cliente é responsável pela configuração dos seus próprios bancos de dados, aplicativos e sistemas operacionais convidados.
- **Conhecimentos e treinamento**: A AWS treina funcionários da AWS, mas o cliente deve treinar seus próprios funcionários.