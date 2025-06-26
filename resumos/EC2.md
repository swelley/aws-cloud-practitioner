# 💻 EC2 – Amazon Elastic Compute Cloud

## O que é?

- EC2 (Elastic Compute Cloud) é um serviço da AWS que permite criar **servidores virtuais** na nuvem.
- Você pode escolher o **sistema operacional, tipo de instância (CPU, memória), armazenamento e rede**.

---

## Principais conceitos

### 🧱 Tipos de instância
- Definem a **capacidade de processamento e memória**.
- Exemplos:
  - `t2.micro`: uso geral, gratuito (Free Tier)
  - `m5.large`: uso geral com mais desempenho
  - `c5`: para aplicações que exigem alto poder de CPU

### 📦 AMI (Amazon Machine Image)
- É a **imagem usada para criar a instância EC2** (ex: Ubuntu, Amazon Linux, etc.)

### 🧰 Storage (armazenamento)
- Geralmente usa **EBS (Elastic Block Store)**
- Custa mesmo se a instância estiver parada

### 🔐 Chave SSH
- Arquivo `.pem` gerado na criação da instância
- Usado para **acesso remoto via terminal (Linux)**

### 🌐 Elastic IP
- Endereço IP fixo que pode ser associado a uma instância EC2
- Se não estiver associado, pode gerar custo

---

## Estados da instância

| Estado       | Custa? | Observações                              |
|--------------|--------|------------------------------------------|
| **Running**  | ✅ Sim  | Instância ativa                          |
| **Stopped**  | ⚠️ Parcial | EBS continua gerando custo               |
| **Terminated** | ❌ Não  | Instância e dados deletados permanentemente |

---

## Segurança

- As instâncias EC2 são protegidas por **Security Groups** (como um firewall)
- Você pode permitir ou bloquear portas, como:
  - `22`: SSH (Linux)
  - `80`: HTTP
  - `443`: HTTPS

---

## Prática com AWS CLI

```bash
# Criar um par de chaves
aws ec2 create-key-pair --key-name MinhaChave --query 'KeyMaterial' --output text > MinhaChave.pem

# Listar tipos de instância disponíveis
aws ec2 describe-instance-types

# Criar uma instância EC2 (exemplo simplificado)
aws ec2 run-instances \
  --image-id ami-1234567890abcdef0 \
  --count 1 \
  --instance-type t2.micro \
  --key-name MinhaChave \
  --security-group-ids sg-12345678 \
  --subnet-id subnet-12345678
