# Variáveis de ambiente

## Índice

- [Variáveis de ambiente](#variáveis-de-ambiente)
  - [Índice](#índice)
  - [🚫 O que *não* fazer](#-o-que-não-fazer)
  - [✅ O que *deveria* ser feito](#-o-que-deveria-ser-feito)
    - [1. **Variáveis de ambiente reais (.env ou CI/CD)**](#1-variáveis-de-ambiente-reais-env-ou-cicd)
    - [2. **Arquivos `.env` separados por ambiente (usado apenas localmente)**](#2-arquivos-env-separados-por-ambiente-usado-apenas-localmente)
    - [3. **Configuração via CI/CD**](#3-configuração-via-cicd)
    - [4. **Ferramentas de gestão de segredos**](#4-ferramentas-de-gestão-de-segredos)
    - [5. **Separação clara de config x código**](#5-separação-clara-de-config-x-código)
  - [✅ Resumo da boa prática](#-resumo-da-boa-prática)


Excelente pergunta — e sim, **não deveria** estar tudo em um único `envs.json` dentro do repositório 😅.
Isso é **uma má prática** de segurança e gestão de ambientes.

Vamos detalhar o **jeito certo de o Git (ou melhor, o pipeline CI/CD)** controlar as variáveis de ambiente (como conexões, senhas e tokens) em cada ambiente:

---

## 🚫 O que *não* fazer

Um arquivo como este dentro do projeto:

```json
{
  "DEV": {
    "DB_URL": "mongodb://dev_user:dev_pass@localhost/devdb"
  },
  "QA": {
    "DB_URL": "mongodb://qa_user:qa_pass@localhost/qadb"
  },
  "PRD": {
    "DB_URL": "mongodb://prd_user:prd_pass@localhost/prddb"
  }
}
```

mesmo que esteja em `.gitignore`, **é perigoso** porque:

* Pode vazar via log, backup ou erro de configuração.
* Senhas ficam expostas em texto puro.
* O versionamento cria histórico dessas credenciais.
* É fácil alguém errar o deploy de um ambiente com credenciais erradas.

---

## ✅ O que *deveria* ser feito

### 1. **Variáveis de ambiente reais (.env ou CI/CD)**

Cada ambiente (DEV, QA, UAT, PRD) deve ter suas próprias variáveis definidas no **ambiente de execução**, não no código-fonte.

Exemplo (em um servidor ou pipeline):

```bash
# DEV
export DB_URL="mongodb://dev_user:dev_pass@localhost/devdb"
export JWT_SECRET="devsecret"
```

```bash
# QA
export DB_URL="mongodb://qa_user:qa_pass@localhost/qadb"
export JWT_SECRET="qasecret"
```

O Node.js acessa via `process.env.DB_URL`.

---

### 2. **Arquivos `.env` separados por ambiente (usado apenas localmente)**

Durante desenvolvimento, é comum ter:

```
.env.development
.env.qa
.env.production
```

E um `.env` padrão com valores genéricos.
Esses arquivos **não devem ir para o Git**, apenas servem para testes locais.

Você pode carregar o correto via script:

```bash
npm run start:dev
npm run start:qa
npm run start:prd
```

e no `package.json`:

```json
{
  "scripts": {
    "start:dev": "NODE_ENV=development node app.js",
    "start:qa": "NODE_ENV=qa node app.js",
    "start:prd": "NODE_ENV=production node app.js"
  }
}
```

Com o pacote [dotenv](https://www.npmjs.com/package/dotenv):

```js
require('dotenv').config({ path: `.env.${process.env.NODE_ENV}` });
```

---

### 3. **Configuração via CI/CD**

No GitLab, GitHub Actions, Jenkins, Azure DevOps, etc.
→ Cada ambiente do pipeline tem seus próprios **secrets** configurados.

Exemplo no GitHub Actions:

```yaml
env:
  NODE_ENV: production
  DB_URL: ${{ secrets.DB_URL }}
  JWT_SECRET: ${{ secrets.JWT_SECRET }}
```

Assim, quando faz o deploy, o ambiente “PRD” recebe as variáveis corretas.

---

### 4. **Ferramentas de gestão de segredos**

Em produção, use um **Vault** para segurança real:

* 🛡️ **AWS Secrets Manager**
* 🗝️ **HashiCorp Vault**
* 🔒 **Azure Key Vault**
* 🔑 **Google Secret Manager**

A aplicação acessa as credenciais dinamicamente via API segura, e não por arquivo local.

---

### 5. **Separação clara de config x código**

O ideal é seguir os [**12-Factor App principles**](https://12factor.net/config), principalmente o fator **Configuração**:

> “Armazene configuração no ambiente, não no código.”

---

## ✅ Resumo da boa prática

| Tipo de ambiente | Onde ficam as credenciais          | Forma de acesso |
| ---------------- | ---------------------------------- | --------------- |
| **DEV local**    | `.env.development` (não commitado) | `dotenv`        |
| **QA / UAT**     | Secrets no pipeline CI/CD          | `process.env`   |
| **PRD**          | Secret Manager ou Vault            | SDK/API segura  |
| **Nunca no Git** | `envs.json`, `config.js`, etc.     | 🚫 Inseguro     |

---

Se quiser, posso te mostrar **um exemplo completo** de estrutura Node.js segura com `dotenv`, pipeline e integração com Secret Manager.
Quer que eu monte esse exemplo completo para ti?
