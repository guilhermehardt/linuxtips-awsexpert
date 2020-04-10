## AWS Expert

Exercício sobre Cloudformation do curso AWS Expert da LinuxTips

## Passos

Com o seu usuário administrador, acesse o console AWS:   
- No serviço EC2, localize a opção Key Pairs no menu lateral esquerdo e crie a sua key pair   
- No serviço CloudFormation, crie uma nova Stack utilizando o template s3-cf-templates.yaml presente neste diretório   

Exporte as credenciais do seu usuário administrador:
```bash
# Credenciais do Administrator
$ export AWS_ACCESS_KEY_ID={}
$ export AWS_SECRET_ACCESS_KEY={}
$ export AWS_DEFAULT_REGION=us-east-2
# Exporte seu IP public
export SSH_SOURCE_IP=$(curl ifconfig.me)
# Exporte o nome da sua Key Pair
export KEY_PAIR_NAME={O NOME DA SUA KEYPAIR AQUI}
```
Copie os templates para um bucket S3
```bash
$ aws s3 cp . s3://s3-cloudformation-recipes-giropops-2020 --exclude="*" --include="*.yaml" --recursive
```
Crie as stacks cloudformation
```bash
# IAM Operations user
$ aws cloudformation create-stack \
  --template-url \
  "https://s3.amazonaws.com/s3-cloudformation-recipes-giropops-2020/iam-operations.yaml" --stack-name "giropops-iam-operations" \
  --capabilities CAPABILITY_NAMED_IAM
# AWS Config
$ aws cloudformation create-stack \
  --template-url \
  "https://s3.amazonaws.com/s3-cloudformation-recipes-giropops-2020/aws-config.yaml" --stack-name "giropops-aws-config" \
  --capabilities CAPABILITY_NAMED_IAM
# CloudTrail
$ aws cloudformation create-stack \
  --template-url \
  "https://s3.amazonaws.com/s3-cloudformation-recipes-giropops-2020/cloudtrail.yaml" --stack-name "giropops-aws-cloudtrail" \
  --capabilities CAPABILITY_NAMED_IAM
# VPC
$ aws cloudformation create-stack \
  --template-url \
  "https://s3.amazonaws.com/s3-cloudformation-recipes-giropops-2020/vpc.yaml" --stack-name "giropops-vpc" \
  --capabilities CAPABILITY_NAMED_IAM
# EC2
aws cloudformation create-stack \
  --template-url \
  "https://s3.amazonaws.com/s3-cloudformation-recipes-giropops-2020/vpc.yaml" --stack-name "giropops-ec2" \
  --parameter ParameterKey=SSHSourceIP,ParameterValue="${SSH_SOURCE_IP}/32" \
  --parameter ParameterKey=KeyPairName,ParameterValue=${KEY_PAIR_NAME} \
  --capabilities CAPABILITY_NAMED_IAM
```