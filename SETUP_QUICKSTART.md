# 🚀 Quick Start - Laravel Social Gaming Engine (Lite 13)

## ⏱️ Time Required: 10-15 minutes

Este guia assume que você tem **PHP 8.4+**, **Composer**, **Node.js**, e **MySQL 8.0+** instalados.

---

## 📋 Pré-requisitos

Verifique se tem tudo instalado:

```bash
# Verificar PHP
php -v
# Deve retornar: PHP 8.4.x ou superior

# Verificar Composer
composer -V
# Deve retornar: Composer version 2.x.x

# Verificar Node.js
node -v
npm -v
# Deve retornar: v16.x.x ou superior

# Verificar MySQL
mysql -V
# Deve retornar: mysql Ver 8.0.x
```

**Não tem tudo?** Veja a seção [Instalando Dependências](#instalando-dependências) no final.

---

## 🎯 Passo 1: Clonar e Acessar o Repositório

```bash
# Clone (você já deve ter feito isso)
git clone -b lite-13 https://github.com/gamingdotme/laravel-social-gaming.git
cd laravel-social-gaming

# Acessar a pasta do Laravel
cd casino
```

---

## 🔧 Passo 2: Copiar e Configurar .env

```bash
# Copiar template de ambiente
cp .env.example .env

# Gerar chave de aplicação
php artisan key:generate
```

Agora edite o arquivo `.env` com suas credenciais:

```env
# Banco de dados (altere com suas credenciais)
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_social_gaming
DB_USERNAME=root
DB_PASSWORD=sua_senha

# App URL (IMPORTANTE!)
APP_URL=http://localhost:8000

# Timezone (opcional)
APP_TIMEZONE=America/Sao_Paulo
```

### 📌 Importante: Criar Banco de Dados

Se ainda não existe, crie manualmente:

```bash
mysql -u root -p -e "CREATE DATABASE laravel_social_gaming CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
```

---

## 📦 Passo 3: Instalar Dependências

```bash
# Instalar dependências PHP
composer install

# Sair da pasta casino (volta para raiz)
cd ..

# Instalar dependências Node.js (IMPORTANTE: fazer na raiz!)
npm install && npm run build
```

> **Volta para casino antes de continuar!**
> ```bash
> cd casino
> ```

---

## 🗄️ Passo 4: Rodar Migrações

```bash
# Criar tabelas no banco de dados
php artisan migrate

# Opcional: Popular com dados de teste
# php artisan db:seed
```

---

## ✨ Passo 5: Compilar Assets

```bash
# Volte para a pasta casino se não estiver lá
cd casino

# Compilar assets
npm run dev
# ou para modo production: npm run build
```

---

## 🎮 Passo 6: Iniciar o Servidor

**Em um terminal (mantenha aberto):**

```bash
php artisan serve
```

Você verá:
```
INFO  Server running on [http://127.0.0.1:8000].
```

---

## 🌐 Passo 7: Acessar a Aplicação

Abra seu navegador e acesse:

```
http://localhost:8000
```

**Parabéns!** 🎉 O projeto está rodando!

---

## 👤 Login (Teste)

As credenciais padrão podem ser:

- **Email:** `admin@example.com` ou `user@example.com`
- **Senha:** `password`

Se não funcionar, você pode:

1. Rodar seeds:
   ```bash
   php artisan db:seed
   ```

2. Ou criar um usuário manualmente:
   ```bash
   php artisan tinker
   >>> User::create(['name' => 'Admin', 'email' => 'admin@test.com', 'password' => bcrypt('password')])
   ```

---

## 🛑 Parando o Servidor

No terminal onde rodou `php artisan serve`, pressione:

```
CTRL + C
```

---

## 📁 Estrutura de Pastas Importante

```
laravel-social-gaming/
├── casino/                    # 👈 App Laravel principal
│   ├── app/                   # Código da aplicação
│   ├── resources/             # Views e assets
│   ├── routes/                # Rotas da aplicação
│   ├── database/              # Migrations e seeds
│   ├── public/                # Arquivos públicos
│   ├── .env                   # Configuração (NÃO commitar!)
│   └── composer.json          # Dependências PHP
├── frontend/                  # Código frontend customizado
├── minimal/                   # Tema minimal alternativo
├── socket_config.json         # Configuração do socket
└── package.json               # Dependências Node.js (raiz)
```

---

## 🐛 Troubleshooting

### Erro: "Class 'PDO' not found"

**Solução:** Instale extensão PDO do PHP:

```bash
# macOS
brew install php@8.4

# Ubuntu/Debian
sudo apt-get install php8.4-mysql

# Windows
Descomente a linha `extension=pdo_mysql` no `php.ini`
```

### Erro: "Connection refused" (banco de dados)

**Solução:** Verifique se MySQL está rodando:

```bash
# macOS
brew services start mysql

# Ubuntu/Debian
sudo systemctl start mysql

# Windows
Abra Services e procure por MySQL
```

### Erro: npm not found

**Solução:** Instale Node.js:

```bash
# macOS
brew install node

# Ubuntu/Debian
sudo apt-get install nodejs npm

# Windows
Baixe em https://nodejs.org/
```

### Assets não carregam / Erro 404 no /games/

Este projeto usa CDN para assets. Se quiser usar localmente:

1. Edite `casino/.env`:
   ```env
   GAMES_CDN_MODE=local
   ```

2. Ou Configure `.htaccess` para sua própria CDN.

---

## ✅ Checklist de Conclusão

- [ ] PHP 8.4+ instalado
- [ ] Composer instalado
- [ ] Node.js instalado
- [ ] MySQL rodando
- [ ] `.env` configurado com suas credenciais
- [ ] `php artisan key:generate` executado
- [ ] `composer install` completado
- [ ] `npm install && npm run build` completado
- [ ] `php artisan migrate` executado
- [ ] `php artisan serve` rodando
- [ ] Conseguir acessar http://localhost:8000

Se todos estão checked ✅, **você está pronto!**

---

## 🔗 Próximos Passos

1. **Explorar o Admin:**
   - Acesse a dashboard
   - Gerencie jogos, sportsbook, pagamentos

2. **Ler a Documentação:**
   - Veja `README.md` para features completas
   - Veja `DEVELOPERS.md` para padrões de código

3. **Integrar APIs (Opcional):**
   - The Odds API (para Sportsbook)
   - XtoPay (para Crypto)
   - Stripe ou PayPal (para Pagamentos)

---

## 📞 Suporte

- **Discord:** https://discord.gg/nYHGyQ5q
- **GitHub:** https://github.com/gamingdotme/laravel-social-gaming
- **Sponsor:** https://github.com/sponsors/promexdotme

---

## 📄 Licença

MIT License - Veja LICENSE file

---

**Bom desenvolvimento!** 🚀
