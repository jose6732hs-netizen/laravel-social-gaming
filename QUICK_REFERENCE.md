# ⚡ Referência Rápida - Laravel Social Gaming

## 🎯 Comandos Essenciais

### Setup Inicial
```bash
cd casino
cp .env.example .env
php artisan key:generate
composer install
cd .. && npm install && npm run build && cd casino
php artisan migrate
```

### Rodar o Projeto
```bash
# Terminal 1 - Backend
php artisan serve

# Terminal 2 - Frontend
npm run dev
```

### Banco de Dados
```bash
php artisan migrate              # Rodar migrations
php artisan migrate:rollback     # Desfazer última
php artisan migrate:refresh      # Reset completo
php artisan migrate:status       # Status das migrations
php artisan db:seed              # Popular dados
php artisan tinker               # Console interativo
```

### Criar Componentes
```bash
php artisan make:model Game -m              # Model + Migration
php artisan make:controller GameController  # Controller
php artisan make:migration create_table     # Só migration
php artisan make:seeder GameSeeder          # Seeder
php artisan make:job SyncOdds               # Background job
php artisan make:event GameSpun             # Event
```

### Cache & Otimização
```bash
php artisan config:cache        # Cache de config
php artisan route:cache         # Cache de rotas
php artisan view:cache          # Cache de views
php artisan cache:clear         # Limpar cache
php artisan config:clear        # Limpar config
php artisan optimize            # Otimizar tudo
```

### Debug
```bash
php artisan route:list          # Listar rotas
php artisan tinker              # REPL
tail -f storage/logs/laravel.log # Ver logs
php artisan telescope           # Debugbar alternativo
```

---

## 📁 Estrutura de Pastas Chave

```
casino/
├── app/Models/                    # Eloquent Models
├── app/Http/Controllers/          # Controllers
├── app/Http/Requests/             # Form validation
├── app/Services/                  # Business logic
├── app/Jobs/                      # Queue jobs
├── resources/views/               # Blade templates
│   ├── frontend/                  # Customer UI
│   └── admin/                     # Admin panel
├── routes/
│   ├── web.php                    # Web routes
│   └── api.php                    # API routes
├── database/
│   ├── migrations/                # DB schema
│   └── seeders/                   # Initial data
├── public/
│   ├── css/                       # Stylesheets
│   └── js/                        # JavaScript
└── .env                           # Config (não commitar!)
```

---

## 🔐 Autenticação Rápida

### Login
```blade
@auth
    <p>Olá, {{ auth()->user()->name }}!</p>
@else
    <a href="{{ route('login') }}">Login</a>
@endauth
```

### Em Controllers
```php
if (auth()->check()) {
    $user = auth()->user();
}

$user = Auth::user();
```

### Proteger Rotas
```php
// routes/web.php
Route::middleware('auth')->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'show']);
});
```

---

## 📡 Eloquent Query Rápidas

```php
// CRUD
User::all();                           // Todos
User::find($id);                       // Por ID
User::where('email', $email)->first(); // Primeiro match
User::create(['name' => 'John']);      // Criar
$user->update(['name' => 'Jane']);     // Atualizar
$user->delete();                       // Deletar

// Relationships
$user->games;                          // Acesso relacionamento
$game->users();                        // Query relacionamento
$user->games()->attach($game);         // Attach

// Aggregates
User::count();                         // Contar
User::where('active', true)->count();  // Contar com filtro
User::pluck('email');                  // Extrair coluna
User::min('balance');                  // Mínimo
User::max('balance');                  // Máximo
User::avg('balance');                  // Média
User::sum('balance');                  // Soma

// Pagination
User::paginate(15);                    // 15 por página
User::simplePaginate(15);              // Paginação simples
```

---

## 🎨 Blade Template Snippets

```blade
{{-- Variáveis --}}
{{ $user->name }}
{{ $user->name ?? 'Anônimo' }}

{{-- Loops --}}
@foreach($users as $user)
    <p>{{ $user->name }}</p>
@endforeach

{{-- Condicionais --}}
@if($user->isAdmin())
    Admin
@elseif($user->isModerator())
    Moderador
@else
    Usuário
@endif

{{-- Auth --}}
@auth
    Autenticado
@else
    Não autenticado
@endauth

{{-- Forms --}}
<form action="{{ route('games.store') }}" method="POST">
    @csrf
    @method('POST')
    <input type="text" name="name" value="{{ old('name') }}">
    @error('name')
        <span>{{ $message }}</span>
    @enderror
</form>

{{-- Includes --}}
@include('components.game-card', ['game' => $game])

{{-- Slots --}}
<x-button>Clique aqui</x-button>
```

---

## 🔗 API Endpoints Comuns

```php
// routes/api.php

// Jogos
Route::get('/games', [GameController::class, 'index']);
Route::get('/games/{game}', [GameController::class, 'show']);
Route::post('/games/{game}/spin', [GameController::class, 'spin']);

// Usuário
Route::middleware('auth:sanctum')->group(function () {
    Route::get('/user', [UserController::class, 'show']);
    Route::post('/user/deposit', [PaymentController::class, 'deposit']);
});

// Sportsbook
Route::get('/sportsbook/fixtures', [SportsController::class, 'fixtures']);
Route::post('/sportsbook/bet', [BetController::class, 'place']);
```

---

## 💾 Salvando Dados Rapidamente

### Model Create
```php
Game::create([
    'name' => 'Slot Clássico',
    'rtp' => 95,
    'provider' => 'NetEnt',
]);
```

### Bulk Insert
```php
Game::insert([
    ['name' => 'Game 1', 'rtp' => 95],
    ['name' => 'Game 2', 'rtp' => 96],
]);
```

### Update
```php
User::where('active', false)->update(['active' => true]);
```

### Delete
```php
Game::where('active', false)->delete();
```

---

## 🚀 API Response Rápida

```php
// JSON response
return response()->json([
    'success' => true,
    'data' => $game,
    'message' => 'Game loaded',
], 200);

// Com headers
return response()->json($data, 200, ['X-Custom' => 'value']);

// Redirect com message
return redirect()->route('games.show', $game)
    ->with('success', 'Game atualizado!');
```

---

## 🧪 Testing Rápido

```php
// tests/Feature/GameTest.php
public function test_can_spin_game()
{
    $user = User::factory()->create();
    $game = Game::factory()->create();

    $response = $this->actingAs($user)
        ->post("/api/games/{$game->id}/spin", ['amount' => 100]);

    $response->assertStatus(200);
    $response->assertJsonStructure(['result', 'payout']);
}
```

### Rodar Tests
```bash
php artisan test                        # Todos
php artisan test tests/Feature/GameTest # Específico
php artisan test --coverage             # Com coverage
```

---

## 📊 Logging Rápido

```php
use Illuminate\Support\Facades\Log;

Log::debug('Debug message', ['data' => $data]);
Log::info('Info message');
Log::warning('Warning!');
Log::error('Error!');

// Ver logs
tail -f storage/logs/laravel.log
```

---

## 🔄 Middleware Rápido

```php
// routes/web.php
Route::middleware('auth')->group(function () {
    // Protegido por auth
});

// app/Http/Middleware/CheckAdmin.php
public function handle($request, Closure $next)
{
    if (!$request->user()->isAdmin()) {
        return redirect('/');
    }
    return $next($request);
}
```

---

## 🎯 Troubleshooting Rápido

| Problema | Solução |
|----------|---------|
| "Class not found" | Rodar `composer dump-autoload` |
| Migrations não rodaram | Verificar `.env` DB config |
| Assets 404 | Rodar `npm run build` |
| Port 8000 ocupada | Usar `php artisan serve --port=8001` |
| Permissão negada | Rodar `chmod -R 775 storage bootstrap` |
| Cache velho | Rodar `php artisan cache:clear` |

---

## 📞 Atalhos Úteis

```bash
# Entrar no REPL
php artisan tinker

# Criar usuário rápido
>>> User::create(['name' => 'Admin', 'email' => 'admin@test.com', 'password' => bcrypt('123456')])

# Verificar migrations
>>> DB::table('migrations')->pluck('migration')

# Listar models
>>> app_path('Models')
```

---

## 🌐 Ambiente .env Essencial

```env
# Banco
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=laravel_gaming
DB_USERNAME=root
DB_PASSWORD=

# App
APP_URL=http://localhost:8000
APP_DEBUG=true  # false em production!
APP_TIMEZONE=UTC

# Mail (opcional)
MAIL_FROM_ADDRESS=noreply@laravel.test
MAIL_FROM_NAME="${APP_NAME}"

# APIs (opcional)
STRIPE_KEY=sk_test_...
XTOPAY_TOKEN=...
```

---

**Bookmark esta página!** 🔖

Mais detalhes: Veja `DEV_GUIDE.md` ou `ARCHITECTURE.md`
