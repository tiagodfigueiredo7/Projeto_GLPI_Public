# Projeto - GPLI 
[![NPM](https://img.shields.io/npm/l/react)](https://github.com/tiagodfigueiredo7/awsterralt/blob/main/LICENCE) 

# Sobre o projeto

> Nesse projeto vamos implementar a ferramenta GLPI, usando AWS, Terraform , e Docker Compose.
O TerraForm será usado para a criação da infra VPC, Ec2, EBS e Security Groups usando data.




#  Infra Cloud

- VPC 
- SUBNET
- IAM
- E2 
- Docker 
- Compose 
- Terraform



##  Projeto 

![Web 1](https://github.com/tiagodfigueiredo7/assets/blob/main/ScreenHunter%20431.png)

 
  



# Tecnologias utilizadas

- Aws 
- Docker
- Compose
- GitHub
- GLPI
- Terraform
- vscode


##  Executar o projeto



- Pré-requisitos: Conta AWS






##  Terraform


> Abra o terminal e usa o comando a baixo:


```
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=

```

- Provider 

```

# Configure the AWS Provider
provider "aws" {
  region = "us-east-1"
}

```
- backend.tf

```
terraform {
  backend "s3" {
    bucket = "grupotfstate"
    key    = "prod"
    region = "us-east-1"
  }
}


```
- vpc.tf


```
data "aws_subnet" "gruporpn" {
     id = "subnet-0bf5a858564489c5c"

}

```
- variables.tf

```
variable "image_id" {                          
  default = "ami-0cff7528ff583bf9a"
  type = string
  description = "essa variavel é um ID de imagem que sera usado no servidor Ec2."
}


variable "key" {
    default = ""                 

}


variable "web" {
  description = ""
  type        = string
  default     = null
}

variable "ssh" {
  description = "sg-016a06030b6b56702"
  type        = string
  default     = null
}

# data para cada Ec2 cair na VPc correta utilizando Subnet! 

```
- securitygroup.tf

```
data "aws_security_group" "portagpli" {
    name = "prod-glpi-web"
    
}

```

- output.tf

```
output "ip_address"{
value = aws_instance.GLPI-Server.*.public_ip            
}

```


- iam.tf

```
resource "aws_iam_policy" "grupo-polyce" {
  name = "grupo-polyce"
  policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "s3:*"
      ],
      "Effect": "Allow",
      "Resource": [ 
         "arn:aws:s3:::",
         "arn:aws:s3:::/*" 
      ] 
    }
  ]
}
EOF
}

resource "aws_iam_role" "grupo_role" {
  name = "gropo-role"

  assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Effect": "Allow",
      "Sid": ""
    }
  ]
}
EOF

}

resource "aws_iam_policy_attachment" "policy_role_attach" {
    name = "grupoterraform"
    policy_arn = aws_iam_policy.grupo-polyce.arn
    roles = [aws_iam_role.grupo_role.name]
}

```

- ec2.ft

```

data "aws_ami" "amazonlinux" {
 most_recent = true


 filter {
   name   = "owner-alias"
   values = ["amazon"]
 }


 filter {
   name   = "name"
   values = ["amzn2-ami-kernel-5*"]
 }
}


resource "aws_instance" "GLPI-Server" {
  ami                         = data.aws_ami.amazonlinux.id                         
  instance_type               = "t2.micro"                                          
  key_name                    = var.key                                            
  vpc_security_group_ids      = [data.aws_security_group.portagpli.id]              
  associate_public_ip_address = "true"                                            
  subnet_id                   = data.aws_subnet.gruporpn.id                        
  root_block_device {
  volume_size = 15
    } 
   
  tags = {
    Name = "GLIP-Server-Terraform"
}
}

resource "aws_eip" "ip-publico" {               
    vpc      = true                             
    instance = aws_instance.GLPI-Server.id      
}

resource "aws_ebs_volume" "ebs1" {
  availability_zone = "us-east-1b"
  size             = 30
  type             = "gp2"
  tags             = {
    Name = "GLPI-Terra-EBS1"
  }
}

resource "aws_volume_attachment" "ebs_att" {
  device_name = "/dev/sdf"
  volume_id   = aws_ebs_volume.ebs1.id
  instance_id = aws_instance.GLPI-Server.id
}
```





# Instalação  Docker ( Linux ) <img align="center" alt="Rafa-Python" height="70" width="80" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original-wordmark.svg">
https://docs.docker.com/desktop/linux/install/

```
yum update
yum install docker 
sudo usermod -aG docker $USER
systemctl status docker
systemctl start docker
systemctl ename docker.service    



```

# Instalação  Docker-Compose Linux ( Linux ) <img align="center" alt="Rafa-Python" height="70" width="80" src="https://github.com/tiagodfigueiredo7/assets/blob/main/container-instalar-docker-compose-no-ubuntu-18-04.webp">
https://docs.docker.com/desktop/linux/install/

```
yum update
pip3 install docker-compose

```


```
which docker-compose          
sudo su
echo $PATH
export PATH=$PATH:/usr/local/bin/


docker-compose             #feito

```

 - proximo passo criando e editando o arquivo docker-compose.yml

```

vim docker-compose.yml

```
- colando e salvando o arquivo .vim

```
version: "3.2"

services:

  glpi:
    .
    .
    .
    .
    .
    .
    .
    
 ```
 
 - subindo o arquivo docker-compose.yml   
 
 ```
 
 docker-compose up -d
 
  ```
 

![Web 1](https://github.com/tiagodfigueiredo7/assets/blob/main/GLPI/ScreenHunter%20454.png)












# Autor

Tiago Domingos Figueiredo 

https://www.linkedin.com/in/tiagodfigueiredo/


![Web 1](https://github.com/tiagodfigueiredo7/assets/blob/main/t.jpg)
