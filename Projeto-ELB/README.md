# ⚡ Escalabilidade e Balanceamento de Carga na AWS com EC2, ALB e Auto Scaling

---

## 💼 Cenário de Negócio

Este projeto simula um cenário fictício de uma empresa que precisa garantir **alta disponibilidade** e **escalabilidade automática** para sua aplicação web hospedada na AWS.  

A solução foi construída utilizando:  
- **Amazon EC2** para hospedar a aplicação.  
- **Amazon Machine Image (AMI)** para padronizar instâncias.  
- **Application Load Balancer (ALB)** para distribuir tráfego.  
- **Auto Scaling Group (ASG)** para ajustar dinamicamente a capacidade.  
- **Amazon CloudWatch** para monitorar métricas e acionar políticas de escalonamento.  

---

## 🎯 Objetivo do Projeto

Implementar uma arquitetura **elástica e resiliente** na AWS, capaz de:  
- Escalar horizontalmente conforme a demanda.  
- Garantir alta disponibilidade em múltiplas zonas de disponibilidade.  
- Distribuir tráfego de forma eficiente entre instâncias.  
- Monitorar métricas de desempenho e reagir automaticamente.  

---

## ⚙️ Atividades Realizadas

- Criação de uma **AMI** a partir da instância inicial (*Web Server 1*).  
- Configuração de um **Application Load Balancer** e **Target Group**.  
- Criação de um **Launch Template** para padronizar instâncias.  
- Configuração de um **Auto Scaling Group** com políticas de escalonamento.  
- Teste de balanceamento de carga via DNS do ALB.  
- Teste de escalabilidade automática com **CloudWatch Alarms**.  
- Encerramento da instância inicial (*Web Server 1*) após criação da AMI.  

---

## 🛠️ Tecnologias Utilizadas

- **Amazon EC2**  
- **Amazon Machine Image (AMI)**  
- **Application Load Balancer (ALB)**  
- **Target Groups**  
- **Auto Scaling Group (ASG)**  
- **Amazon CloudWatch**  

---

## 🏗️ Estrutura da Arquitetura

A arquitetura final é composta por:  
- **Instâncias EC2 em subnets privadas**, criadas automaticamente pelo Auto Scaling.  
- **Application Load Balancer em subnets públicas**, recebendo tráfego externo.  
- **Target Group** conectando o ALB às instâncias EC2.  
- **CloudWatch Alarms** monitorando métricas de CPU e acionando escalonamento.  

---

## ⚙️ Implementação Prática

### 1️⃣ Criação da AMI

A partir da instância inicial (*Web Server 1*), foi criada uma **AMI** para padronizar futuras instâncias.  

![Criando AMI](./images/Criando-Imagem.png)  
![AMI criada](./images/AMI-Criada.png)

---

### 2️⃣ Configuração do Load Balancer

Foi criado um **Application Load Balancer** para distribuir tráfego entre múltiplas instâncias.  

- Tipo: **Application Load Balancer (HTTP)**  
- Subnets: públicas  
- Target Group: `lab-target-group | HTTP`  
- Porta: **80**  

![Criando ALB](./images/Criando-ALB.png)  
![Criando ALB - continuação](./images/Criando-ALB1.png)  
![Grupo de segurança e listeners do ALB](./images/Grupo-Seguranca-Listener-ALB.png)  
![Criando grupo de destino](./images/Criando-Grupo-Destino.png)  
![Registro de destino](./images/Registro-Destino.png)  
![Grupo de destino criado](./images/Grupo-Destino-Criado.png)  
![ALB criado](./images/ALB-Criado.png)

---

### 3️⃣ Criação do Launch Template

Definido o **Launch Template** para instâncias EC2:  

- AMI: criada no passo anterior  
- Tipo de instância: `t3.micro`  
- Security Group: `Web Security Group`  

![Criando modelo de execução - parte 1](./images/Criando-Modelo-Execucao-1.png)  
![Criando modelo de execução - parte 2](./images/Criando-Modelo-Execucao-2.png)  
![Criando modelo de execução - parte 3](./images/Criando-Modelo-Execucao-3.png)  
![Modelo de execução criado](./images/Modelo-Execucao-Criado.png)

---

### 4️⃣ Criação do Auto Scaling Group

Configurado o **ASG** com:  

- Nome: `Lab Auto Scaling Group`  
- VPC: `Lab VPC`  
- Subnets privadas: `10.0.1.0/24` e `10.0.3.0/24`  
- Capacidade desejada: **2**  
- Capacidade mínima: **2**  
- Capacidade máxima: **4**  
- Política de escalonamento: **Target Tracking (CPU média = 50%)**  

![Criando ASC - parte 1](./images/Criando-ASC-1.png)  
![Criando ASC - parte 2](./images/Criando-ASC-2.png)  
![Criando ASC - parte 3](./images/Criando-ASC-3.png)  
![Grupo ASC criado](./images/Grupo-ASC-Criado.png)

---

### 5️⃣ Teste do Balanceamento de Carga

Após criação do ASG, duas instâncias *Lab Instance* foram iniciadas.  

- Verificação de integridade: **Saudável** no Target Group.  
- Teste via DNS do ALB → aplicação respondeu corretamente.  

![Instâncias mínimas criadas pelo ASC](./images/Instancias-Minimas-Criadas-Pelo-ASC.png)

---

### 6️⃣ Teste de Escalabilidade Automática

- Aplicação de carga via **Teste de Carga**.  
- CloudWatch detectou CPU > 50%.  
- AlarmHigh acionado → novas instâncias criadas automaticamente.  

![Alarme CloudWatch sem entrar no site](./images/Alarme-CloudWatch-sem-entrar-site.png)  
![Entrou no site e usando CPU](./images/Entrou-site.png)  
![CloudWatch em alarme](./images/CloudWatch-Em-alarme.png)  
![Mais duas Lab Instance criadas](./images/Mais-Duas-Lab-Instance.png)

---

### 7️⃣ Encerramento da Instância Inicial

A instância **Web Server 1** foi encerrada, pois não é mais necessária.  
O ambiente agora depende apenas do Auto Scaling Group.  

![Encerrando Web Server 1](./images/Encerra-Web-Server.png)

---

## ✅ Resultado Final

- Arquitetura elástica e altamente disponível.  
- Escalabilidade automática configurada com base em métricas de CPU.  
- Balanceamento de carga funcionando via ALB.  
- Instâncias protegidas em subnets privadas.  

---

## 📝 Conclusão

Este laboratório demonstrou, na prática, como construir uma arquitetura **resiliente e escalável** na AWS utilizando EC2, ALB, Auto Scaling e CloudWatch.  

### Principais aprendizados:
- **AMI** garante padronização das instâncias.  
- **ALB + Target Group** distribuem tráfego de forma eficiente.  
- **Auto Scaling Group** ajusta capacidade conforme demanda.  
- **CloudWatch Alarms** monitoram métricas e acionam escalonamento.  
- **Subnets privadas** aumentam a segurança da aplicação.  

✅ **Resumo final:**  
O exercício evidencia como combinar serviços da AWS para criar uma solução **altamente disponível, segura e escalável**, alinhada às boas práticas de arquitetura em nuvem.  
