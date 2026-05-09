# 🏗️ Infraestrutura como Código com AWS CloudFormation

---

## 💼 Cenário de Negócio

Este projeto simula a necessidade de uma empresa em **automatizar a criação de infraestrutura de rede** na AWS, garantindo consistência, confiabilidade e redução de erros humanos.  
A abordagem adotada utiliza o **AWS CloudFormation** para provisionar recursos de forma declarativa, permitindo que ambientes sejam replicados facilmente em diferentes regiões ou contas.

---

## 🎯 Objetivo do Projeto

Implantar e gerenciar uma infraestrutura básica de rede utilizando **CloudFormation**, incluindo:

- Criação de uma **VPC**  
- Configuração de uma **subnet pública**  
- Associação de **Internet Gateway** e **Route Table**  
- Definição de um **Security Group** para aplicações web  

---

## ⚙️ Atividades Realizadas

- Upload do template `task1.yaml` no console do CloudFormation  
- Análise das seções **Parameters**, **Resources** e **Outputs**  
- Criação da pilha com status `CREATE_COMPLETE`  
- Validação dos recursos provisionados no console da VPC  
- Observação dos eventos e logs gerados pelo CloudFormation  

---

## 🛠️ Tecnologias Utilizadas

- **AWS CloudFormation** (Infraestrutura como Código)  
- **Amazon VPC** (Virtual Private Cloud)  
- **Subnets** (rede pública)  
- **Internet Gateway**  
- **Route Tables**  
- **Security Groups**  

---

## 🏗️ Estrutura da Infraestrutura

Este laboratório cria a seguinte arquitetura de rede:

- **VPC** com CIDR `10.0.0.0/20`  
- **Subnet pública** com CIDR `10.0.0.0/24`  
- **Internet Gateway** conectado à VPC  
- **Tabela de rotas pública** direcionando tráfego externo para o IGW  
- **Security Group** permitindo tráfego HTTP (porta 80)  

---

## ⚙️ Implementação Prática

### 📥 Upload do Template

Print da tela de upload do arquivo `task1.yaml`.

### 📝 Especificação de Detalhes

Print da tela **Specify Details** com o nome da pilha “Lab”.

### 🔄 Criação da Pilha

Print da tela de **Events** mostrando `CREATE_IN_PROGRESS`.

### ✅ Resultado Final

Print da tela de **Resources** com todos os recursos criados.  
Print do console da **VPC** mostrando a “Lab VPC”.

---

## 📊 Outputs

O template fornece como saída o **Security Group padrão da VPC** criada.  
Print da seção **Outputs** no CloudFormation.

---

## 💡 Considerações

- O uso de CloudFormation garante **consistência** e **automação**.  
- A pilha pode ser recriada em outra região sem esforço adicional.  
- O rollback automático protege contra falhas durante a criação.  

---

## 📝 Conclusão

Este laboratório demonstrou como o **CloudFormation** simplifica a criação de infraestrutura de rede na AWS.  
Principais aprendizados:

- **Infraestrutura como Código (IaC)** → ambientes replicáveis e auditáveis.  
- **Automação** → menos erros humanos e maior agilidade.  
- **Governança** → integração com IAM e auditoria via CloudTrail.  

✅ **Resumo final:** O CloudFormation é essencial para equipes que precisam de **escala, consistência e governança** na nuvem.


