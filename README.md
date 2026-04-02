# 💰 FinançasCLAR

Dashboard financeiro pessoal com integração automática ao C6 Bank via Pluggy (Open Finance).

> Sem servidor, sem banco de dados, sem mensalidade. Roda 100% no browser.

---

## Funcionalidades

- **Dashboard** com KPIs mensais, gráfico de gastos por categoria e evolução mensal
- **Lançamentos** manuais com categorias, filtros e resumo do mês
- **Dívidas** com visualização de utilização de limite e simulador de quitação
- **Integração automática** com C6 Bank via Pluggy (Open Finance regulado pelo BACEN)
- Dados salvos localmente no browser (localStorage)

---

## Stack

| Camada | Tecnologia |
|---|---|
| Frontend | HTML + CSS + JS puro |
| Gráficos | Chart.js |
| Hospedagem | GitHub Pages (gratuito) |
| Proxy seguro | Cloudflare Workers (gratuito) |
| Dados bancários | Pluggy API (Open Finance) |

---

## Estrutura

```
financas/
├── index.html        # App completo (single file)
└── README.md
```

---

## Setup completo

### 1. Pluggy (dados bancários)

1. Crie uma conta em [dashboard.pluggy.ai](https://dashboard.pluggy.ai)
2. Vá em **Applications** → crie um app
3. Copie o **Client ID** e o **Client Secret**

### 2. Cloudflare Worker (proxy seguro)

O Worker impede que suas credenciais fiquem expostas no frontend.

```bash
# Instala o Wrangler
npm install -g wrangler

# Login na Cloudflare
wrangler login

# Na pasta do worker
wrangler deploy

# Salva as credenciais como secrets (nunca no código)
wrangler secret put PLUGGY_CLIENT_ID
wrangler secret put PLUGGY_CLIENT_SECRET
```

Worker disponível em:
```
https://pluggy-proxy.SEU-SUBDOMINIO.workers.dev
```

Testa com:
```
https://pluggy-proxy.SEU-SUBDOMINIO.workers.dev/health
```

Resposta esperada: `{"status":"ok","msg":"Pluggy Worker rodando"}`

### 3. GitHub Pages

1. Cria um repositório público no GitHub
2. Sobe o `index.html`
3. Settings → Pages → Source: branch `main`, pasta `/ (root)`
4. App disponível em `https://SEU-USUARIO.github.io/REPO`

### 4. Conectar o banco

1. Abre o app no browser
2. Menu **Integração**
3. Cola Client ID e Secret → clica **Testar conexão**
4. Clica **Conectar C6** → autoriza no app do C6 Bank
5. Transações importadas automaticamente a cada 24h

---

## Rotas do Worker

| Método | Rota | Descrição |
|---|---|---|
| GET | `/health` | Verifica se o Worker está rodando |
| POST | `/token` | Gera connect token para o widget Pluggy |
| GET | `/accounts?itemId=xxx` | Lista contas do item conectado |
| GET | `/transactions?accountId=xxx&from=yyyy-mm-dd&to=yyyy-mm-dd` | Busca transações |

---

## Segurança

- Credenciais da Pluggy ficam **somente** nos secrets do Cloudflare Worker
- O frontend nunca vê o Client ID ou Client Secret
- Autorização bancária segue o fluxo oficial do Open Finance (BACEN)
- Dados financeiros ficam **só no seu browser** (localStorage) — nenhum servidor externo armazena seus dados

---

## Bancos suportados via Pluggy

C6 Bank · Nubank · Banco do Brasil · Itaú · Bradesco · Santander · Sicredi · Inter · e mais

---

## Licença

Uso pessoal. Feito por Marcelo Braga Veras.
