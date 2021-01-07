# VPC (Virtual Private Cloud)

- Provisiona uma seção lógica virtual na Aws Cloud
- Virtual network

## Principais componentes

- Internet Gateway (IGW)
- Virtual Private Gateway (VPN Gateway)
- Routing tables
- Network Access Control Lists (ACLs) - Stateless
- Security Groups (SG) Stateful
- Private/Public subnets
- NAT Gateway (não suportam IPv6 traffic)
- Customer Gateway
- VPC Endpoints
- VPC Peering

## Key features

- VPC são específicas para região e não podem cobrir multi regions
- Pode ter até 5 VPCs por região
- Toda região tem uma VPC padrão
- Uma VPC pode ter no máximo 200 subnets
- Podem ser configuradas com IPv4 CIDR Blocks ou IPv6 CIDR Blocks
- Sem custo: VPCs, route tables, NACLs, Internet Gateway, Security groups, Subnets e VPC Peering
- Com custo: NAT Gateway, VPN Gateway, VPC Endpoint (Interface Endpoint) e Customer Gateway
- Podem ser configurados DNS hostnames (para instâncias também)

## Default VPC

Quando criada tem:

- Tamanho /16 IPv4 CIDR Block (172.31.0.0/16)
- Uma subnet em cada AZ com tamanho /20
- Um Internet gateway padrão
- Um security group padrão
- Um NACL padrão
- Opões DHPC padrão

### IP 0.0.0.0

- Representa todos os IPs possíveis
- Quando especificado em uma IGW está permitindo acesso da internet
- Quando especificado em um SG inbound rules está permitindo todo tráfego da internet em nossos recursos públicos

## VPC Peering

- Conecta uma VPC a outra usando um **IP privado**
- Instâncias em peered VPC se comportam como se estivessem na mesma rede
- Peering usa uma Star Configuration: 1 VPC central conectada a outras 4
- Não há transitividade entre VPCs ou seja
- Se A conectado a B, e B conectado a C, A não conhece C
- Não há sobreposição de CIDR blocks
- Pode parear com VPCs em outras regiões e de outras contas AWS (sucesso)

## Route Tables

- São usadas para determinar onde o tráfego de rede será direcionado
- Cada subnet tem uma Route table associada
- Uma subnet só pode estar associada a uma Route Table mas
- Uma Route table pode estar associada a multilpas subnets
- Por padrão toda subnet criada é atribuida à Route table padrão
- Um cuidado a se tomar é, se tornar a route table padrão publica, então todas subnets serão publicas
- Uma boa prática é manter a route table padrão privada
- Ex de fluxo: Internet -> IGW -> Router -> Route Table -> Public subnet

## Internet Gateway (IGW)

- Em resumo conecta o trafego da internet para sua instância e vice versa
- Se quer que todos os seus recursos permangem privado, não anexe uma IGW
- Quando atribuido um IP publico para uma instância, ela não tem conhecimento disso
- Somente o IGW conhece os IPs públicos
- Quando uma requisição vem da internet, primeiro passa pela IGW e ela direciona o tráfego
- Quando uma instância quer acessar a internet, passa pela IGW que "cria" um ip publico e depois direciona à internet
- É possível ter apenas uma IGW por VPC

## Bastions / Jumpbox

- Sâo instâncias EC2 com propósito de segurança
- Servem de intermediarias para acessar instâncias privadas
- Se quer acessar via SSH ou RDP, o acesso da internet primeiro passa na instância Bastion
- E a instância Bastion acessa a instância privada
- Por isso se também é conhecida como Jumpbox, pois pula de uma instância para outra
- São diferentes de NAT Gateway/Instances
- Bastions são de tráfego de fora para dentro
- NAT Gateway/Instances são de tráfego de dentro para fora (ex: fazer atualizações famoso apt update)
- Bastions é famoso por permitir acesso de um único IP de conexão
- Em resumo previne ataques externos que tentem acessar instancias privadas

## DirectConnect

- Estabelece uma rede dedicada entre recursos on-premise e a AWS
- Reduz custos e aumenta a banda
- Disponibiliza uma experiencia consistente de rede (alta disponibilidade e confiabilidade)
- Fluxo: On-prem router -> dedicated line -> your own cage / DMZ -> cross connect line -> AWS Direct Connect Router -> AWS backbone -> AWS Cloud
- Em resumo conectar recursos on-premise a VPC da AWS via um túnel não publico

## VPC Endpoints

- Em resumo permite conectar sua instâncias à outras serviços da AWS sem sair do ecosistema AWS (não sai para a internet)
- Ou seja, não precisa de Internet Gateway, NAT device, VPN ou DirectConnect
- Comunicação segura entre serviços
- É escalado horizontalmente tendo alta disponibilidade

### Interface Endpoint

- Provisiona uma Elastic Network Interface ENI (uma placa de rede) com IP privado
- Serve como uma entrada e saída para o tráfego que vai e vem para outros serviços AWS
- Usa um registro DNS para direcionar seu tráfego para o endereço IP privado da interface
- É pago por AZ e por dado processado
- Suporta vários serviços (API Gateway, Kinesis, SQS, SNS, CodeBuild, CloudWatch, KMS, Endpoints services em outras contas e mais)

### Gateway Endpoint

- É um gateway que direciona para uma rota específica na route table
- Destinado para trafegar para o serviço AWS especificado
- Precisa especificar em qual VPC será criado o endpoint e qual serviço será o alvo
- Suporta somente S3 e DynamoDB
- Não paga nada

## VPC Flow Logs

- Captura informações de rede de entrada e saída em
- Uma VPC, subnet ou Network Interface (de instâncias EC2 eth01, eth02, etc)
- Captura dados de tráfego válido e não valido
- **Captura** source IP, destination IP, packet size, qualquer coisa que pode ser observado de fora do packet
- **Não captura** query requests, DHCP traffic e Query requests para AWS DNS server
- O log pode ser salvo no CloudWatch Logs ou em um bucket S3
- Captura dados entre VPCs **pareadas na mesma conta**
- Não pode ser tageado
- Não pode ser alterado. Tem que criar um novo (tá facil pra ninguem)

## NACLs - Network Access Control Lists

- Funciona como um firewall virtual no nível de subnet
- VPCs automaticamente usam a NACLs padrão
- Sâo avaliadas antes de Security Groups
- A NACLs padrão permite todos outbound e inbound traffics
- Uma custom NACLs proibe todo o trafega por padrão
- Constituido por um conjunto de regras que
- Permitem ou proíbem acessos de entrada e saída
- Security group só tem regra para permitir
- Cada regra tem um "Role" que funciona como prioridade, ou a ordem de preferência
- É lida de forma crescente
- Subnets podem pertecem somente a uma NACLs
- Pode bloquear IPs específicos
- Com security group não é possível, somente portas
- São Stateless, ou seja, uma regra de inbound permitida também permite uma de outbound
- Pode impactar sobre como as instâncias EC2 em uma subnet privada se comunicarão com qualquer serviço, incluindo VPC Endpoints

## Security Groups

- Camada de firewall a nivel de instância
- São acossiadas à instancias EC2
- Todo o tráfego é negado por padrão
- Só há regras para permitir tráfego, não há de proibição
- Pode especificar a porta, protocolo e IP
- Agem sobre multiplas subnets
- Podem agir sobre mútilplas instancias EC2
- Instancias EC2 podem ter mais de um security group
- A source pode ser outro security group
- Pode ter até 10.000 em uma região
- Cada security group pode ter 60 inbound/outbound rules
- Pode anexar até 16 security groups por ENI

| NACL | Security Group |
| ------------- | ------------- |
| Opera no nível de subnet | Opera no nível de instância |
| Suporta regras de permissão e regras de negação | Suporta regras de permissão apenas |
| Stateless: o tráfego de outbound deve ser explicitamente permitido pelas regras | Stateful: o tráfego de retorno é permitido automaticamente, independentemente de quaisquer regras |
| As regras são processadas em ordem, começando com a regra de menor número | Avalia todas as regras antes de decidir se o tráfego é permitido |
| Aplica-se automaticamente a todas as instâncias nas subnets às quais está associado (portanto, fornece uma camada adicional de defesa se as regras do secutiry group forem muito permissivas) | Aplica-se a uma instância apenas se alguém especificar o security group ao iniciar a instância ou associar posteriormente |

## NAT (Network Address Translation)

- Mapea endereços IPs (de privado para público normalmente)
- Se você tem instâncias privadas e precisa acessar a internet, você pode usar um NAT
- Se tem redes com conflitos de endereço, pode usar NAT

### NAT Instances

- Deprecado
- São instâncias EC2 individuais
- Não tem alta disponibilidade
- Não são tolerante à falhas
- São um ponto de falha na VPC
- Não tem auto-scaling

### NAT Gateway

- Serviço gerenciado pela propria AWS
- Composto de múltiplas instâncias
- Atuam dentro de uma AZ
- Pode ter no máximo uma por AZ
- Tem auto-scaling por padrão
- Começa com 5Gbps e escala até 45Gbps
- Tem atribuição a IPs publicos automaticamente
- Route tables para NAT Gateway devem ser atualizadas
- Se recursos em multiplas AZs compartilham o Gateway e ele cair, todos irão perder acesso à internet
- A não ser que crie um Gateway em cada AZ e configure as route tables
