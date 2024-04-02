# FIAP-CICD-DevSecOps - Trabalho-final - Exercício Terraform

-------------------------------------


# 00 - Setup e Configuração de ambiente

Você irá utilizar 2 ferramentas para desenvolver os laboratorios:

1. Conta AWS através da AWS Academy. Conta provisionada para você utilizar durante essa disciplina com 100 dólares de crédito.
2. Cloud 9 IDE. Uma IDE cloud dentro da AWS para todos terem um ambeinte igual e com a autorização para executar os comandos dos exercicios.

### Conta AWS - Academy

1. Caso não tenha conta no AWS Academy ainda:
   1. Entre no seu email da FIAP pelo endereço [webmail.fiap.com.br](http://webmail.fiap.com.br/)
   2. Seu email é no seguinte formato rm + numero do seu RM + @fiap.com.br. Caso seu RM seja por exemplo 12345 seu email seria <rm12345@fiap.com.br>. A senha é a mesma de portais.
   3. Você terá um email na caixa de entrada para convite do Academy, siga as instruções.
   4. Ao conseguir entrar na plataforma do academy, aparecerá uma turma que começa com `AWS Academy Learner Lab`. Apenas clique em `Enroll` para aceitar e acessar.
2. Para entrar na conta do Academy que já existe, acesse o link:  [awsacademy.com/LMS_Login](https://www.awsacademy.com/LMS_Login). Ao conseguir entrar na plataforma do academy, aparecerá uma turma que começa com AWS Academy Learner Lab. Apenas clique em Enroll para aceitar e acessar.
3. Dentro da plataforma clique em `cursos` na lateral esquerda e clique no curso da disciplina atual.
![](img/academy1.png)
4. Dentro do curso clique em `Módulos` na lateral esquerda

   ![](img/academy2.png)

5. Clique em `Iniciar os laboratórios de aprendizagem da AWS Academy`

   ![](img/academy3.png)

6. Se for seu primeiro acesso irão aparecer os 2 contratos de termos e condições para aceitar. Role até o final para aceitar após a leitura do mesmo. Caso já tenha feito isso antes. Pule para o passo 8.
   ![](img/academy4.png)
7. Clique no link iniciando com `Academy-CUR` para acessar a conta AWS.Caso peça consentimento clique em `I agree` e execute o passo novamente.
   ![](img/academy8.png)
8. Essa é a tela para acessar sua conta AWS. Cada sessão terá 4 horas. Após esse tempo você terá que começar outra sessão, mas os dados gravados dentro da conta AWS ficam salvos até o final do curso ou entrega do trabalho final da disciplina. Clique em `Start Lab` para iniciar uma sessão. Esse processo pode demorar alguns minutos.
   ![](img/academy5.png)
   ![](img/academy6.png)
9. Quando tudo estiver pronto a bolinha ao lado do escrito `AWS` no superior esquerdo da tela ficará verde como na imagem abaixo. Apenas clique em `AWS` para abrir a conta aws em outra aba do navegador.
![](img/academy7.png)

### Cloud 9

1. Abra o console da AWS e va para o serviço `Cloud 9` através da barra de pesquisas.
   
   ![img/acharcloud9.png](img/acharcloud9.png)

2. garanta que a região que esta utilizando é `us-east-1/ Norte da Virgínia`. Você consegue ver isso no canto superior direiro da tela.
    
    ![img/regiao.png](img/regiao.png)

3. Clique em `create environment`.
4. Coloque o nome `lab-fiap` .
   
   ![img/nomelab.png](img/nomelab.png)

5. Deixe as configurações como na imagem a seguir. Se atente ao tipo da maquina que deve ser `t2.medium` e o sistema operacional `Ubuntu Server 22.04 LTS`:
   
   ![img/config.png](img/config.png)

6. Em `Configurações de rede` deixe como na imagem a seguir:
  
  ![img/configuracaoDeRede.png](img/configuracaoDeRede.png)

7. Caso todos os passos anteriores estejam corretos, clique em `Criar`.
8. A criação do ambiente pode levar alguns minutos.
    
    ![img/criando.png](img/criando.png)

9. Após a criação clique em `Em aberto`, caso o IDE não tenha aberto automaticamente.
   
![img/abriride.png](img/abriride.png)

10.  Para os próximos comandos utilize o console bash que fica no canto inferior do 
seu IDE.
   
![img/bash.png](img/bash.png)

11.  Execute o comando `npm install -g serverless` para instalar o serverless framework.
    
![img/installserverless.png](img/installserverless.png)

12.  Execute o comando `sudo apt update -y && sudo apt  install jq -y` para instalar o software que irá nos ajudar a ler e manipular Jsons no terminal. Caso esse comando de erro, aguarde 3 minutos para o sistema terminar a preparação inicial.
13. Execute o comando `npm install -g c9` para baixar a extenção que ajudará o Cloud9 a lidar melhor com o como abrir arquivos no IDE.
14. Execute o comando `git clone https://github.com/vamperst/FIAP-CICD-DevSecOps.git` para clonar o repositório com os exercicios.
15. Execute o comando `cd FIAP-CICD-DevSecOps/` para entrar na pasta criada pelo git
16. Execute o comando `cd 00-Setup-e-configuracao` para entrar na pasta com os scripts de Configuração.
17. Precisamos aumentar o tamanho do volume(HD) do cloud9. Para isso execute o comando  `sh resize.sh`
   
![img/resizeEBS](img/resizeEBS.png)

18. Agora vamos criar o Bucket S3 que irá receber todos os arquivos de configuração durante o curso. Para tal, abra uma [aba do console AWS](https://us-east-1.console.aws.amazon.com/console/home?region=us-east-1). Clique em serviços no canto superior esquerdo e pesquisa e clique em S3.
19. Clique em 'Criar bucket'
    
![](img/s3CreateBucket.png)

20.  De o nome do bucket de `base-config-<SEU RM>` e clique em `Criar` no final da página. Não há necessidade de alterar nenhum outro parametro.

![](img/createBucket.png)

21.  Utilizando a mesma aba do AWS Academy onde acessa a conta da AWS e no canto superior direito clique em 'AWS Details' e clique em 'show' nos campos de SSH Key.

![](img/academy-pem-1.png)

22.  Copie o conteudo da chave privada para a area de transferência(Ctrl+C).
    
![](img/academy-pem-2.png)

23.  Execute o comando `c9 open ~/.ssh/vockey.pem` para criar o arquivo que utilizaremos como chave para entrar nas instancias. 
24. Cole o conteudo da chave privada copiado nos passos anteriores e cole no ide no aquivo vockey.pem e salve utilizando "ctrl+S".
25.  Execute o comando `chmod 400 ~/.ssh/vockey.pem` para que a chave tenha a permissão correta.
26.  Execute o comando `aws s3 cp ~/.ssh/vockey.pem s3://base-config-<SEU RM>/instance-need/keys/` para copiar a chave que criou para o seu bucket de configurações do S3.
27. Execute o comando a seguir para instalar o terraform `sh installTerraform.sh`


-------------------------------------



* Antes de executar os scripts para criação do runner do gitlab é necessário criar uma conta no serviço. Caso já tenha, apenas faça o login. [GITLAB](https://gitlab.com/).
* Para conseguir fazer os commits para o gitlab você irá precisar criar uma chave de conexão. Para tal siga os passos:
   * Vá ao terminal do Cloud9 e utilize o seguinte comando para criar a chave:
   ```shell
    ssh-keygen -t rsa -b 2048 -C "gitlab key" -f /home/ubuntu/.ssh/gitlab
   ```
   * Pressione enter duas vezes para sinalizar que não quer senha para a chave
   ![](img/gitlab-1.png)
   * Abre a parte publica da sua chave no IDE do cloud9 com o comando `c9 open /home/ubuntu/.ssh/gitlab.pub` e copie o conteúdo para a área de transferência do seu computador.
   * Acesse o link da configuração de chaves do seu gitlab: [Chaves Gitlab](https://gitlab.com/-/profile/keys)
   * Cole o conteúdo copiado no campo destacado e clique em `Add New Key`
   ![](img/gitlab-2.png)

* Execute o comando `cd ~/environment/FIAP-CICD-DevSecOps/` para entrar na pasta principal do repositório e então execute o comando `git reset --hard && git pull origin master` para garantir que esta com a versão mais atualizada do exercicio.'.
* Execute o comando `cd ~/environment/FIAP-CICD-DevSecOps/Trabalho-final` para entrar no diretório do exercício.
* Primeiramente é necessário instalar o ansible além de atualizar o python e utilizar virtual env. Para tal vamos utilizar a sequência de comandos abaixo.
```bash

#Atualizando o python para 3.8
sudo apt update -y
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:deadsnakes/ppa -y
sudo apt install python3.8 -y
python3.8 --version
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1
python --version

#Instalando Ansible
sudo add-apt-repository --yes --update ppa:ansible/ansible -y
sudo apt install ansible -y

#Instalando pip3
sudo apt-get -y install python3-pip -y

#Instalando e utilizando virtualEnv
sudo pip3 install virtualenv
python3 -m venv ~/venv 
source ~/venv/bin/activate
```

   * No terminal do cloud9 execute os comandos abaixo para ativar a chave na sessão de terminal que esta utilizando:
   ```shell
    eval $(ssh-agent -s) 
    ssh-add -k /home/ubuntu/.ssh/gitlab
   ```

* Vamos criar um projeto final no gitlab. Para isso acesse o [link](https://gitlab.com/projects/new). Clique em `Create Blank Projet`.
* De o nome de `projeto final` ao projeto. Marque como `Public` e desmarque a opção de inicializar com README. 
   ![](img/gitlab-3.png)
* Clique em `Create project`

AQUI **** ==========

  
* De volta ao Cloud9 você vai subir o código desse projeto final no gitlab. Para isso siga os comandos abaixo tomando o cuidado com os pontos onde precisa colocar suas informações
```bash
git config --global user.name "SEU NOME"
git config --global user.email "SEU EMAIL DO GITLAB"
cd /home/ubuntu/environment/projeto-final

git init
git remote add origin git@gitlab.com:SEU-USUARIO/projeto-final.git
git add .
git commit -m "Initial commit"
git push -u origin master
```
![](img/gitlab-4.png)
![](img/gitlab-5.png)

* No seu repositório do gitlab clique em settings na lateral esquerda e então clique em `CI/CD`
    ![](img/gitlab-6.png)
* Em `Runners` clique em `Expand`
    ![](img/gitlab-7.png)
* Desabilite a opção `Enable shared runners for this project` 
    ![](img/gitlab-8.png)
* Copie o token e mantenha na área de transferência. Clique nos três pontos ao lado de `New project runner` e copie.
    ![](img/gitlab-9.png)
* De volta ao cloud9 você vai colar o token do gitlab no arquivo ansible que registra o runner. Para tal execute o comando `c9 open ~/environment/FIAP-CICD-DevSecOps/03-Ansible/01-provisionando-gitlab-runner/ansible-gitlab-runner/tasks/register-runner.yml` e altere a linha 48. Não esqueça de salvar.
* O runner do gitlab será uma EC2 que será provisionada com terraform. Para entrar na pasta com o código execute o comando `cd ~/environment/FIAP-CICD-DevSecOps/03-Ansible/01-provisionando-gitlab-runner/terraform-gitlab-runner/`
* Atualize o estado remoto do repositório para utilizar um bucket S3 disponivel na sua conta. Abra o arquivo com `c9 open state.tf`
* Agora que já alterou o bucket e salvou. Execute o comando `terraform init`
* Verifique o que será criado com o comando `terraform plan`
* Execute o `terraform apply --auto-approve` para subir a maquina que será o runner. lembrando que esse script executa alguns comandos na maquina criada para preprar tudo para que o ansible consiga executar no host criado.
* Execute o comando `terraform output ec2_dns` para copiar o ip publico da instancia para a área de transferência.
    ![](img/gitlab-11.png)
* Agora entre na pasta onde vai executar o ansible. Para isso utilize o comando `cd ~/environment/FIAP-CICD-DevSecOps/03-Ansible/01-provisionando-gitlab-runner/ansible-gitlab-runner/`
* Utilize o comando `c9 open hosts` para abrir o arquivo onde é configurado quais maquinas e como serão acessadas pelo ansible. Altere adicionando o IP publico da instancia criada no local indicado.
    ![](img/gitlab-12.png)
* Execute o comando abaixo para executar o ansible que vai configurar o EC2 como gitlab runner:
``` shell
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -u 'ubuntu' -i hosts  --extra-vars 'gitlab_runner_name=gitlab-runner-fleet-001' play.yaml    
```
![](img/gitlab-14.png)
* Se voltar a mesma página do gitlab onde pegou o token notará que agora tem um runner registrado.
![](img/gitlab-13.png)

------------------------------

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

-------------------------------------------

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
* Verifique o arquivo txt do prod não esta mais na pasta mas o dev esta intacto.
* Olhe no arquivo referente ao ambiente prod no bucket do S3, ao ler o mesmo irá notar que esta vazio, tamanho pequeno, mas não foi excluido* 


-------------------------------------


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
