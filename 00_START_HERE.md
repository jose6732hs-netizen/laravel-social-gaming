# 🎮 Laravel Social Gaming Engine (Lite 13)

## 👋 Bem-vindo!

Este é o **Laravel Social Gaming Engine** - uma plataforma completa de jogos sociais com suporte a Sportsbook e múltiplos gateways de pagamento.

---

## ⚡ Quick Links (Clique para ir)

| Novo no projeto? | Quer entender a estrutura? | Desenvolvendo? |
|------------------|---------------------------|----------------|
| 👉 [SETUP_QUICKSTART.md](SETUP_QUICKSTART.md) | 👉 [ARCHITECTURE.md](ARCHITECTURE.md) | 👉 [DEV_GUIDE.md](DEV_GUIDE.md) |
| **10-15 min de setup** | **Visão geral técnica** | **Padrões e convenções** |

---

## 🎯 O que é este Projeto?

**Laravel Social Gaming (Lite 13)** é um motor de jogos de arcade social com:

✅ **Jogos Arcade** - Slots, Crash, Mines, etc com RNG certificável  
✅ **Sportsbook** - Integração com TheOddsAPI e ParalayAPI  
✅ **Pagamentos** - Stripe, PayPal, Crypto (XtoPay), Transferência bancária  
✅ **Multiplayer** - WebSockets para resultados em tempo real  
✅ **Admin Dashboard** - Gerenciar jogos, apostas, pagamentos  
✅ **Shared Hosting Pronto** - Roda em cPanel sem root access  

---

## 🚀 Iniciar em 3 Passos

### 1️⃣ Pré-requisitos (Verificar)

```bash
php -v          # PHP 8.4+
composer -V     # Composer 2.x
node -v         # Node.js 16+
mysql -V        # MySQL 8.0+
```

**Não tem?** Siga o [SETUP_QUICKSTART.md](SETUP_QUICKSTART.md#instalando-dependências)

### 2️⃣ Setup (Executar)

```bash
# Clonar (se ainda não fez)
git clone -b lite-13 https://github.com/gamingdotme/laravel-social-gaming.git
cd laravel-social-gaming/casino

# Configurar
cp .env.example .env
php artisan key:generate

# Editar .env com suas credenciais de banco de dados
nano .env

# Instalar
composer install
cd .. && npm install && npm run build && cd casino

# Migrar
php artisan migrate
```

### 3️⃣ Rodar

**Terminal 1:**
```bash
php artisan serve
# http://localhost:8000
```

**Terminal 2:**
```bash
npm run dev
```

✅ **Pronto!** Acesse http://localhost:8000

---

## 📂 Estrutura Rápida

```
casino/                    👈 App Laravel principal
├── app/Models/           # Database models
├── app/Http/Controllers/ # Controllers & API
├── app/Services/         # Business logic
├── resources/views/      # Blade templates
├── database/migrations/  # Schema
├── public/               # Assets públicos
└── routes/               # Routes (web.php, api.php)

frontend/                 # Frontend customizado (opcional)
minimal/                  # Tema alternativo
socket_config.json        # WebSocket config
```

Veja [ARCHITECTURE.md](ARCHITECTURE.md) para detalhes.

---

## 🎓 Documentação

### Para Iniciantes
- 📖 **[SETUP_QUICKSTART.md](SETUP_QUICKSTART.md)** - Setup passo-a-passo (10 min)
- 📋 **[ARCHITECTURE.md](ARCHITECTURE.md)** - Visão geral técnica

### Para Desenvolvedores
- 👨‍💻 **[DEV_GUIDE.md](DEV_GUIDE.md)** - Padrões de código e convenções
- 📘 **[DEVELOPERS.md](DEVELOPERS.md)** - Documentação original (no repo)
- 📄 **[README.md](README.md)** - Informações gerais

---

## 🛠️ Tarefas Comuns

### Criar um Novo Jogo
```bash
# Model + Migration
php artisan make:model Game -m

# Editar migration e model
# Depois migrate
php artisan migrate

# Controller
php artisan make:controller GameController
```

### Criar API Endpoint
```bash
# Controller API
php artisan make:controller Api/GameController

# Editar routes/api.php
# Testei com Postman
```

### Trabalhar com Banco de Dados
```bash
# Entrar no DB interativo
php artisan tinker

# Criar usuário
User::create(['name' => 'Admin', 'email' => 'admin@test.com', 'password' => bcrypt('password')])

# Listar
User::all()
```

### Debug
```bash
# Ver logs
tail -f storage/logs/laravel.log

# Artisan commands
php artisan list

# Ver rotas
php artisan route:list

# Ver migrations
php artisan migrate:status
```

---

## 🔐 Login (Teste)

Após `php artisan migrate && php artisan db:seed`:

- **Email:** admin@example.com
- **Senha:** password

Ou crie um user manualmente:
```bash
php artisan tinker
User::create(['name' => 'Admin', 'email' => 'admin@test.com', 'password' => bcrypt('password')])
```

---

## ⚙️ Configuração Importante

### `.env` - Dados Críticos

```env
# Database
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=laravel_gaming
DB_USERNAME=root
DB_PASSWORD=

# App
APP_URL=http://localhost:8000
APP_TIMEZONE=UTC

# APIs (opcional)
PARLAY_API_KEY=
ODS_API_KEY=
STRIPE_KEY=
XTOPAY_TOKEN=
```

**⚠️ NUNCA commitar `.env` com credenciais!**

---

## 🐛 Troubleshooting

| Erro | Solução |
|------|---------|
| "Class 'PDO' not found" | Instale PHP MySQL extension |
| "Connection refused" | MySQL não está rodando |
| "npm: command not found" | Instale Node.js |
| Assets 404 | Execute `npm run build` |
| Port 8000 ocupada | Use `php artisan serve --port=8001` |

Mais em [SETUP_QUICKSTART.md#troubleshooting](SETUP_QUICKSTART.md#-troubleshooting)

---

## 🌟 Principais Recursos

### Jogos
- 🎰 Múltiplos fornecedores de jogos
- 🎲 RNG certificável
- 📊 Histórico e estatísticas

### Sportsbook
- ⚽ Fixtures em tempo real
- 📈 Múltiplos mercados
- 🪙 Apostas simples e parlays

### Pagamentos
- 💳 Stripe Checkout
- 🅿️ PayPal Orders
- 🪙 Crypto (XtoPay)
- 🏦 Bank Transfer manual

### Admin
- 🎮 Gerenciar jogos
- 💰 Revisar pagamentos
- 📊 Dashboard com estatísticas
- ⚙️ Configurações de API

---

## 📞 Suporte & Comunidade

- **Discord:** https://discord.gg/nYHGyQ5q
- **GitHub:** https://github.com/gamingdotme/laravel-social-gaming
- **Sponsor:** https://github.com/sponsors/promexdotme

---

## 📋 Checklist de Setup

```
□ PHP 8.4+ instalado
□ Composer instalado
□ Node.js instalado
□ MySQL rodando
□ Repositório clonado
□ .env configurado
□ composer install executado
□ npm install && npm run build executado
□ php artisan key:generate executado
□ php artisan migrate executado
□ php artisan serve rodando
□ npm run dev rodando (outro terminal)
□ Conseguir acessar http://localhost:8000
```

---

## 🎓 Próximos Passos

1. **Completar Setup:** Siga [SETUP_QUICKSTART.md](SETUP_QUICKSTART.md)
2. **Entender Arquitetura:** Leia [ARCHITECTURE.md](ARCHITECTURE.md)
3. **Começar a Desenvolver:** Siga [DEV_GUIDE.md](DEV_GUIDE.md)
4. **Explorar Código:** Veja `casino/app/` para exemplos

---

## 📄 Licença

MIT - Veja [LICENSE](LICENSE)

---

## 🚀 Ready?

👉 **[Comece aqui: SETUP_QUICKSTART.md](SETUP_QUICKSTART.md)**

Boa sorte! 🎮
