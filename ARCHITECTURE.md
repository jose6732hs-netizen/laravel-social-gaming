# 🏗️ Arquitetura - Laravel Social Gaming (Lite 13)

## 📊 Visão Geral

```
┌─────────────────────────────────────────────────────────────┐
│                    NAVEGADOR DO USUÁRIO                      │
└────────────────────┬────────────────────────────────────────┘
                     │ HTTP/WebSocket
        ┌────────────┴────────────┐
        │                         │
    ┌───▼────────────┐      ┌────▼──────────┐
    │  FRONTEND      │      │  WEBSOCKET    │
    │  (Blade Views) │      │  (Socket.io)  │
    └───┬────────────┘      └────┬──────────┘
        │                         │
        └────────────┬────────────┘
                     │
        ┌────────────▼────────────┐
        │   LARAVEL BACKEND       │
        │   (casino/app)          │
        ├─────────────────────────┤
        │ Routes, Controllers     │
        │ Models, Services        │
        │ Middleware, Policies    │
        └────────────┬────────────┘
                     │
        ┌────────────▼────────────┐
        │   MYSQL DATABASE        │
        │   (Migrations)          │
        └─────────────────────────┘
                     │
    ┌────────────────┼────────────────┐
    │                │                │
┌───▼────────┐  ┌────▼────────┐  ┌──▼──────────┐
│  PAYMENT   │  │  SPORTSBOOK │  │   GAMES     │
│  GATEWAYS  │  │  PROVIDERS  │  │   ASSETS    │
│ (Stripe,   │  │ (The Odds,  │  │  (CDN)      │
│  PayPal,   │  │  Polyapp)   │  │             │
│  XtoPay)   │  └─────────────┘  └─────────────┘
└────────────┘
```

---

## 📁 Estrutura de Pastas

### Raiz do Projeto

```
laravel-social-gaming/
├── casino/                          # 👈 App Laravel principal
│   ├── app/
│   │   ├── Http/
│   │   │   ├── Controllers/        # Controllers
│   │   │   ├── Requests/           # Form Requests
│   │   │   ├── Resources/          # JSON Resources
│   │   │   └── Middleware/         # Middleware
│   │   ├── Models/                 # Eloquent Models
│   │   ├── Services/               # Business Logic
│   │   │   ├── Payments/           # PaymentDriverInterface
│   │   │   └── Sportsbook/         # Sportsbook Logic
│   │   ├── Jobs/                   # Queue Jobs
│   │   ├── Events/                 # Events
│   │   └── Console/                # Artisan Commands
│   ├── resources/
│   │   ├── views/                  # Blade Templates
│   │   │   ├── frontend/           # Theme frontend
│   │   │   └── admin/              # Admin dashboard (Liteback)
│   │   └── css/
│   │   └── js/
│   ├── routes/
│   │   ├── web.php                 # Web routes
│   │   ├── api.php                 # API routes
│   │   └── console.php             # Console commands
│   ├── database/
│   │   ├── migrations/             # Migrations
│   │   ├── seeders/                # Seeders
│   │   └── factories/              # Model factories
│   ├── public/
│   │   ├── minimal/                # Tema minimal (CSS/JS)
│   │   ├── Default/                # Tema padrão
│   │   └── uploads/                # User uploads
│   ├── .env.example                # Template .env
│   ├── composer.json               # Dependências PHP
│   ├── package.json                # Dependências Node.js
│   └── vite.config.js              # Configuração Vite
│
├── frontend/                         # Frontend customizado (opcional)
├── minimal/                          # Tema minimal alternativo
├── socket_config.json                # Configuração WebSocket
├── socket_config2.json               # Config alternativa
├── package.json                      # Root dependencies
└── README.md
```

---

## 🔄 Fluxo de Dados

### 1. Autenticação

```
Usuário
    ↓
Formulário de Login (Blade)
    ↓
AuthController@login
    ↓
Laravel Auth Guard
    ↓
Session/Token
    ↓
Dashboard
```

### 2. Jogos

```
Usuário clica em Jogo
    ↓
GameController@show
    ↓
Retorna view + game_id
    ↓
JavaScript carrega Game Asset (CDN ou Local)
    ↓
WebSocket conecta (socket_config.json)
    ↓
GameEngine RNG Logic
    ↓
Resultado → Laravel API → Database
    ↓
Atualizar saldo do usuário
```

### 3. Sportsbook

```
Admin executa Sync Job
    ↓
TheOddsAPI / ParalayAPI
    ↓
Parser converte odds (American → Decimal)
    ↓
Salva fixtures/markets em DB
    ↓
Usuario acessa Sportsbook
    ↓
Tela lista: Jogos, Mercados, Odds
    ↓
Usuário coloca aposta
    ↓
BetEngine valida + salva
    ↓
Webhook recebe resultado
    ↓
Settlement (vitória/derrota)
    ↓
Saldo atualizado
```

### 4. Pagamentos

```
Usuário clica "Depositar"
    ↓
PaymentController@initiate
    ↓
PaymentRouter escolhe gateway:
    ├─ Stripe → Checkout Session
    ├─ PayPal → Orders API
    ├─ XtoPay → Crypto Address
    └─ Manual → Proof upload
    ↓
Redireciona para gateway
    ↓
Usuário completa pagamento
    ↓
Webhook retorna para /webhooks/payment
    ↓
Valida assinatura
    ↓
Credita saldo no account
```

---

## 🗄️ Modelos Principais

### User
```
id, name, email, password, balance, status
```

### Game
```
id, name, slug, rtp, provider, active, icon
```

### Bet / GameResult
```
id, user_id, game_id, amount, result, payout, created_at
```

### SportsBook (Fixtures, Markets, Odds)
```
Fixture: id, sport, league, team_a, team_b, commence_time, status
Market: id, fixture_id, market_type (win/draw/over_under), odds
```

### Payments
```
id, user_id, gateway, amount, status, reference, webhook_data
```

---

## 🔐 Segurança

### Middleware

- **ForceShopOne**: Garante que tudo usa `shop_id = 1`
- **Authenticate**: Protege rotas autenticadas
- **CheckAdmin**: Valida admin access
- **RateLimiter**: Previne abuso

### Verificações

- ✅ Senha hasheada com `bcrypt`
- ✅ CSRF tokens em forms
- ✅ API Token validation
- ✅ Webhook signature verification
- ✅ SQL Injection prevention (Eloquent)

---

## 🎮 Game Engine

### RNG (Random Number Generator)

Localização: `app/Services/GameEngine/`

```php
class GameEngine {
    public function spin($bet_amount, $game_id) {
        $random = mt_rand(0, 10000); // Seed aleatório
        $result = $this->calculateResult($random, $game_id);
        return [
            'result' => $result,
            'payout' => $this->calculatePayout($result, $bet_amount)
        ];
    }
}
```

### Certificação

- Cada jogo tem **RTP (Return to Player)** configurável
- Resultados são determináveis para auditoria
- Logs completos em `game_results` table

---

## 💳 Gateways de Pagamento

### Implementação

Cada gateway deve implementar `PaymentDriverInterface`:

```php
interface PaymentDriverInterface {
    public function initiate($amount, $description);
    public function verify($webhook_data);
    public function refund($transaction_id);
}
```

### Drivers Disponíveis

1. **Stripe** → `StripeDriver`
   - Checkout Session
   - Webhook: `/webhooks/stripe`

2. **PayPal** → `PayPalDriver`
   - Orders API v2
   - Webhook: `/webhooks/paypal`

3. **XtoPay** → `XtoPayDriver`
   - Crypto deposits
   - Blockchains: TRON, Polygon, BSC, Ethereum
   - Webhook: `/webhooks/xtopay`

4. **Manual** → `ManualDriver`
   - Bank transfer proofs
   - Admin review queue

---

## ⚽ Sportsbook Engine

### Providers de Odds

1. **TheOddsAPI**
   - Fixture sync job
   - American odds → Decimal conversion
   - Timezone handling (UTC → app.timezone)

2. **ParalayAPI**
   - Pinnacle filter option
   - Multi-selection (parlay) bets
   - Settlement webhook

### Workflow

```
1. Admin triggers: Artisan > Sync Odds
2. TheOddsAPI fornece fixtures/markets
3. Parser valida + converte odds
4. Salva: fixtures, markets, odds
5. Frontend exibe atualizado
6. User seleciona + aposta
7. BetEngine cria bet
8. Webhook de resultado entra
9. Settlement: vitória/derrota
10. Saldo atualizado
```

---

## 📡 WebSocket (Socket.io)

### Configuração

Arquivo: `socket_config.json`

```json
{
  "websocket_url": "https://socket.377.live",
  "room": "game_{user_id}",
  "events": {
    "game_spin": "game:spin_result",
    "balance_update": "account:balance_updated"
  }
}
```

### Eventos

```javascript
// Cliente escuta
socket.on('game:spin_result', (result) => {
    updateBalance(result.payout);
});

// Servidor emite
io.to(`game_${user_id}`).emit('game:spin_result', result);
```

---

## 🚀 Deployment

### Shared Hosting (CPanel)

```
1. Upload files via FTP
2. Set .htaccess rewrite rules
3. Point socket_config.json to cloud instance
4. Configure .env com credenciais
5. Run php artisan migrate
6. Done!
```

### Self-Hosted VPS

```
1. Clone repo
2. composer install
3. npm install && npm run build
4. Configure .env + database
5. Run migrations
6. Point socket_config.json to local instance
7. Start: php artisan serve + npm run dev
```

---

## 🔧 Configurações Importantes

### `.env` Essencial

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

# APIs (Opcional)
PARLAY_API_KEY=sua_chave
ODS_API_KEY=sua_chave
STRIPE_KEY=sua_chave
XTOPAY_TOKEN=seu_token

# Game Mode
GAMES_CDN_MODE=cloud  # ou 'local'
```

---

## 📊 Base de Dados

Principais tabelas (criadas via migrations):

```
users                    # Usuários
games                    # Catálogo de jogos
game_results             # Histórico de spins
bets                     # Apostas (sportsbook)
fixtures                 # Jogos (sportsbook)
markets                  # Mercados (sportsbook)
odds                     # Odds (sportsbook)
transactions             # Payments log
shops                    # Lojas (single: shop_id=1)
```

---

## 🧪 Testando

```bash
# Unit tests
php artisan test

# Specific test
php artisan test tests/Unit/GameEngineTest.php

# Coverage
php artisan test --coverage
```

---

## 📚 Referência Rápida

| Recurso | Local | Comando |
|---------|-------|---------|
| Criar Controller | `app/Http/Controllers/` | `php artisan make:controller MyController` |
| Criar Model | `app/Models/` | `php artisan make:model MyModel -m` |
| Criar Migration | `database/migrations/` | `php artisan make:migration create_table` |
| Criar Seeder | `database/seeders/` | `php artisan make:seeder MySeeder` |
| Criar Job | `app/Jobs/` | `php artisan make:job MyJob` |
| Criar Event | `app/Events/` | `php artisan make:event MyEvent` |
| Listar Routes | Terminal | `php artisan route:list` |
| Listar Migrations | Terminal | `php artisan migrate:status` |
| Rollback Migrations | Terminal | `php artisan migrate:rollback` |

---

**Mais perguntas?** Veja `README.md` ou `DEVELOPERS.md`
