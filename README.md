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

#### User data EC2 - cria instância e executa script somente a primeira vez que máquina iniciar

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

## AWS WAF
Protects against DDoS Attacks and malicious Web Traffic.
- Atua na camanda 7 de aplicação
- Filtrar tráficos de origens (ex: países\)

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
- Nat gateway: como um aplicação, mais confiavel
## CLI - Commands
aws configure
aws iam list-users
aws s3 ls

aws s3 cp --recursive s3://andrefreitasoriginal s3://andrefreitasaws
