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

## 📖 Conceitos Fundamentais

### 🔹 CloudFormation não é programação
O **CloudFormation** não é para escrever código de programação, mas sim **templates declarativos** em **YAML ou JSON**.  
Esses templates descrevem **o que você quer que a AWS crie**.  

👉 Isso é chamado de **Infraestrutura como Código (IaC)**: você declara os recursos, e toda a lógica de criação e ordem de dependências é feita automaticamente pela AWS.  

### 🔹 Pilhas (Stacks)
Na AWS, a infraestrutura é organizada em **pilhas (stacks)**.  
- Uma pilha é o **conjunto de recursos criados a partir de um template**.  
- Todos os recursos da pilha são **gerenciados juntos**: se você excluir a pilha, todos os recursos são removidos.  

---

## ⚙️ Estrutura do Template YAML

Os templates podem ser escritos em **YAML** (mais legível) ou **JSON** (mais verboso).  
O formato é importante: recuos e hifens devem ser respeitados.

---

### 📖 Explicação do YAML - Parameters

```yaml
Parameters:
  LabVpcCidr:
    Type: String
    Default: 10.0.0.0/20
  PublicSubnetCidr:
    Type: String
    Default: 10.0.0.0/24
```
- **Parameters** → esta seção serve para definir valores que podem ser personalizados quando a pilha é criada.  
- **LabVpcCidr** → representa o intervalo de endereços IP (CIDR) da VPC.  
  - `Type: String` → o valor é tratado como texto.  
  - `Default: 10.0.0.0/20` → se o usuário não informar nada, esse será o valor usado.  
- **PublicSubnetCidr** → representa o intervalo de endereços IP da subnet pública.  
  - Também é do tipo `String`.  
  - `Default: 10.0.0.0/24` → valor padrão para a subnet.  

<sub>👉 Em resumo: essa parte do código define **parâmetros de entrada** que tornam o template flexível.  
Você pode reaproveitar o mesmo arquivo YAML em diferentes cenários apenas mudando os valores de CIDR na hora de criar a pilha.</sub>

### 📖 Explicação do YAML – Resources

```yaml
###########
# VPC with Internet Gateway
###########

  LabVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: !Ref LabVpcCidr
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: Lab VPC

  IGW:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
        - Key: Name
          Value: Lab IGW

  VPCtoIGWConnection:
    Type: AWS::EC2::VPCGatewayAttachment
    DependsOn:
      - IGW
      - LabVPC
    Properties:
      InternetGatewayId: !Ref IGW
      VpcId: !Ref LabVPC
```

- **Resources** → esta seção define os recursos que serão criados na pilha.  
- **LabVPC** → cria uma VPC usando o CIDR definido em `LabVpcCidr`.  
  - `EnableDnsSupport` e `EnableDnsHostnames` ativam suporte a DNS e nomes de host.  
  - A tag `Name: Lab VPC` facilita a identificação no console.  
- **IGW** → cria um **Internet Gateway**, que conecta a VPC à Internet.  
- **VPCtoIGWConnection** → faz a ligação entre a VPC e o Internet Gateway.  
  - `DependsOn` garante que tanto a VPC quanto o IGW existam antes da conexão.  
  - `InternetGatewayId` e `VpcId` usam referências (`!Ref`) para apontar para os recursos criados.  

<sub>👉 Em resumo: essa parte do código define a **rede principal (VPC)**, cria o **gateway de Internet** e estabelece a **conexão entre eles**, preparando a base da infraestrutura.  
Além disso, o template também cria outros recursos como tabela de rotas pública, rota padrão, subnet pública, associação da subnet à tabela de rotas e um grupo de segurança para aplicações web.</sub>

### 📖 Explicação do YAML – Outputs

```yaml
###########
# Outputs
###########

Outputs:

  LabVPCDefaultSecurityGroup:
    Value: !Sub ${LabVPC.DefaultSecurityGroup}
```

- **Outputs** → esta seção serve para expor valores de saída após a criação da pilha.  
- **LabVPCDefaultSecurityGroup** → retorna o **Security Group padrão** da VPC criada.  
  - `Value: !Sub ${LabVPC.DefaultSecurityGroup}` → utiliza a função `!Sub` para substituir dinamicamente o ID do Security Group associado à VPC.  

<sub>👉 Em resumo: essa parte do código fornece informações úteis sobre recursos criados, permitindo consultar ou reutilizar esses valores em outras pilhas ou configurações.</sub>

---

## 📝 Conclusão

Este laboratório demonstrou como o **CloudFormation** simplifica a criação de infraestrutura de rede na AWS.  
Principais aprendizados:

- **Infraestrutura como Código (IaC)** → ambientes replicáveis e auditáveis.  
- **Automação** → menos erros humanos e maior agilidade.  
- **Governança** → integração com IAM e auditoria via CloudTrail.  

✅ **Resumo final:** O CloudFormation é essencial para equipes que precisam de **escala, consistência e governança** na nuvem.


