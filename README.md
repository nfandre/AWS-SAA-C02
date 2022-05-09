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
## Virtual private cloud (VPC)
    default VPC X Custom VPC
Conceitos importantes:
- VPC PEERING - conectar VPC com outra VPC
- No TRANSIT entre VPCs: permite apenas conexão direta
- ACL -> statefull: permite regras de permissão e de bloqueio
- SEC GROUPS -> stateless: apenas permite (bloqueia o restante)


## CLI - Commands
aws configure
aws iam list-users
aws s3 ls

aws s3 cp --recursive s3://andrefreitasoriginal s3://andrefreitasaws
