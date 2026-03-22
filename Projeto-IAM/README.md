# 🔐 Gerenciamento de Acessos na AWS com IAM

---

## 💼 Cenário de Negócio

Este projeto simula um cenário fictício de uma empresa que precisa estruturar o controle de acesso aos seus recursos em nuvem utilizando o AWS Identity and Access Management (IAM).

A abordagem adotada consiste na organização de permissões por meio de grupos de usuários, onde cada grupo representa um conjunto de acessos específicos a serviços como Amazon EC2 e Amazon S3.

Essa estratégia facilita a gestão de acessos e segue boas práticas de segurança, como o princípio do menor privilégio.

---

### 🔑 Estrutura de Acessos

| Usuário  | Grupo        | Tipo de Acesso |
|----------|-------------|----------------|
| user-1   | S3 Support  | Leitura no Amazon S3 |
| user-2   | EC2 Support | Leitura no Amazon EC2 |
| user-3   | EC2 Admin   | Visualizar, iniciar e parar instâncias EC2 |

> A partir dessa definição, as permissões são implementadas e validadas ao longo do laboratório.

---

## 🎯 Objetivo do Projeto

Implementar e testar um sistema de gerenciamento de identidades e acessos na AWS utilizando o IAM, garantindo segurança e controle sobre os recursos da conta.

---

## ⚙️ Atividades Realizadas

- Configuração de política de senha  
- Análise de usuários e grupos pré-existentes  
- Inspeção de permissões via políticas IAM  
- Associação de usuários a grupos  
- Teste prático das restrições de acesso  
- Utilização do URL de login do IAM  

---

## 🛠️ Tecnologias Utilizadas

- AWS IAM (Identity and Access Management)  
- Gerenciamento de usuários e grupos  
- Políticas de permissão (IAM Policies)  
- Princípio do menor privilégio  
- Controle de acesso em nuvem  

---

## 🏗️ Estrutura de Permissões (IAM)

Este projeto utiliza uma abordagem baseada em grupos para gerenciar permissões de acesso aos serviços da AWS.

A estrutura segue o princípio do menor privilégio, garantindo que cada usuário tenha apenas as permissões necessárias para executar suas atividades.

O modelo adotado representa a relação entre:

- Usuários  
- Grupos  
- Políticas de permissão  

> Diagrama desenvolvido utilizando draw.io (diagrams.net)

## ⚙️ Implementação Prática

### 🔐 Configuração da Política de Senha

Nesta etapa, foi realizada a configuração da política de senhas da conta no AWS Identity and Access Management (IAM), com o objetivo de reforçar a segurança no acesso aos usuários.

#### 🔧 Configuração

![Edição da política de senha](./images/password-edit.png)

A política foi ajustada para atender a requisitos mais rigorosos, incluindo:

- Comprimento mínimo de **10 caracteres**  
- Obrigatoriedade de:
  - Letras maiúsculas  
  - Letras minúsculas  
  - Números  
  - Caracteres especiais  
- Expiração de senha configurada para **90 dias**  
- Prevenção de reutilização das últimas **5 senhas**  
- Permissão para que usuários alterem suas próprias senhas  

#### ✅ Resultado

![Política de senha aplicada](./images/password-applied.png)

Após a configuração, as alterações foram aplicadas com sucesso, passando a valer para todos os usuários da conta.
