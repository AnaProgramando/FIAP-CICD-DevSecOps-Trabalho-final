# FIAP-CICD-DevSecOps - Trabalho-final - Exercício Terraform

* Execute o comando `cd ~/environment/FIAP-CICD-DevSecOps/Trabalho-final` para entrar no diretório do exercício.
* Mude os arquivos para que os arquivos virem um módulo que recebe a quantidade de nós no load balancer:
  * Com base nos arquivos e pastas do exercício de Módulos, o [02-Modules](https://github.com/vamperst/FIAP-CICD-DevSecOps/tree/master/02-Terraform/demos/02-Modules), faça:
  * Estrutura do Módulo:
      * Crie uma nova pasta na qual você armazenará o módulo* Por exemplo, você pode chamá-la de load_balancer_module.
      * Dentro dessa pasta, crie os seguintes arquivos:
          * main.tf: Este arquivo conterá o código Terraform principal do seu módulo.
          * variables.tf: Este arquivo definirá as variáveis que serão usadas no módulo.
          * outputs.tf: Este arquivo definirá os outputs que serão retornados pelo módulo.
* Definir as Variáveis:
    * Abra o arquivo variables.tf no editor de texto de sua escolha.
    * Declare as variáveis necessárias para o seu módulo* Por exemplo, você pode ter uma variável chamada node_count para representar a quantidade de nós no load balancer.
    
terraform
   ```
    variable "node_count" {
      description = "The number of nodes in the load balancer"
      type        = number
    }
  ```

* Escrever o Código do Módulo:
Abra o arquivo main.tf no editor de texto.
Escreva o código Terraform que cria o load balancer, usando a variável node_count conforme necessário.

```
resource "aws_lb" "example" {
  name               = "example-lb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.lb.id]
  subnets            = var.subnet_ids
  enable_deletion_protection = false
  depends_on         = [aws_lb_target_group.example]

  lifecycle {
    create_before_destroy = true
  }
}

resource "aws_lb_target_group" "example" {
  name     = "example-tg"
  port     = 80
  protocol = "HTTP"
  vpc_id   = var.vpc_id

  dynamic "target" {
    for_each = range(var.node_count)
    content {
      target_id = aws_instance.example[target.key].id
      port      = 80
    }
  }
}
```


* Definir os Outputs
Abra o arquivo outputs.tf no editor de texto.
Defina quaisquer outputs que você deseja que o módulo retorne.

```
output "load_balancer_dns" {
  description = "The DNS name of the load balancer"
  value       = aws_lb.example.dns_name
}
```

* Usar o Módulo
No código Terraform onde você deseja usar o módulo, adicione uma referência ao diretório do módulo e forneça os valores necessários para as variáveis.

```
module "load_balancer" {
  source = "./load_balancer_module"

  node_count = 3
  subnet_ids = ["subnet-12345678", "subnet-87654321"]
  vpc_id     = "vpc-abcdef12"
}

output "load_balancer_dns" {
  value = module.load_balancer.load_balancer_dns
}
```

* Monte o arquivo que chama o módulo recem criado.
Primeiro, definimos o provedor AWS com a região onde queremos criar os recursos.
Em seguida, chamamos o módulo load_balancer que criamos anteriormente* Especificamos a origem do módulo como o diretório local onde o módulo está armazenado.
Passamos os valores necessários para as variáveis do módulo, como node_count, subnet_ids e vpc_id.
Por fim, definimos um output para expor o DNS do load balancer como um output do nosso módulo principal.
Obs: Certifique-se de ajustar os valores das variáveis para corresponder aos detalhes da sua infraestrutura, como IDs de subrede, ID da VPC e assim por diante.

```
provider "aws" {
  region = "us-east-1"
}

module "load_balancer" {
  source = "./load_balancer_module"

  node_count = 3
  subnet_ids = ["subnet-12345678", "subnet-87654321"]
  vpc_id     = "vpc-abcdef12"
}

output "load_balancer_dns" {
  value = module.load_balancer.load_balancer_dns
}
```


* Adicione estado remoto no S3 no arquivo que chama os módulos.
Neste arquivo, adicionamos uma configuração de backend Terraform que especifica o armazenamento do estado remoto no Amazon S3* Certifique-se de substituir os valores bucket, key e dynamodb_table pelos valores correspondentes na sua conta da AWS* Isso garantirá que o estado do Terraform seja armazenado de forma segura e centralizada no Amazon S3.

* Utilize o comando c9 open state.tf para abrir o arquivo responsavel por configurar o estado remoto e adicione o do seu bucket S3 na linha 3* Caso não se lembre o nome do bucket execute o comando aws s3 ls.

```
provider "aws" {
  region = "us-east-1"
}

terraform {
  backend "s3" {
    bucket         = "seu-bucket-s3"
    key            = "caminho/para/o/arquivo/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform_locks"
    encrypt        = true
  }
}

module "load_balancer" {
  source = "./load_balancer_module"

  node_count = 3
  subnet_ids = ["subnet-12345678", "subnet-87654321"]
  vpc_id     = "vpc-abcdef12"
}

output "load_balancer_dns" {
  value = module.load_balancer.load_balancer_dns
}
```


* Os nomes das maquinas definadas dentro do modulo devem ser de acordo com o ambiente do workspace* Ex: nginx-workspace-002
* O nome do ELB e do Securitygroup do módulo também devem conter o workspace
* Crie um ambiente de dev e um de prod

Exercício de 05-Workspaces (https://github.com/vamperst/FIAP-CICD-DevSecOps/tree/master/02-Terraform/demos/05-Workspaces)

* Execute o comando `cd ~/environment/FIAP-CICD-DevSecOps/02-Terraform/demos/05-Workspaces/` para entrar na pasta do exercicío.
* tilize o comando `c9 open state.tf` para abrir o arquivo responsavel por configurar o estado remoto e adicione o do seu bucket S3 na linha 3* Caso não se lembre o nome do bucket execute o comando `aws s3 ls`.
* Execute o comando `terraform init` para inicializar o terraform* Caso tenha dado erro porque o nome do bucket esta incorreto você terá que reconfigurar o estado remoto com o comando `terraform init -reconfigure`.
* Crie um novo workspace com o comando `terraform workspace new dev`
* Crie outro workspace com o comando `terraform workspace new prod`
* Para voltar ao ambiente dev execute `terraform workspace select dev`
* Para listar todos os workspaces execute `terraform workspace list`, note que um workspace default esta listado ele é criado pelo terraform automaticamente.
   ![](images/workspacescommands.png)
* Rode o apply (`terraform apply -auto-approve`) com cada um dos ambiente e note que serão gerados arquivos diferentes para cada workspaces dentro da pasta 'modules'* Para trocar de ambiente utilize o comando `terraform workspace select NOMEDAWORKSPACE`.
*  Se for no Bucket verá que foi criada uma estrutura de pastas para os workspaces que criou* E dentro das pastas prod e dev tem um arquivo 'workspaces' que é o estado do workspace em questão.
* De volta ao terminal execute um comando `ls modules/file/*.txt`, vai ver para para cada apply que fez foi criado um arquivo para o workspace.
* No workspace 'prod' execute o comando `terraform destroy -auto-approve`.
* Verifique o arquivo txt do prod não esta mais na pasta mas o dev esta intacto.
* Olhe no arquivo referente ao ambiente prod no bucket do S3, ao ler o mesmo irá notar que esta vazio, tamanho pequeno, mas não foi excluido* 

_________________________________

* Execute o comando `terraform init`
* Execute o comando `terraform apply -auto-approve`
* Aguarde alguns minutos para que todas as maquinas estejam prontas no ELB e o script terraform termine com sucesso* Após o termino no [painel](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#LoadBalancer:loadBalancerArn=terraform-example-elb;tab=targetInstances) note que o ELB estará com todas as maquinas em estado `Fora de serviço`.
   ![still](images/stillinregistration.png)
* Aguarde até que todas estejam em `Em serviço`.
   ![inservice](images/inservice2.png)
* Utilize o dns do ELB fornecido como saida no terraform no cloud9 para colar no navegador e testar o funcionamento da Stack
   ![dnsc9](images/dnsc9.png)
   ![nginx1](images/nginx1.png)
* Abra o arquivo main.tf com o comando `c9 open main.tf` e altere o valor do count para 3 na linha 67.
   ![countmod](images/countmod.png)
* Execute o comando `terraform plan` para verificar o que será alterado* Note que tem 1 item para adicionar e 1 para alterar* O item para adicionar é a nova maquina e o item para alterar é o ELB que terá que adicionar a nova maquina.
   ![plan](images/plan.png)
* Execute novamente o comando `terraform apply -auto-approve`
   ![apply2](images/apply2.png)
* Note no [painel da AWS](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#LoadBalancer:loadBalancerArn=terraform-example-elb;tab=targetInstances) que a maquina foi criada e já colocado no ELB
   ![inservice3](images/inservice3.png)
* Vá novamente até o arquivo `main.tf` e altere o valor do count para 1
      ![countmod2](images/countmod3.png)
* Execute novamente o comando `terraform apply -auto-approve`
    ![countmod2](images/countmod2.png)
* Dessa vez no [painel da AWS](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#LoadBalancer:loadBalancerArn=terraform-example-elb;tab=targetInstances) foram 2 destruições de maquina e uma alteração no ELB
    ![service1](images/inservice1.png)
* Execute o comando `terraform destroy -auto-approve`

------------------------

Faça um zip dos arquivos desse exercicio e submeta no portal da fiap.
