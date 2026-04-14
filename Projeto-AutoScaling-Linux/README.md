# 🚀 Escalabilidade e Alta Disponibilidade na AWS: Implementando Auto Scaling e Load Balancing (Linux e AWS CLI)

---

## 💼 Cenário de Negócio

Este projeto simula a necessidade de uma aplicação web que enfrenta variações constantes de tráfego. O desafio era garantir que a aplicação fosse **resiliente** (capaz de se recuperar de falhas) e **elástica** (capaz de aumentar ou diminuir recursos conforme a demanda), otimizando custos e mantendo a performance para o usuário final.

A arquitetura foi desenhada seguindo o padrão **Multi-AZ**, distribuindo servidores em diferentes Zonas de Disponibilidade para garantir a continuidade do negócio mesmo em caso de falha em um Data Center inteiro.

---

## 🎯 Objetivo do Projeto

Configurar uma infraestrutura auto-gerenciável na AWS, utilizando uma abordagem híbrida: **AWS CLI** para o provisionamento técnico de base e o **Console de Gerenciamento** para a orquestração de serviços de escalonamento dinâmico.

---

## ⚙️ Atividades Realizadas

- Provisionamento de instância EC2 via **AWS CLI**.
- Automação de instâncias utilizando scripts de **User Data** (Bash).
- Criação de uma **Golden Image (AMI)** personalizada para padronização.
- Implementação de um **Application Load Balancer (ALB)** para distribuição de carga.
- Configuração de **Launch Templates** e **Auto Scaling Groups (ASG)**.
- Execução de teste de estresse de CPU para validar a política de escalonamento horizontal.

---

## 🛠️ Tecnologias Utilizadas

- **AWS CLI:** Automação de infraestrutura via terminal.
- **Amazon EC2:** Servidores virtuais (instâncias t3.micro).
- **Amazon Machine Image (AMI):** Criação de moldes de sistema.
- **Elastic Load Balancing (ELB):** Balanceamento de tráfego inteligente.
- **Auto Scaling:** Elasticidade e gerenciamento de frota.
- **Amazon CloudWatch:** Monitoramento e gatilhos de alarmes.

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

## ⚙️ Implementação Prática

### 1. Provisionamento e Automação via CLI

Nesta etapa, utilizei a linha de comando para configurar a região de trabalho e lançar a instância que serviria de base para o projeto. O diferencial aqui foi o uso de scripts de **User Data** para que o servidor já iniciasse com o Apache e PHP instalados e configurados.

#### 🔧 Configuração e Lançamento
![Configuração CLI e Run Instances](./images/cli-provisioning.png)

> **Código CLI utilizado:**
> ```bash
> aws ec2 run-instances --key-name vockey --instance-type t3.micro --image-id ami-xxxxxxxx --user-data file://UserData.txt --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=WebServer}]'
> ```

---

### 2. Criação da Golden Image (AMI)

Para garantir que o Auto Scaling consiga lançar novos servidores em segundos, foi criada uma **AMI**. Este processo "fotografa" o estado atual do servidor (SO + Apps + Configurações), criando um molde padrão.

#### 📸 Comando de Criação da Imagem
![Criação da AMI](./images/ami-creation.png)

---

### 3. Orquestração de Alta Disponibilidade (Console)

A infraestrutura foi conectada através de um **Application Load Balancer**, que serve como o ponto único de entrada (DNS) para os usuários, distribuindo o tráfego para um **Target Group** monitorado.

#### ⚖️ Balanceador de Carga e Target Group
![Configuração do ALB](./images/alb-config.png)

#### 📈 Grupo de Auto Scaling (ASG)
O ASG foi configurado para monitorar a saúde das instâncias e gerenciar a quantidade de máquinas ativas nas sub-redes privadas.

| Parâmetro | Valor Configurado |
| :--- | :--- |
| **Modelo de Execução** | web-app-launch-template |
| **Zonas de Disponibilidade** | us-west-2a e us-west-2b |
| **Verificações de Integridade** | Ativas via ELB |

---

## ⚡ Teste de Estresse e Elasticidade

A prova real da arquitetura foi o teste de carga. Ao acionar o script de estresse (100% de CPU), o comportamento do sistema foi monitorado em tempo real.

### 🚩 Alarme do CloudWatch
O sistema identificou que a utilização média excedeu o limite de segurança de 50%.

![Estresse de CPU iniciado](./images/stress-test-start.png)

### 🚀 Resultado: Scaling Out
O Auto Scaling respondeu imediatamente, provisionando novas instâncias para dividir a carga e manter a saúde da aplicação.

![Nova instância lançada automaticamente](./images/scaling-activity.png)

---

## 📝 Conclusão

Este projeto consolidou conhecimentos fundamentais para a certificação **Cloud Practitioner** e para o dia a dia de um **Cloud Architect**.

### Principais aprendizados:

- **Infraestrutura Ágil:** O uso de AMIs e CLI reduz drasticamente o tempo de deploy de novos recursos.
- **Segurança por Design:** A aplicação de sub-redes privadas isola os servidores de ataques diretos da internet, permitindo acesso apenas via Load Balancer.
- **Otimização de Custos:** O Auto Scaling garante que a empresa pague apenas pelo recurso necessário, desligando máquinas extras em horários de baixo acesso.

✅ **Status do Projeto:** Implementado com sucesso e validado sob carga.

---
> **Projeto desenvolvido por [Seu Nome] durante o treinamento de Cloud Architect.**
