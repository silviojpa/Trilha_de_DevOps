# DAY-16 | Terraform

This repository contains the full documentation for Terraform. You can also access the official Terraform documentation at [https://www.terraform.io/docs/](https://www.terraform.io/docs/).

## Sections

- [Introduction](https://www.terraform.io/docs/)
- [Getting Started](https://www.terraform.io/docs/getting-started/index.html)
- [Configuration Language](https://www.terraform.io/docs/configuration/index.html)
- [Providers](https://www.terraform.io/docs/providers/index.html)
- [Modules](https://www.terraform.io/docs/modules/index.html)
- [Command-Line Interface (CLI)](https://www.terraform.io/docs/cli/index.html)
- [State](https://www.terraform.io/docs/state/index.html)
- [Workspaces](https://www.terraform.io/docs/state/workspaces.html)
- [Terraform Cloud and Terraform Enterprise](https://www.terraform.io/docs/cloud/index.html)
- [Extending Terraform](https://www.terraform.io/docs/extend/index.html)
- [FAQ](https://www.terraform.io/docs/faq/index.html)

Terraform scripts:
https://github.com/jaiswaladi246/Terraform-scripts
---------------------------------------------------------------------------------------

Desvendando o Terraform: Do Conceito à Prática
Bem-vindo ao guia completo sobre Terraform! Este material foi criado para fornecer uma introdução clara e um exemplo prático de como utilizar essa poderosa ferramenta de Infraestrutura como Código (IaC).

# O que é o Terraform? (A Didática)
Imagine que você precisa construir uma casa. Você pode ir lá e colocar tijolo por tijolo, manualmente. Isso funciona para uma casa pequena, mas e se você precisasse construir um bairro inteiro, com 100 casas idênticas? Seria um trabalho repetitivo, demorado e muito propenso a erros.

Agora, e se você pudesse escrever uma planta detalhada (um projeto) que descreve exatamente como a casa deve ser: o número de quartos, a cor das paredes, o tipo de telha, etc. E se uma equipe de robôs pudesse ler essa planta e construir a casa (ou as 100 casas) de forma automática e perfeita?

O Terraform é essa planta. E a sua infraestrutura de tecnologia (servidores, bancos de dados, redes na nuvem como AWS, Google Cloud ou Azure) são as casas que você quer construir.

Com o Terraform, você escreve arquivos de configuração simples para "descrever" todos os componentes que você precisa. Ele é a ferramenta (os "robôs") que lê essa descrição e constrói tudo para você na nuvem.

Em resumo: Terraform é uma ferramenta de Infraestrutura como Código (IaC) que permite que você defina e provisione sua infraestrutura de TI de forma segura e eficiente usando uma linguagem de configuração declarativa.

# Por que usar Terraform?
- Automação: Construa e gerencie sua infraestrutura de forma automática, reduzindo trabalho manual.

- Repetibilidade: Garanta que sua infraestrutura seja criada da mesma forma todas as vezes, evitando erros.

- Versionamento: Trate sua infraestrutura como código. Você pode versioná-la com Git, revisar mudanças e colaborar em equipe.

- Multi-Cloud: Use uma única ferramenta para gerenciar serviços de diversos provedores de nuvem (AWS, Azure, Google Cloud, etc.).

Como o Terraform Funciona? (Os 3 Passos Mágicos)
O fluxo de trabalho do Terraform é baseado em três comandos principais:

- terraform init: Preparação. É como reunir as ferramentas e os materiais. O Terraform verifica seu código, baixa os "plugins" (providers) necessários para se comunicar com a sua nuvem (ex: AWS) e prepara o ambiente.

- terraform plan: Planejamento. O Terraform lê seu código e compara com a infraestrutura que já existe na nuvem. Ele então te mostra um plano detalhado do que será feito: o que será criado, alterado ou destruído. É a sua chance de revisar tudo antes de qualquer ação.

- terraform apply: Execução. Se você concorda com o plano, este comando executa as ações planejadas e constrói a infraestrutura descrita no seu código.

Exemplo Prático: Criando um Bucket S3 na AWS
Vamos criar um bucket de armazenamento S3 na Amazon Web Services. Um bucket é como uma pasta na nuvem onde você pode guardar arquivos.

Estrutura dos Arquivos
Para este projeto, vamos usar a seguinte estrutura de arquivos:
````
.
├── main.tf
├── variables.tf
├── terraform.tfvars
└── .gitignore
````
O Arquivo main.tf (O Cérebro)
Este é o arquivo principal onde descrevemos o que queremos criar.

````Terraform

# 1. Configuração do Provedor (Provider)
# Aqui, dizemos ao Terraform que vamos trabalhar com a AWS
# e em qual região queremos criar nossos recursos.
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}

# 2. Definição do Recurso (Resource)
# Este bloco descreve o recurso que queremos criar.
# "aws_s3_bucket" é o tipo do recurso.
# "meu_bucket_exemplo" é um nome LÓGICO que usamos no nosso código.
resource "aws_s3_bucket" "meu_bucket_exemplo" {
  # O nome do bucket precisa ser único globalmente na AWS.
  # Estamos usando uma variável para torná-lo configurável.
  bucket = var.nome_do_bucket

  tags = {
    Name        = "Meu-Bucket-Exemplo"
    Environment = "Demonstracao"
    ManagedBy   = "Terraform"
  }
}
````
O Arquivo variables.tf (As Perguntas)
Este arquivo define as "perguntas" ou variáveis que nosso código precisa para funcionar. Isso torna nosso código reutilizável e evita que coloquemos valores fixos (hardcoded) no main.tf.

````Terraform

# Variável para a região da AWS
variable "aws_region" {
  description = "A região da AWS onde os recursos serão criados."
  type        = string
  default     = "us-east-1"
}

# Variável para o nome do nosso bucket
variable "nome_do_bucket" {
  description = "O nome único para o bucket S3. Deve ser globalmente único."
  type        = string
  # Não há valor padrão aqui, forçando o usuário a fornecer um.
}

# Variáveis para as chaves de acesso da AWS (Credenciais)
# ATENÇÃO: Nunca coloque suas chaves de acesso diretamente aqui.
variable "aws_access_key" {
  description = "Sua chave de acesso da AWS (AWS_ACCESS_KEY_ID)."
  type        = string
  sensitive   = true # Marca a variável como sensível para não ser exibida nos logs.
}

variable "aws_secret_key" {
  description = "Sua chave secreta da AWS (AWS_SECRET_ACCESS_KEY)."
  type        = string
  sensitive   = true # Marca a variável como sensível.
}
````
O Arquivo terraform.tfvars (As Respostas Secretas)
Este é o arquivo onde você fornece os valores (as "respostas") para as variáveis definidas no variables.tf. Este arquivo nunca deve ser enviado para o GitHub ou qualquer outro sistema de controle de versão, pois ele contém informações sensíveis.

Crie um arquivo chamado terraform.tfvars e preencha-o com suas informações:

````Terraform

# Exemplo de preenchimento para o arquivo terraform.tfvars
# Substitua pelos seus valores reais

aws_access_key = "SUA_CHAVE_DE_ACESSO_AQUI"
aws_secret_key = "SUA_CHAVE_SECRETA_AQUI"
nome_do_bucket = "meu-bucket-super-unico-123456789" // USE UM NOME ÚNICO!
````
Importante: O Terraform carrega automaticamente os valores de um arquivo chamado terraform.tfvars, se ele existir.

O Arquivo .gitignore
Para garantir que seu arquivo de segredos (terraform.tfvars) e outros arquivos de estado do Terraform não sejam enviados para o Git, crie um arquivo .gitignore com o seguinte conteúdo:
````
# Arquivos de segredos
*.tfvars

# Arquivos de estado do Terraform
.terraform/
*.tfstate
*.tfstate.backup

# Logs de crash
crash.log
````
Colocando a Mão na Massa: Passo a Passo
Instale o Terraform e configure a AWS CLI.

Crie os quatro arquivos (main.tf, variables.tf, terraform.tfvars, .gitignore) em uma pasta no seu computador.

Abra o terminal e navegue até essa pasta.

Execute terraform init para inicializar o projeto.

````Bash

terraform init
````
Execute terraform plan para ver o que será criado.

````Bash

terraform plan
````
O Terraform mostrará que planeja criar 1 recurso (o bucket S3).

Execute terraform apply para criar o bucket. O Terraform pedirá uma confirmação final. Digite yes e pressione Enter.

````Bash

terraform apply
````
Pronto! Vá até o console da AWS na região que você configurou e você verá seu novo bucket S3 criado.

Para destruir os recursos criados e não gerar custos, use o comando:

````Bash

terraform destroy
````
## Conclusão
O Terraform é uma ferramenta transformadora para qualquer profissional de tecnologia. Ao tratar sua infraestrutura como código, você ganha agilidade, segurança e controle.

Este exemplo é apenas o começo. A partir daqui, você pode explorar a criação de servidores, bancos de dados, redes e muito mais, tudo de forma automatizada e confiável.
