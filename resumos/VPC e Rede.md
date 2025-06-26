# 🌐 VPC e Redes – Virtual Private Cloud e Fundamentos de Rede

## 🎯 Objetivo deste resumo

Este documento aborda os conceitos essenciais de **VPC (Virtual Private Cloud)** e fundamentos de rede na AWS, com dicas práticas para a prova **AWS Cloud Practitioner**, exemplos de comandos **AWS CLI** e boas práticas de arquitetura.

---

## 🏗️ O que é VPC?

* **VPC** é um ambiente de rede virtual dedicado na AWS, isolado logicamente dentro da nuvem.
* Permite definir **endereçamento IP**, **subnets**, **roteamento** e **políticas de segurança** para controlar totalmente o tráfego de rede.

### 🎯 Dicas para a prova

* Entenda que cada conta AWS cria automaticamente uma **VPC padrão** em cada região.
* VPC é **regional**, ou seja, cada região tem sua própria VPC.
* Diferencie **VPC padrão** (já configurada com subnets públicas) e **VPC customizada** (configuração do zero).

---

## 🔑 Principais componentes

| Componente                          | Função                                                     |
| ----------------------------------- | ---------------------------------------------------------- |
| **CIDR Block**                      | Bloco de IPs (ex: `10.0.0.0/16`) para a VPC                |
| **Subnets**                         | Segmentos de IP dentro da VPC (públicas ou privadas)       |
| **Internet Gateway (IGW)**          | Permite comunicação entre VPC e Internet                   |
| **NAT Gateway/Instance**            | Permite saída para Internet de subnets privadas            |
| **Route Tables**                    | Conjunto de regras de roteamento para subnets              |
| **Security Groups (SG)**            | Firewall em **nível de instância EC2** (stateful)          |
| **Network ACLs (NACL)**             | Firewall em **nível de rede/subnet** (stateless)           |
| **Elastic Network Interface (ENI)** | Interface de rede virtual que conecta instâncias EC2 à VPC |

---

## 📋 Explicando os Conceitos-Chave

### 🔹 CIDR (Classless Inter-Domain Routing)

* Formato usado para definir faixas de IPs em uma rede.
* Exemplo: `10.0.0.0/16` → até 65.536 IPs; `10.0.1.0/24` → até 256 IPs.
* **Dica de prova**: CIDR é essencial para definir o tamanho da VPC e subnets.

### 🔹 Subnets Públicas e Privadas

* Subnet **pública**: tem uma rota associada a um **Internet Gateway (IGW)**, permitindo acesso à internet.
* Subnet **privada**: não possui rota direta para a internet; usa **NAT Gateway** ou **NAT Instance** para tráfego de saída.
* **Boa prática**: use subnets privadas para bancos de dados e recursos sensíveis.

### 🔹 Internet Gateway (IGW)

* Componente necessário para que uma VPC tenha acesso direto à internet.
* Associado à VPC, e usado pelas subnets públicas por meio da tabela de rotas.

### 🔹 NAT Gateway / NAT Instance

* Permite que **subnets privadas** tenham acesso à internet (por exemplo, para atualizações), sem ficarem expostas diretamente.
* NAT Gateway é gerenciado, altamente disponível, mas mais caro.
* NAT Instance é mais barato, mas precisa de configuração manual e escalabilidade limitada.

### 🔹 Security Groups (SG)

* São firewalls em **nível de instância EC2** ou **ENI (interface de rede virtual)**.
* **Stateful**: se uma solicitação de entrada é permitida, a resposta de saída é automaticamente liberada.
* Permite regras de entrada e saída.
* **Dica de prova**: lembre que SGs só **permitem** tráfego, não bloqueiam — tudo que não for explicitamente permitido é negado.

### 🔹 Network ACLs (NACL)

* São firewalls em **nível de subnet**, controlando tráfego que entra ou sai da rede.
* **Stateless**: cada regra de entrada e saída deve ser criada separadamente.
* Suporta regras de **permitir e negar** tráfego.
* Boa prática: use como **segunda camada de proteção**, junto com os SGs.

---

## 🌉 Recursos de Redes Híbridas e Avançadas

### 🔁 VPC Peering

* Conecta duas VPCs para que se comuniquem por IPs privados, sem passar pela internet.
* Não suporta **roteamento transitivo** (A → B e B → C não implica A → C).
* Pode ser entre regiões ou contas diferentes.

### 🏢 AWS Outposts

* Executa serviços AWS **localmente**, no seu datacenter, com gerenciamento da AWS.
* Ideal para **ambientes híbridos**, com baixa latência ou exigência de dados locais.
* Integra-se com VPC, IAM, EC2, S3, etc.

---

## 💻 Prática com AWS CLI

```bash
# Criar uma VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Nomear a VPC
aws ec2 create-tags \
  --resources vpc-xxxxxxxx \
  --tags Key=Name,Value=MinhaVPC

# Criar subnets pública e privada
aws ec2 create-subnet --vpc-id vpc-xxxxxxxx --cidr-block 10.0.1.0/24 --availability-zone sa-east-1a
aws ec2 create-subnet --vpc-id vpc-xxxxxxxx --cidr-block 10.0.2.0/24 --availability-zone sa-east-1b

# Criar e associar Internet Gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --vpc-id vpc-xxxxxxxx --internet-gateway-id igw-yyyyyyyy

# Criar rota pública
aws ec2 create-route --route-table-id rtb-zzzzzzzz --destination-cidr-block 0.0.0.0/0 --gateway-id igw-yyyyyyyy

# Criar NAT Gateway
aws ec2 create-nat-gateway --subnet-id subnet-xxxxxxxx --allocation-id eipalloc-abcdefgh

# Criar Security Group
aws ec2 create-security-group --group-name SG-web --description "Allow HTTP/HTTPS" --vpc-id vpc-xxxxxxxx
aws ec2 authorize-security-group-ingress --group-id sg-xxxxxxxx --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id sg-xxxxxxxx --protocol tcp --port 443 --cidr 0.0.0.0/0
```

---

## 🧠 Boas práticas e dicas extras

* **Alta disponibilidade**: use múltiplas AZs e distribua suas subnets.
* **Segurança**: combine SG e NACL; ative logs (VPC Flow Logs) para auditoria.
* **Custo**: prefira NAT Instance em ambientes de teste para economia.
* **Organização**: adote padrões de nomenclatura e tags para facilitar a gestão.
* **Restrições**:

  * Subnet pública → precisa de IGW e rota.
  * Subnet privada → usa NAT para sair.
  * ACL → bloqueia tráfego, SG → apenas permite.
* **Monitoramento**: VPC Flow Logs ajudam a diagnosticar tráfego e falhas.

---

> 📚 Para reforçar: estude bem os papéis e diferenças entre SG, NACL, NAT, IGW, subnets públicas/privadas e CIDR — são temas frequentes e fundamentais para a certificação AWS Cloud Practitioner.
