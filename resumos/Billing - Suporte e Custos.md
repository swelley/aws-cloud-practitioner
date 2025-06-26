# 💰 Billing, Suporte e Custos na AWS

Este documento é um resumo sobre como **monitorar, controlar e entender custos na AWS**, incluindo o uso do **Free Tier**, alertas com **AWS Budgets** e os **planos de suporte** disponíveis.

---

## 📊 Monitoramento de Uso e Cobrança na AWS

A AWS oferece diversas ferramentas para **acompanhamento de gastos e uso de recursos**, disponíveis no console de faturamento (Billing).

### 🧰 Principais ferramentas:

- **Billing Dashboard**: visão geral de faturas, pagamentos e uso.
- **Cost Explorer**: análise detalhada dos custos por serviço, região, conta, tag etc.
- **AWS Budgets**: define limites de custo e uso, com alertas.
- **Usage Reports**: exporta relatórios detalhados em CSV.

> ✅ Use tags em recursos para identificar projetos e facilitar a análise de custos.

---

## 🔔 Alertas de Custo com AWS Budgets

### Como funciona:
- Você define um **limite de custo (ex: R$ 50/mês)**.
- Configura alertas por **e-mail** se o uso ou previsão atingir certo percentual (ex: 80%).

### Exemplo prático:
1. Crie um budget de custo de **R$ 50/mês**.
2. Configure alerta se atingir **80% do orçamento**.
3. Receba notificação e revise os serviços ativos.

> 💡 Dica: monitore também **uso de serviços gratuitos (Free Tier)**, pois eles podem gerar cobranças se ultrapassarem os limites.

---

## 🧾 Planos de Suporte da AWS

A AWS oferece **4 níveis de suporte**, com diferentes benefícios. A escolha depende do seu uso, orçamento e criticidade do ambiente.

| Plano        | Indicado para | Custo                   | Benefícios principais |
|--------------|----------------|--------------------------|------------------------|
| **Basic**    | Usuários Free Tier e testes | Gratuito                  | Suporte à conta, documentação e fóruns |
| **Developer**| Desenvolvimento e testes     | A partir de $29/mês      | Suporte por e-mail em horário comercial |
| **Business** | Produção        | A partir de $100/mês     | Suporte 24x7, chat, API de suporte |
| **Enterprise** | Missão crítica | Sob consulta (alto custo) | Gerente técnico designado, suporte proativo |

> ⚠️ O suporte técnico **não está incluso no plano gratuito (Basic)**. Para abrir chamados técnicos é necessário assinar o Developer ou superior.

---

## 🆓 Free Tier da AWS

O **Free Tier** é a camada gratuita da AWS que permite **testar serviços sem custo** dentro de certos limites.

### Tipos de ofertas:

| Tipo              | Duração        | Exemplo                         |
|-------------------|----------------|----------------------------------|
| 12 meses grátis   | 12 meses após criação da conta | EC2 (750h/mês t2.micro), S3 (5 GB), RDS (750h/mês db.t2.micro) |
| Sempre gratuito   | Indefinido     | Lambda (1 milhão de execuções), DynamoDB (25 GB), S3 Glacier (10 GB) |
| Gratuito por tempo limitado | Promoções específicas | Varia por serviço |

### Dica para iniciantes:
- **Acompanhe seu uso no console de Billing**.
- **Habilite alertas com Budgets para evitar surpresas**.
- Ao final dos 12 meses, **os serviços deixam de ser gratuitos automaticamente**, e podem gerar cobranças.

---

## ✅ Conclusão

- Acompanhe custos e uso com **Cost Explorer e AWS Budgets**.
- O **Free Tier** é ótimo para praticar, mas deve ser monitorado.
- Escolha o **plano de suporte** ideal conforme sua necessidade.
- Use boas práticas de **tags, alertas e encerramento de recursos não utilizados** para manter o controle financeiro da sua conta AWS.

> 💡 Dica final: acesse o [AWS Billing Dashboard](https://console.aws.amazon.com/billing/home) toda semana se estiver usando o Free Tier.
