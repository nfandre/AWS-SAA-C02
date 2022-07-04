# AWS-SAA-C02

## Identity and Access Management (IAM)

Tipos de usuários:
 - com acesso a console AWS
 - não precisa acessar a console AWS, acessa de forma programática (AWS CLI). Necessário duas informações:
    - access key
    - security key
    
## ARCHITECT - Amazon Simple Storage Service (Amazon S3)
- Tiers
- Lifecycle
- cloud front (CDN - Content delivery network)
- Storage gateway - software para replicar dados de um data center em um bucket
   > https://docs.aws.amazon.com/pt_br/storagegateway/latest/userguide/StorageGatewayConcepts.html
  - File gateway (pictures, fotos, vídeos, docs) NFS
     - S3 Standard, Infrequent Access, Glacier
  - Volume gateway (OS, VM's) ISCSI - > snapshots
    - Arquitetura de volumes armazenados em cache
    - Arquitetura de volumes armazenados
  - Tape gateway (Backup de software vtl ) virtual VTL -> Glacier
- Snowball
  - Snowlball - dispositivo para fazer opload do armazenamento da empresa para Amazon s3
  - Snowball edge  - contém o um sistema EC2
  - Snow Mobile - 100 PB "caminhão container"
- S3 Transfer acceleration 
  - upload de arquivos a partir de edge location (cliente server)
- RRS - Reduced Redondancy Storage
  - armazena tipos de arquivos equivalentes thumbnail
  

## Elastic Public Cloud (EC2)

#### EBS - Elastic Block Store: tipos de volumes que permiete aptmizar o armazenamento

*Não é possível conectar um único EBS a duas instâncias EC2 ao mesmo tempo

- Tamanho fixo alocado
- Tipos de discos:
  - SSD
  - HDD
- Criar volumes
  - EC2 > Elastic Block Store > volumos > criar
- Snapshots
  - EC2 > Elastic Block Store > Snapshots > criar
  - EC2 > Elastic Block Store > Snapshots > create image 
- Lifecycle Manager - cria políticas para ciclos de backups

#### EFS - Elastic File System
- É possível conectar um EFS a uma ou mais instâncias EC2 ao mesmo tempo.
- Tamanho variável

#### User data EC2
cria instância e executa script somente a primeira vez que máquina iniciar

#### Estado de uma instância:
-  running
- stopped
- hibernate behavior (precisa habilitar na criação da instância)
  - volume EBS precisa ser encriptado 

#### AMI - Amazon Machine Image
Imagem virtual

#### EC2  Global Viel:
- Visualização geral de todas as máquinas em todas as Regions.

#### AWS Marketplace:
- Empresas ou pessoas físicas disponibilizam software no marketplace em instâncias EC2

#### Security Groups - Virtual firewall
- inbounds x outbounds
- stateful

#### Acessing Services - Acess Keys and IAM Roles
- Access Keys (não é o mais indicado por segurança): é configurado na instância EC2, é uma chave de acesso armazenada no sismtema de arquivo da instância.
- Intances Profiles (IAM Roles): A instância tem uma IAM Role associada, que contém as Policy de acesso.

#### EC2 Placement Groups
- cluster: packs instances close together inside an Availability zone. This strategy enables workloads to achieve the low-latency network performance necessary for tightly-coupled node-to-node communication that is typlical of HPC Applications
- Partition: spreads your instances across logical partition such that group of instances in one partition do not share de underlying hardware with groups of instances in diefferente partitions. This strategy is typically used by large distributed and replicated worloads, such as Hadoop, Cassandra, and Kafka
- spread: strictly  places a small group of instances across distinct underlying hardware to reduce correlated failures

#### ENIs Elastic Network Iterfaces
- ENI Elastic network interface: Basic adapter type when don't have any high-performance requirements, all type instances.
- ENA Elastic network adapter: Enhanced networking performance, High bandwith and lower inter-instance latency, choose supported instance.
- EFA Elastic fabric adapter: High Performance computing nad MPI and ML use cases, Tightly coupled applications, all type instances.
> EC2 > Elastic IP > Allocate
#### Public, Private and Elastic IP Addresses
- Public IP: is a dynamic address, cannot be remapped acrooss AZs
- Elastic IP: is a static address, Can be remapped acrooss AZs
- Private IP: Used in public and Private Subnets, retained when instance is stopped

#### Bastion Hosts
É uma forma de conectar em máquinas EC2 com ip privado dentro de uma VPC, para isso é usado uma instância com ip público com saída internet gateway. A conxão se dá acessando a instância pública que irá acessar a instância privada através do ip privado.

#### Amazon EC2 Princing Use Cases
- On-Demand: Small project for several hours, cannot be interrupted.
- Reserved: Steady-state, business critical, line-of-business apliccation, conitnuous demand.
- Scheduled Reserved: Reporting application, runs for 6 hours a day, 4 days per week.
- Spot Instances: Compute-intensive, const-sensitive distrubted computing, can withstand interruption.
- Dedicated Intances: Security-sensitive application, requires dedicated hardware; per-instance billing.
- Dedicated Hosts: Database with per-socker licensing.

#### AWS Nitro Sytem - Nitro Instances and Nitro Enclaves
Is the underlying plataform for the next generation of EC2 instances
- bare metal and virtualized instances
- Especialized hardware
- improves performance, security and innovation 

AWS Nitro Enclaves
- Isolated compute environments
- no persisitente storage, interactive access



## Elastic Load Balancing and Auto Scaling
### Elasticity: Escaling Up vc Out
- Scaling Up (Vertical scaling): means adding resources to the instance, add more hardware.
  - Limitation is that it has a single point of failure (SPOF)

Scaling Out (Horizontal scaling): add another instance of application, provides greater resiliency, can be used to add almost unlimited capacity.

### Amazon EC2 Auto Scaling
Escalonamento
- Horizontal

Escala baseada em:
- CPU
- FILA
- SQS
- Rede
- Mem
- etc..

Suporta quais apps?
- Os dados não podem ficar no servidor
- segmentar para outros serviços (RDS, EFS etc)

Como Rotear o acesso?
- Utilizando ELB, balanceador de carga
  *Custo zero
  
#### Configuração:
- Launch Template: Specifies the EC2 instance configuration
- Launch Config: are replaced by launch templates an have fewer features

####  Health checks
- EC2: EC2 status checks
- ELB: Use the ElB health checks int addition to EC2 status checks
- Health check grace period
  - how long to wait berore checking the health status of the instance
  - Auto scaling does not act on heath checks unitl grace period expires

#### Monitoring
- Group metrics (ASG):
  - Data pints about the auto scaling group
  - Must be enabled
  - no charge
  - 1-minute granularity
- Basic monitoring (Instances): instances Sending metrics to CloudWatch
  - 5-minute granularity
  - no charge
- Detailed monitoring (Instances)
  - 1-minute granularity
  - Charges apply (it have to pay)
  

#### Addtional Scaling Settings
- Cooldowns: Used with simple scaling polici to prevent Auto scaling from laucnhing or terminating before effects of previus activities are visible.
  - Default value is 300 seconds (5 minutos)
- Termination Policy: Controls which instances to terminate first when a scale-in event occurs.
- Termination Protection: Prevents Auto scaling from terminating protected instances.
- Standby state - Used to put an instance in the InService state into the Standby state, pudate ou trobleshoot the instance
- Lifecycle Hooks - Used to  custom actions by pausing instances as the ASG launches or terminates them
 - Use case: run a script to download and install software after launching

### ELB - Elastic Load Balancer
Balanceador de carga
- Targets:
  - Instâncias
  - Container
  - Lambda

- Integra com Cert. Manager
- Faz Health Check

#### Types of ELB
- ##### ALB Application Load Balancer
  Layer 7, nível de aplicação precisa requests
  - Target Type: IP, Instance EC2, Lambda, ECS
  - HTTP, HTTPS
  - No Static IP address
  - HTTP header based
  - No privateLink support
  - Routing: 
    Are used to route requests to registered targets, it can be EC2 intances, IP addresses, lambda functions or containers
    > - requests can be routed based on the path in the URL
    > - Path-based routing: /example /public
    > - host-based routing: example.curso.com; public.curso.com

- ##### NLB Network Load Balancer:
  Layer 4, camada de rede, high perfomance and veey low latency, TLS
  - Target Type: IP, Instance EC2
  - TCP, UDP, TLS
  - Static IP address
  - No HTTP Header based
  - PrivateLink support (TCP, TLS)
  - nodes routing: Targets can be EC2 instance or  IP addresses (can be outside a VPC a VPC - E.G. on-premises)
    >  - NLB nodes can have elastic IPs in each subnet
    >  - a separete listener on a unique port is required for routing: example:8080
    >  - requests are routed based on IP protocol data
  
- ##### CLB Classic Load Balancer: 
  Old generation; not recommened for new applications, layer 7 and 4.
- ##### GLB Gateway Load Balancer: 
  Layer 3 listens packets on all ports, used in front of virtual appliances such as firewalls, IDS/IPS


## AWS WAF
Protects against DDoS Attacks and malicious Web Traffic.
- Atua na camanda 7 de aplicação
- Filtrar tráficos de origens (ex: países\)

## ECS
Serviço de orquestração de containers
- Cluster EC2 ou Cluster AWS Fargate

Integra com:
- EC2
- IAM
- SM Security manager
- CW Cloud Watch
- ELB
- CI Devops

Task definitions ( Usado para configurar porta, autoscaling, ELB), pode ser feito com YAML.

## AWS Config
Dedo duro, permite avaliar(auditar) recursos para garantir conformidade e seguir diretrizes.
- Monitoramento contínuo
- Histórico de tudo
- CloudTrail amigável
- Escopo Regional
- Custom checks

## AWS Cloud Trail
- Trilhas de logs
- Possibilita automação
- tudo, tudo e tudo
- Regional (mas da para gerar global)
- Integra com Cloud Watch (log, metrics, alert)

## AWS Database
Relacional (RDS - Relational database service)
- Mulit AZ -> ex: Disaster Recovery
  - ex: Duas DB idênticas em duas ou mais AZ, mesmo DNS
  
        Amazon RDS > databases > modify > multi-AZ deployment

- Read Replica -> cópia idêntica
  - Duas DB com DNS diferente, inserção manual na réplica
  - backup enabled
  
        Amazon RDS > databases > actions > create read replica

Backups:
- Automated: 1 - 35 dias - > S3 (free)
  - Amazon RDS > databases > modify > backup
- DB snapshot: manual 
  - Amazon RDS > actions > take snapshot --> snapshots
      
#### Dynamo DB
- Latência baixa (milisegundos)
- Documentos -> key value
- APP Web/ mobile/ games
- SSD
- Armazenamento em 3 ou mais data centers
- Consistent Reads x Strongly consistent reads
  - Consistent Reads: Write/read maior que 1 segundo
  - Strongly consistent reads: Write/read menor que 1 segundo
  
#### Elasticache
- Rápido, latência baixa (criação, operação e escalabidade)
- Não utiliza disco
- Armazenamento em processamento e memória (in-memory cache)
- Ex: Web site e-commerce(acessos simultâneos ao mesmo produtos)
- Tipos:
  - Memory cached: armazena no bd relacionado a objetos
  - Redis: key-value and Multi-AZ
  
#### Redshift
- Warehouse (armazem) ex: Toyota, Amazon
- Armazenamento em colunas (leitura em colunas)
- 1 datbase inicia com 160GB
- Compressão de dados
- Tipos:
  - Single mode -> 1 instância DB
  - Compute mode -> 128 instâncias de DB
- MPP: Massively Parallel Processing -> Leitura em várias DB ao mesmo tempo  
- Não é Multi-AZ

#### Aurora
- criada 2014
- compatibilidade com mysql oracle
- 5x Faster
- 10x barata
- Default 10gb -> autoscaling automatico (646GB) -> 64TB
- criação de replicas (read) x15
- Recover -> Point in-time 
- Backup continuo -> 3 zonas


## Route53
Serviço AWS para AWS, responsável pelas resoluções de endereçamento IP.
- Redundância de localização: replicado para todas locations
- 100% SLA (availabilty)
- GEO Location, Failover 

#### DNS Records (Registros)
- HOST A (A -IPV4 ou AAAA - IPV6): amazon.com -> 1.1.1.1
- Alias (Cname): *cursos*.amazon.com -> 1.1.1.2
- Mail exchange (MX): Email (server), utiliza prioridade 
- Service Record (SRV): qual serviço está rodando? target(IP)? porta? Prioridade?
- Start of Authority(SOA): Primary NS
- Name Server (NS): Armazena Start of Authority
- POinter (PTR): Constrário do Host, dado um IP converte no Domínio DNS

### Routing
Health check
- #### Simple Routing
  Uma requisição de DNS para cada servidor, envia randomicamente o endereçamento IP para o HOST

- #### Failover Routing
  Quando um servidor falhar redirecionar para outro

- #### Geolocation Routing
  Rotear pra uma Region a partir de uma localização

- #### Geoproximity Routing(Traffic Flow Only)
  Traffic flow: idêntifica onde o tráfico está passando aplicando políticas para esse tráfico, baseado em aproximação de geolocation

- #### Latency-based Routing
  Redirecionamento feito a partir da menor latência, acessando os servidores mais próximos

- #### Multivalue Answer Routing
  Simple check with Health check

- #### Weighted Routing
  - Peso de tráfego que está sendo enviado para cada servidor

## Virtual private cloud (VPC)
    default VPC X Custom VPC
Conceitos importantes:
- VPC PEERING - conectar VPC com outra VPC
- No TRANSIT entre VPCs: permite apenas conexão direta
- ACL -> stateless - Deny && Allow
- SEC GROUPS -> stateful - apenas Allow
> https://medium.com/awesome-cloud/aws-difference-between-security-groups-and-network-acls-adc632ea29ae#:~:text=Security%20groups%20are%20tied%20to,assigned%20explicitly%20to%20the%20instance.

Fluxo até internet Gateway
> Subnet > ACL > Routing Table > Router > Internet Gateway

### NAT - Network address translation
Roteador que tem um enderaçamento privado e um endereço público, serve para transcrever um ip privado para um ip público de forma que este tenha acesso a rede.

- Nat instance: dispositivo criado como um servidor EC2, sem escalabilidade
  > Cria uma instância EC2 do tipo Nat que será associada a uma rota de saída para uma route table.
  > requires the source and destination checks to be disabled
- Nat gateway: como um aplicação, mais confiavel
 > VPC > Nat gateway - Create
> Nat gateway é criado em uma subnet publica
### ACL
- In bound
- Out bound

### VPC Flow Logs
VPC > actions > create flow log

### Bations Hosts (Jump Box)
Servido que fica dentro da tupologia, sendo o único que tenha acesso a internet. É através desse host que será acessado as outras instâncias da rede.

### Direct Connect
Roteadores da Amazon que conectam diretamente com a nuvem da AWS e fazem conexão diretamente com uma provedora de telecom (internet).

### VPC EndPoint
Cria uma ponte entre a VPC e os serviços AWS, ou seja, uma máquina dentro de uma VPC sem acesso a internet consegue acessar os recursos da AWS.
- Interface endpoint
- Gateway endpoint
> VPC > Endpoint > create

## Architect - Aplicações AWS

### SQS - Simple Queue Service 
Serviço simples de fila, sistema de mensagens que são armazenados em um aplicativo, geralmente máquinas EC2.
- Standard: Não possui ordem de envio, não é em sequência e pode ser duplicada.
- FIFO: Garantia de entrega em sequência
> https://docs.aws.amazon.com/pt_br/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html
- #### Pull base
  É solicitado informações para o SQS, e o mesmo retorna quem está na fila.

### Simple Workflow Service
Conjunto de tarefas que devem ser executadas em uma ordem específica. Gerencia o workflow tasks de um sistema.
- Pode se extender até 10000 mil tarefas
- Faz por volta de 1000 por segundo

### Elastic Transcoder
Converte mídias para formatos específicos (MP4,MP3,HD).

### API Gateway
Faz um serviço de "Load Balancer" para aplicações, portão de entrada para softwares que acessam de forma programática os serviços da AWS.

### Kinesis
Responsável por coletar, armazenar e analizar dados de streaming para serem analisados ou utilizados por outra aplicações. Armazena em shad(partes).
- Streams: recebe os dados e armazena para que depois outros serviços possam consumi-los.
- Firehose: recebe os dados, mas não armazena, apenas processa ou deixa que outro serviço o faça.
- Analytics: faz a análise dentro do Streams e do Firehose.

### Cognito
Web Identity Federation, permitir ou bloquear acesso de acordo com uma credencial que não está armazenada na aplicação.
- User Pool: Retorna autenticação do User.
- Identity Pool: Retorna o que pode ou não pode acessar

### SNS - Simple Notification Service
Sistema de notificação
- Push
- SMS
- Text Message
- Email
- HTTP

## Serverless Lambda 
- "Sem servidor"
- Scaling por conta da AWS
- Pricing:
  - 1 Milion Requests = FREE
  - 0,20R$ por cada 1M de requisições
  > https://aws.amazon.com/pt/lambda/pricing/
  
## Cloud Formation
Cria a partir de códigos (template) as configurações de serviços na AWS (EC2, S3 etc)
- Benefícios:
  - INFRA baseada em códigos
    - Controle e gerenciamento
    - Versões
    - Visualizar mudanças antes que elas sejam aplicadas
  - Custo
    - TAG.
    - Custo estimado
    - Sistema de adição e remoção de serviços
## CLI - Commands
aws configure

aws iam list-users

aws s3 ls

aws s3 cp --recursive s3://andrefreitasoriginal s3://andrefreitasaws

aws s3 ls --region us-east-1
