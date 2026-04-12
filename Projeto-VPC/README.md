# 🌐 Implementação de Rede Personalizada e Alta Disponibilidade na AWS (VPC)

---

## 💼 Cenário de Negócio

Este projeto simula a demanda de um cliente da Fortune 100 que necessita de uma infraestrutura de rede robusta, personalizada e segura na nuvem AWS. 

A solução foca na criação de uma **Virtual Private Cloud (VPC)** isolada, dividida em sub-redes públicas e privadas distribuídas em múltiplas Zonas de Disponibilidade (Multi-AZ). Essa arquitetura garante que aplicações críticas (como servidores web) fiquem protegidas por camadas de firewall e possuam conectividade controlada com a internet.

---

## 🎯 Objetivo do Projeto

Projetar e implementar uma infraestrutura de rede segmentada, configurar tabelas de rotas para tráfego público e privado e realizar o deploy de um servidor web (EC2) funcional para validar a conectividade e a segurança da rede.

---

## 🏗️ Arquitetura da Solução

O projeto segue o modelo de arquitetura padrão de mercado para alta disponibilidade:

- **VPC:** Rede isolada com bloco CIDR `10.0.0.0/16`.
- **Sub-redes:** Divisão entre camadas Públicas (acesso externo) e Privadas (isoladas).
- **Gateways:** Utilização de **Internet Gateway** para entrada/saída de dados e **NAT Gateway** para saída segura de recursos privados.
- **Segurança:** Implementação de **Security Groups** atuando como firewall estatal.

---

## 🛠️ Tecnologias Utilizadas

- **AWS VPC** (Virtual Private Cloud)
- **Amazon EC2** (Elastic Compute Cloud)
- **Subnets** (Públicas e Privadas)
- **Route Tables** (Tabelas de Rotas)
- **Internet Gateway & NAT Gateway**
- **Security Groups** (Firewall de rede)

---

## ⚙️ Implementação Prática

### 1. Criação da VPC e Infraestrutura Base

A primeira etapa consistiu no provisionamento da VPC utilizando o assistente, garantindo a criação automatizada dos componentes de conectividade inicial.

#### 🔧 Configuração da VPC
![Configurações da VPC](./images/vpc-creation-details.png)
*(Print da tela de criação da VPC com os detalhes de CIDR e nome)*

- **Nome:** `Lab VPC`
- **CIDR:** `10.0.0.0/16`
- **Componentes:** 1 Sub-rede Pública, 1 Sub-rede Privada e 1 NAT Gateway.

---

### 2. Expansão para Alta Disponibilidade (Multi-AZ)

Para atender aos requisitos de resiliência, foram criadas sub-redes adicionais manualmente em uma segunda Zona de Disponibilidade.

| Sub-rede | Bloco CIDR | Tipo |
| :--- | :--- | :--- |
| Public Subnet 2 | `10.0.2.0/24` | Pública |
| Private Subnet 2 | `10.0.3.0/24` | Privada |

#### ✅ Sub-redes Criadas
![Lista de sub-redes](./images/vpc-subnets-list.png)
*(Print da lista de sub-redes no console da AWS mostrando as 4 subnets)*

---

### 3. Associação de Tabelas de Rotas

Nesta fase, as novas sub-redes foram associadas às tabelas de rotas corretas para garantir que a `Public Subnet 2` tivesse acesso ao Internet Gateway e a `Private Subnet 2` ao NAT Gateway.

#### 🔗 Associações de Rota
![Associações de tabela de rotas](./images/vpc-route-associations.png)
*(Print da aba 'Subnet associations' de uma das tabelas de rotas)*

---

### 4. Configuração do Web Security Group (Firewall)

Foi criado um grupo de segurança específico para permitir o tráfego web, seguindo o princípio de liberar apenas o necessário.

- **Regra de Entrada:** Porta `80` (HTTP) liberada para `0.0.0.0/0`.
- **Descrição:** Permite requisições web externas.

#### 🛡️ Regras do Grupo de Segurança
![Regras de entrada HTTP](./images/vpc-security-group-rules.png)
*(Print das 'Inbound Rules' do Web Security Group)*

---

### 5. Deployment e Validação do Servidor Web

Por fim, uma instância EC2 foi lançada na `Public Subnet 2` com um script de automação (User Data) para instalar o servidor Apache.

#### 🚀 Status da Instância
![Instância EC2 rodando](./images/ec2-status-check.png)
*(Print da instância com o status '2/2 checks passed')*

#### 🌐 Validação do Acesso Externo
![Página web funcionando](./images/web-server-success.png)
*(Print do seu navegador exibindo a página do servidor através do DNS Público)*

---

## 📝 Conclusão

Este laboratório demonstrou a importância de uma rede bem estruturada para a segurança e disponibilidade de serviços em nuvem. 

### Principais aprendizados:

- **Segmentação de Rede:** A separação entre sub-redes públicas e privadas é vital para proteger dados sensíveis.
- **Roteamento Inteligente:** O uso coordenado de Internet Gateways e NAT Gateways permite controle total sobre o fluxo de dados.
- **Automação de Provisionamento:** O uso de *User Data* no EC2 mostra como a nuvem facilita o deploy rápido de serviços.

✅ **Resumo final:** A infraestrutura implementada está pronta para suportar aplicações de alta escala, garantindo que o servidor web esteja disponível para o mundo enquanto a rede interna permanece protegida.
