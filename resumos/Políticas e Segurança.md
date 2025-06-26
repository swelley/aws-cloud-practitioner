# 🔐 Políticas e Segurança na AWS

Este material apresenta os conceitos essenciais sobre **Políticas e Segurança na AWS**, com foco no entendimento das permissões, tipos de políticas, aplicação correta em usuários, grupos e funções, além de boas práticas e o **modelo de responsabilidade compartilhada**.

---

## 🎯 Objetivos

- Entender os tipos de políticas e sua aplicação.
- Aprender a aplicar políticas corretamente a usuários, grupos e funções.
- Compreender o modelo de responsabilidade compartilhada.
- Adotar boas práticas para proteger sua conta AWS.

---

## 📜 Tipos de Políticas IAM

### 🔹 Políticas Gerenciadas

- Criadas e mantidas pela **AWS** ou pelo próprio **cliente**.
- Podem ser reutilizadas em múltiplos usuários, grupos ou funções.
- Exemplo: `AmazonS3ReadOnlyAccess`, `AdministratorAccess`.

✅ **Vantagens**:
- Mais fáceis de manter.
- Permitem centralizar e padronizar permissões.

### 🔸 Políticas Inline

- Anexadas diretamente a **um único usuário, grupo ou função**.
- Não podem ser reaproveitadas.
- São recomendadas para **casos específicos e pontuais**.

⚠️ **Atenção**:
- Difíceis de rastrear e manter em grandes ambientes.
- Ideal apenas para permissões muito específicas e isoladas.

---

## 👥 Aplicação de Políticas: Usuários, Grupos e Funções

| Entidade  | Uso Principal | Características |
|-----------|---------------|-----------------|
| **Usuário** | Pessoas reais | Possuem credenciais (senha, chave de acesso) |
| **Grupo**  | Agrupamento de usuários | Permissões atribuídas coletivamente |
| **Função (Role)** | Serviços, aplicações, contas | Assumidas temporariamente com permissões específicas |

### 🧩 Exemplo de Aplicação:

- Crie um **grupo** chamado `AnalistasS3` com uma política gerenciada de acesso somente leitura ao S3.
- Adicione usuários reais a esse grupo.
- Crie uma **função IAM** com acesso limitado para um serviço EC2 acessar um bucket específico do S3.

---

## 🤝 Shared Responsibility Model (Modelo de Responsabilidade Compartilhada)

A segurança na nuvem é **compartilhada** entre a **AWS** e o **cliente**:

| Responsabilidade | AWS | Cliente |
|------------------|-----|---------|
| Físico           | ✅   | ❌      |
| Infraestrutura (rede, hardware, data centers) | ✅ | ❌ |
| Configuração de serviços | ❌ | ✅ |
| Gerenciamento de usuários, senhas e permissões | ❌ | ✅ |
| Criptografia de dados | ✅ (ferramentas) | ✅ (aplicação) |

🔐 **Resumo**:
- A AWS protege a **infraestrutura**.
- Você é responsável por **configurar e proteger seus dados e acessos**.

---

## 🔐 Dicas para Proteger sua Conta AWS

1. **Nunca utilize a conta root para tarefas diárias**.
2. **Ative MFA (autenticação multifator)** para todos os usuários, principalmente o root.
3. **Crie usuários individuais com permissões mínimas necessárias**.
4. **Use grupos para aplicar políticas comuns**.
5. **Evite usar políticas muito permissivas (`Action: *`)**.
6. **Acompanhe logs com o AWS CloudTrail**.
7. **Habilite alertas com o AWS Config e CloudWatch**.
8. **Rode periodicamente revisões de permissões (access advisor)**.
9. **Rotacione senhas e chaves regularmente**.
10. **Utilize tags para controlar e organizar permissões e recursos**.

---
## 🧠 Dicas e Exemplos de Segurança no Modelo de Responsabilidade do Cliente

Neste guia, você encontrará **dicas práticas com pequenos exemplos** de serviços da AWS em que a responsabilidade de segurança é **do cliente**, de acordo com o **Modelo de Responsabilidade Compartilhada**.

---

## 📌 Lembrete: o que é responsabilidade do cliente?

No modelo da AWS, **você é responsável por tudo que configurar dentro da nuvem**, incluindo:

- Controle de acesso (IAM)
- Criptografia de dados
- Atualizações de sistema (em alguns serviços)
- Configuração de segurança de rede
- Monitoramento e auditoria

---

## 🔐 Dicas e Exemplos por Serviço

### 🟢 Amazon S3 (armazenamento de objetos)

#### Sua responsabilidade:
- Controlar quem acessa seus buckets e objetos
- Ativar criptografia
- Evitar acesso público desnecessário

#### Exemplo de ação:
```json
{
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:*",
  "Resource": "arn:aws:s3:::meu-bucket/*",
  "Condition": {
    "Bool": {
      "aws:SecureTransport": "false"
    }
  }
}
```
## 🟡 Amazon EC2 (máquinas virtuais)
### Sua responsabilidade:

Atualizar o sistema operacional da instância
Instalar e manter antivírus/firewalls internos
Proteger as chaves de acesso (key pairs)
Configurar grupos de segurança (SG) corretamente
Dica:

Permitir acesso SSH (porta 22) somente de IPs confiáveis:
SG: permitir entrada apenas de 200.100.50.25/32 na porta 22
---
## 🔵 Amazon RDS (banco de dados gerenciado)
### Sua responsabilidade:

Controlar o acesso ao banco (usuários e senhas)
Gerenciar permissões com grupos de segurança e subnets privadas
Habilitar criptografia se necessário
Fazer backups automáticos se não estiver ativado por padrão
Dica:

Deixe o banco em uma subnet privada, sem acesso direto pela internet.
---
## 🔵 Amazon RDS (banco de dados gerenciado)
### Sua responsabilidade:

Controlar o acesso ao banco (usuários e senhas)
Gerenciar permissões com grupos de segurança e subnets privadas
Habilitar criptografia se necessário
Fazer backups automáticos se não estiver ativado por padrão
Dica:

Deixe o banco em uma subnet privada, sem acesso direto pela internet.

```
{
  "Effect": "Allow",
  "Action": "s3:PutObject",
  "Resource": "arn:aws:s3:::meu-bucket/diretorio-especifico/*"
}

```
📌 Permite que a função Lambda escreva somente em uma pasta específica no bucket.

---
## 🔶 IAM (controle de acesso)
### Sua responsabilidade:

Criar e aplicar políticas de forma segura
Ativar MFA
Acompanhar uso de credenciais
Dica:

Crie um grupo Desenvolvedores com acesso somente leitura ao S3 em ambiente de produção.

## 📌 Outras Ações Importantes do Cliente

Ativar CloudTrail para registrar atividades na conta.
Utilizar AWS Config para verificar desvios de segurança.
Usar AWS KMS para gerenciar criptografia de dados.
Definir alertas no CloudWatch para ações suspeitas.

## ✅ Conclusão

A AWS oferece ferramentas para manter seu ambiente seguro, mas cabe ao cliente configurá-las corretamente. Sempre que um serviço permitir acesso, execução, armazenamento ou configuração — você é o responsável por garantir que tudo esteja seguro, monitorado e controlado.

## 🧠 Pense sempre: “Esse serviço está com os acessos mínimos e criptografia ativada?” Se a resposta for "não sei", revise!

## 📚 Recursos Úteis

- [IAM User Guide (oficial AWS)](https://docs.aws.amazon.com/IAM/latest/UserGuide/)
- [Simulador de Políticas IAM](https://policysim.aws.amazon.com/)
- [Well-Architected Framework – Pilar de Segurança](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/)

---

> 🔒 A segurança na AWS começa com a forma como você controla quem tem acesso e o que pode ser feito. Entender e aplicar corretamente as políticas é o primeiro passo para ambientes seguros e bem administrados.
