# 💻 EC2 – Amazon Elastic Compute Cloud

## O que é?

* EC2 (Elastic Compute Cloud) é um serviço da AWS que permite criar **servidores virtuais** na nuvem.
* Você pode escolher o **sistema operacional, tipo de instância (CPU, memória), armazenamento e rede**.

---

## Principais conceitos

### 🧱 Tipos de instância

* Definem a **capacidade de processamento e memória**.
* Exemplos:

  * `t2.micro`: uso geral, gratuito (Free Tier)
  * `m5.large`: uso geral com mais desempenho
  * `c5`: para aplicações que exigem alto poder de CPU

### 📦 AMI (Amazon Machine Image)

* É a **imagem usada para criar a instância EC2** (ex: Ubuntu, Amazon Linux, etc.).
* Você pode **criar uma AMI personalizada** com a configuração do seu servidor para reutilizar ou replicar.
* AMIs armazenam tudo: SO, apps, configs, permissões.

💡 **Curiosidade**: Ao iniciar uma instância, uma cópia da AMI é usada. O original permanece inalterado.

### 🧰 Storage (armazenamento)

* Geralmente usa **EBS (Elastic Block Store)**.
* **Mesmo instâncias paradas geram custo** se o volume EBS ainda existir.
* Pode ser configurado como magnético, SSD ou provisionado por IOPS.

### 🔐 Chave SSH

* Arquivo `.pem` gerado na criação da instância.
* Usado para **acesso remoto via terminal (Linux)**.
* Importante: **salve esse arquivo com segurança**, não é possível baixar novamente.

### 🌐 Elastic IP

* Endereço IP fixo que pode ser associado a uma instância EC2.
* Se **não estiver associado**, pode gerar cobrança.

---

## Estados da instância

| Estado         | Custa?     | Observações                                 |
| -------------- | ---------- | ------------------------------------------- |
| **Running**    | ✅ Sim      | Instância ativa                             |
| **Stopped**    | ⚠️ Parcial | EBS continua gerando custo                  |
| **Terminated** | ❌ Não      | Instância e dados deletados permanentemente |

---

## Segurança

* As instâncias EC2 são protegidas por **Security Groups** (como um firewall).
* Você pode permitir ou bloquear portas, como:

  * `22`: SSH (Linux)
  * `80`: HTTP
  * `443`: HTTPS

---

## 💸 Dica de economia – Qual plano escolher?

Se seu uso será **contínuo por mais de um ano**, considere os seguintes planos para reduzir custos:

| Tipo de Instância | Descrição                                                                 |
| ----------------- | ------------------------------------------------------------------------- |
| **On-Demand**     | Ideal para uso curto ou testes, paga por hora/segundo.                    |
| **Reserved**      | Compromisso de 1 ou 3 anos. Custo **até 75% menor**.                      |
| **Spot**          | Usa capacidade ociosa com **grande economia**, mas pode ser interrompida. |

✅ **Dica**: Para cargas estáveis e previsíveis, **instâncias reservadas** são a melhor escolha!

---

## 🧠 Curiosidades e boas práticas

* **Cache de memória (RAM)**: Ao parar uma instância EC2, o conteúdo da RAM é perdido. Ao reiniciar, ela parte do zero.
* **Naming**: Use nomes e tags para organizar instâncias e saber qual é a função de cada uma.
* **Monitoramento**: Ative o **CloudWatch** para acompanhar o uso de CPU, disco e rede.
* **Backup**: Crie snapshots dos volumes EBS com frequência, especialmente antes de atualizações.
* **Região**: Escolher uma região mais próxima dos usuários melhora a latência e reduz custo de transferência de dados.

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
```
