# 🚀 Escalabilidade e Alta Disponibilidade na AWS: Implementando Auto Scaling e Load Balancing (Linux e AWS CLI)

---

## 💼 Cenário de Negócio

Este projeto simula a necessidade de uma aplicação web que enfrenta variações constantes de tráfego. O desafio era garantir que a aplicação fosse **resiliente** (capaz de se recuperar de falhas) e **elástica** (capaz de aumentar ou diminuir recursos conforme a demanda), otimizando custos e mantendo a performance para o usuário final.

A arquitetura foi desenhada seguindo o padrão **Multi-AZ**, distribuindo servidores em diferentes Zonas de Disponibilidade para garantir a continuidade do negócio mesmo em caso de falha em um Data Center inteiro.

---

## 🎯 Objetivo do Projeto

Configurar uma infraestrutura auto-gerenciável na AWS, utilizando uma abordagem híbrida: **AWS CLI** para o provisionamento técnico de base e o **Console de Gerenciamento** para a orquestração de serviços de escalonamento dinâmico.

---

## 🏗️ Estrutura da Infraestrutura

| Componente | Especificação | Finalidade |
| :--- | :--- | :--- |
| **Região** | us-west-2 (Oregon) | Localização dos recursos físicos. |
| **Rede (VPC)** | Lab VPC | Isolamento lógico do ambiente. |
| **Sub-redes** | Privadas (AZ1 e AZ2) | Hospedagem segura das instâncias de aplicação. |
| **Capacidade** | Mín 2 / Máx 4 | Balanço entre disponibilidade e custo. |
| **Política de Scaling** | Target Tracking (50% CPU) | Manter a performance estável sob carga. |

---

## ⚙️ Implementação Prática: Tarefa 1 (Provisionamento CLI)

### 1. Preparação do Ambiente: O Command Host
Para iniciar o projeto, utilizamos uma instância chamada **Command Host**.

* **O que é:** Um servidor administrativo (*Bastion Host*) configurado para ser o ponto central de controle dentro da nuvem.
* **Por que usamos:** Em vez de conectar via SSH diretamente do meu computador pessoal, usamos o Command Host para garantir um ambiente padronizado com o **AWS CLI** já instalado e isolado na rede do laboratório.
* **Conexão:** O acesso foi feito via terminal utilizando comandos Linux, já que o sistema operacional do servidor é baseado em Unix.

### 2. Autenticação e Configuração
Antes de enviar comandos para a AWS, precisei configurar as credenciais de acesso.

* **Comando:** `aws configure` (ou exportação de variáveis).
* **Função:** Validar a identidade (Access Key e Session Token) para que a AWS permita o provisionamento dos recursos.
![Imagem colocando as credenciais](./images/Imagem1.png)

#### Validação de Região
Executei o comando abaixo para confirmar a localização geográfica do laboratório:
`curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region`
* **Função:** Um comando Linux que consulta os metadados internos da instância para filtrar (`grep`) apenas a região ativa.

### 3. Automação via User Data
Naveguei até a pasta de scripts para validar a automação:
`cd /home/ec2-user/`
![Imagem entrando na pasta User-EC2](./images/Entrei-Pasta-EC2-User.png)

Para inspecionar o script de instalação, usei:
`more UserData.txt`
* **Função:** O comando `more` permite visualizar o conteúdo de um arquivo de texto página por página. Este script contém as instruções de instalação do Apache e PHP que o servidor executará automaticamente no boot.
![Imagem comando UserData](./images/ComandoUserData.png)

### 4. Lançamento da Instância Base
Após ajustar as credenciais de sessão, lancei o servidor que servirá de modelo:
![Imagem do erro das credenciais corrigidos](./images/Erro-Corrigido-Credenciais.png)

> **Comando de Provisionamento:**
> ```bash
> aws ec2 run-instances --key-name vockey --instance-type t3.micro --image-id ami-0126145f1f70769be --user-data file:///home/ec2-user/UserData.txt --security-group-ids sg-0c7e1988b2ae2e3b3 --subnet-id subnet-08f6b8f60451cd349 --associate-public-ip-address --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=WebServer}]' --output text --query 'Instances[*].InstanceId'
> ```
![Imagem comando para ligar instancia](./images/Comando-Ligar-Instancia.png)

#### Sincronização de Estado
Utilizei o comando de espera para garantir que a instância estivesse pronta antes de prosseguir:
`aws ec2 wait instance-running --instance-ids [ID-DA-INSTANCIA]`

### 5. Validação e Golden Image
Para testar o servidor, capturei seu DNS Público:
`aws ec2 describe-instances --instance-id [ID] --query 'Reservations[0].Instances[0].NetworkInterfaces[0].Association.PublicDnsName' --output text`
![Imagem para pegar DNS](./images/Comando-Pega-DNS.png)

Ao acessar o endereço no navegador, confirmei que o servidor base estava operando corretamente com a aplicação ativa:
![Imagem do servidor funcionando na web](./images/Servidor-base-criado.png)

#### Comprovação no Console
![Imagem mostrando a instancia no console comprovando que criou](./images/Instancia-Web-Server-criada-Console.png)

#### Criando o Molde (AMI)
Com o servidor validado, criei a **Golden Image (AMI)**:
`aws ec2 create-image --name WebServerAMI --instance-id [ID]`
* **Função:** Este comando "fotografa" o estado atual do servidor. Esta imagem será o padrão utilizado pelo Auto Scaling para criar novos clones idênticos em segundos.
![Imagem do comando para criar imagem](./images/Comando-Criar-Imagem.png)

#### AMI Disponível no Console
![Imagem mostrando AMI criada no console](./images/AMI-Criada-Console.png)

---

## ⚙️ Orquestração: Tarefa 2 & 3 (Console de Gerenciamento)

Com a AMI pronta, a infraestrutura foi conectada através de serviços de orquestração:

### 1. Application Load Balancer (ALB)
Configurado como o ponto único de entrada (DNS) para os usuários, distribuindo o tráfego para um **Target Group** monitorado. O ALB realiza *Health Checks* constantes para garantir que apenas servidores saudáveis recebam tráfego.

### 2. Auto Scaling Group (ASG)
O ASG utiliza o **Launch Template** baseado na AMI criada via CLI.
* **Capacidade Desejada:** 2 instâncias iniciais.
* **Elasticidade:** Configurado para escalar até 4 instâncias caso a carga de trabalho aumente.

---

## ⚡ Teste de Estresse e Elasticidade

A prova real da arquitetura foi o teste de carga. Ao acionar o script de estresse para consumir 100% de CPU, o comportamento do sistema foi monitorado:

### 🚩 Alarme e Resposta
O CloudWatch identificou o pico de processamento e disparou o alarme. O Auto Scaling respondeu imediatamente, provisionando novas instâncias para dividir a carga e manter a estabilidade da aplicação.

---

## 📝 Conclusão

Este projeto consolidou o fluxo completo de entrega de infraestrutura moderna. Aprendi a utilizar a **CLI** para automação e o **Console** para gerenciamento de alto nível.

### Principais aprendizados:
- **Troubleshooting de Identidade:** Resolução de erros de `AuthFailure` e gestão de tokens de sessão.
- **Automação de Provisionamento:** Uso de Scripts Bash (User Data) para configuração *zero-touch*.
- **Conceito de Golden Image:** Redução do tempo de deploy através de AMIs pré-configuradas.

✅ **Status do Projeto:** Implementado com sucesso e documentado.

---
> **Projeto desenvolvido por Gisele Lage durante o treinamento de Cloud Architect.**
