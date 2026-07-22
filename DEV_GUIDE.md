# 👨‍💻 Guia do Desenvolvedor - Laravel Social Gaming

## 🎯 Antes de Começar

1. Leia `SETUP_QUICKSTART.md` e tenha o projeto rodando
2. Leia `ARCHITECTURE.md` para entender a estrutura
3. Leia `DEVELOPERS.md` no repositório original

---

## 🛠️ Ambiente de Desenvolvimento

### Iniciar Servidor de Desenvolvimento

**Terminal 1: Backend Laravel**
```bash
cd casino
php artisan serve
# Acesso: http://localhost:8000
```

**Terminal 2: Frontend Assets (Vite)**
```bash
cd casino
npm run dev
# Escuta mudanças em resources/css e resources/js
```

**Terminal 3: WebSocket (se usar local)**
```bash
# Descomente em casino/package.json scripts
npm run socket
```

---

## 📝 Convenções de Código

### PHP / Laravel

#### Controllers

```php
namespace App\Http\Controllers;

use App\Models\Game;
use Illuminate\View\View;

class GameController extends Controller
{
    // Listagem
    public function index(): View
    {
        $games = Game::where('active', true)
            ->paginate(15);
        
        return view('games.index', compact('games'));
    }

    // Detalhe
    public function show(Game $game): View
    {
        return view('games.show', compact('game'));
    }

    // Criar
    public function store()
    {
        $validated = request()->validate([
            'name' => 'required|string',
            'rtp' => 'required|numeric|between:0,100',
        ]);

        Game::create($validated);
        
        return redirect()->route('games.index')
            ->with('success', 'Jogo criado com sucesso!');
    }
}
```

#### Models

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\HasMany;

class Game extends Model
{
    protected $fillable = ['name', 'slug', 'rtp', 'provider', 'active'];
    
    protected $casts = [
        'active' => 'boolean',
        'rtp' => 'decimal:2',
    ];

    // Relacionamentos
    public function results(): HasMany
    {
        return $this->hasMany(GameResult::class);
    }

    // Scopes
    public function scopeActive($query)
    {
        return $query->where('active', true);
    }

    // Métodos
    public function getAverageRtpAttribute()
    {
        return $this->results()
            ->avg('payout') / $this->results()->avg('amount');
    }
}
```

#### Services (Business Logic)

```php
namespace App\Services\Games;

use App\Models\Game;
use App\Models\GameResult;
use Illuminate\Support\Facades\DB;

class GameEngine
{
    public function __construct(
        protected Game $game,
        protected float $bet_amount,
    ) {}

    public function spin(): array
    {
        // RNG Logic
        $random = mt_rand(0, 10000);
        $multiplier = $this->calculateMultiplier($random);
        $payout = $this->bet_amount * $multiplier;

        // Log result
        $result = GameResult::create([
            'game_id' => $this->game->id,
            'user_id' => auth()->id(),
            'bet_amount' => $this->bet_amount,
            'payout' => $payout,
            'multiplier' => $multiplier,
            'seed' => $random,
        ]);

        return [
            'result' => $result,
            'payout' => $payout,
            'new_balance' => auth()->user()->refresh()->balance,
        ];
    }

    private function calculateMultiplier(int $seed): float
    {
        $rtp = $this->game->rtp / 100;
        // Seu algoritmo RNG aqui
        return ($seed % 100 / 100) * 2 * $rtp;
    }
}
```

### JavaScript / Vue

```javascript
// resources/js/composables/useGame.js
import { ref, computed } from 'vue'
import { useToast } from '@vueuse/core'

export function useGame(gameId) {
    const loading = ref(false)
    const betAmount = ref(100)
    const result = ref(null)
    const { toast } = useToast()

    const spin = async () => {
        if (loading.value) return
        
        loading.value = true
        try {
            const response = await fetch(`/api/games/${gameId}/spin`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'X-CSRF-TOKEN': document.querySelector('meta[name="csrf-token"]').content,
                },
                body: JSON.stringify({ amount: betAmount.value }),
            })

            if (!response.ok) throw new Error('Spin failed')
            
            result.value = await response.json()
            toast.success(`Você ganhou: ${result.value.payout}!`)
        } catch (error) {
            toast.error(error.message)
        } finally {
            loading.value = false
        }
    }

    return {
        loading,
        betAmount,
        result,
        spin,
    }
}
```

### Blade Templates

```blade
{{-- resources/views/games/show.blade.php --}}
@extends('layouts.app')

@section('content')
<div class="game-container">
    <h1>{{ $game->name }}</h1>
    
    <div class="game-info">
        <p>RTP: {{ $game->rtp }}%</p>
        <p>Provedor: {{ $game->provider }}</p>
    </div>

    <div id="game-instance" data-game-id="{{ $game->id }}">
        {{-- Game iframe ou WebGL aqui --}}
    </div>

    @auth
        <form action="{{ route('games.spin', $game) }}" method="POST" @submit="onSpin">
            @csrf
            <input type="number" name="amount" min="1" value="100" required>
            <button type="submit" :disabled="loading">
                <span v-if="!loading">Girar</span>
                <span v-else>Girando...</span>
            </button>
        </form>
    @else
        <p><a href="{{ route('login') }}">Faça login para jogar</a></p>
    @endauth
</div>
@endsection
```

---

## 🗄️ Criando Migrações

### Estrutura

```bash
php artisan make:migration create_my_table
```

```php
// database/migrations/2024_07_22_000000_create_my_table.php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('my_table', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->unsignedBigInteger('user_id');
            $table->decimal('amount', 10, 2);
            $table->timestamps();

            $table->foreign('user_id')
                ->references('id')
                ->on('users')
                ->onDelete('cascade');

            $table->index('user_id');
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('my_table');
    }
};
```

### Executar

```bash
# Rodar todas
php artisan migrate

# Rodar com seeding
php artisan migrate:fresh --seed

# Reverter última
php artisan migrate:rollback

# Reverter tudo
php artisan migrate:refresh

# Status
php artisan migrate:status
```

---

## 🔌 Criando API Endpoints

### Estrutura

```php
// routes/api.php
use App\Http\Controllers\Api\GameController;

Route::middleware('auth:sanctum')->group(function () {
    Route::post('/games/{game}/spin', [GameController::class, 'spin']);
    Route::get('/games', [GameController::class, 'index']);
});
```

### Controller API

```php
namespace App\Http\Controllers\Api;

use App\Http\Resources\GameResultResource;
use App\Services\Games\GameEngine;
use Illuminate\Http\JsonResponse;

class GameController extends Controller
{
    public function spin($gameId): JsonResponse
    {
        $game = Game::findOrFail($gameId);
        
        $validated = request()->validate([
            'amount' => 'required|numeric|min:1|max:10000',
        ]);

        $engine = new GameEngine($game, $validated['amount']);
        $result = $engine->spin();

        return response()->json(
            new GameResultResource($result['result']),
            201
        );
    }
}
```

---

## 🔐 Autenticação e Autorização

### Gates e Policies

```php
// app/Providers/AuthServiceProvider.php
public function boot(): void
{
    Gate::define('edit-game', function (User $user, Game $game) {
        return $user->isAdmin();
    });
}
```

```php
// app/Policies/GamePolicy.php
public function update(User $user, Game $game): bool
{
    return $user->isAdmin();
}
```

### Usar em Views

```blade
@can('edit-game', $game)
    <a href="{{ route('games.edit', $game) }}">Editar</a>
@endcan
```

### Usar em Controllers

```php
if (auth()->user()->cannot('edit-game', $game)) {
    abort(403);
}
```

---

## 🧪 Testando

### Criar Tests

```bash
php artisan make:test GameEngineTest --unit
```

```php
// tests/Unit/GameEngineTest.php
use PHPUnit\Framework\TestCase;
use App\Services\Games\GameEngine;

class GameEngineTest extends TestCase
{
    public function test_spin_calculates_payout()
    {
        $game = Game::factory()->create(['rtp' => 95]);
        $engine = new GameEngine($game, 100);
        $result = $engine->spin();

        $this->assertIsArray($result);
        $this->assertArrayHasKey('payout', $result);
    }
}
```

### Rodar Tests

```bash
# Todos
php artisan test

# Com coverage
php artisan test --coverage

# Específico
php artisan test tests/Unit/GameEngineTest.php
```

---

## 🐛 Debug

### Usando dd() e dump()

```php
// Parar execução e mostrar
dd($data);

// Mostrar e continuar
dump($data);

// No Blade
{{ dd($variable) }}
```

### Usando Laravel Debugbar

```bash
composer require barryvdh/laravel-debugbar --dev
```

### Logs

```php
use Illuminate\Support\Facades\Log;

Log::debug('Debug message', ['context' => 'data']);
Log::info('Info message');
Log::warning('Warning message');
Log::error('Error message');

// Ver logs
tail -f storage/logs/laravel.log
```

---

## 📤 Deploy

### Compartir Código via Git

```bash
# Status
git status

# Add changes
git add .

# Commit
git commit -m "feat: add new game feature"

# Push para sua branch
git push origin v0/farleymortimerhelga9-6030-d9d0b895

# Abrir PR no GitHub
```

### Build para Production

```bash
# Compilar assets
npm run build

# Minify código
php artisan optimize

# Cache config
php artisan config:cache

# Precompile routes
php artisan route:cache
```

---

## 📚 Recursos Úteis

| Recurso | Link |
|---------|------|
| Laravel Docs | https://laravel.com/docs |
| Eloquent ORM | https://laravel.com/docs/eloquent |
| Vue.js | https://vuejs.org/ |
| Tailwind CSS | https://tailwindcss.com/ |
| Composer | https://getcomposer.org/ |
| NPM | https://www.npmjs.com/ |

---

## ❓ Dúvidas Comuns

**P: Como adicionar nova coluna à tabela?**
R: Crie uma migration: `php artisan make:migration add_column_to_table`

**P: Como resetar o banco de dados?**
R: `php artisan migrate:fresh --seed`

**P: Onde coloco lógica de negócio?**
R: Em `app/Services/`, não em Controllers.

**P: Como criar relacionamento entre models?**
R: Use `hasMany()`, `belongsTo()`, `belongsToMany()` nos Models.

---

**Bom código!** 🚀
