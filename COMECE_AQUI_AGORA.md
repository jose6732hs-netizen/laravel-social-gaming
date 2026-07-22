# 🚀 COMECE AQUI AGORA - 5 Passos Simples

**Não é desenvolvedor? Sem problema! Siga estes 5 passos e estará rodando o projeto em 30 minutos.**

---

## 📍 PASSO 1: INSTALAR 4 PROGRAMAS (10 min)

Você precisa de 4 programas. Escolha seu SO:

### ✅ Passo 1A: PHP

**Windows:**
1. Vá para: https://www.php.net/downloads
2. Clique em "Windows: Binaries"
3. Baixe "Non Thread Safe" (versão recente)
4. Descompacte em `C:\php`

**macOS:**
```bash
brew install php
```

**Linux (Ubuntu):**
```bash
sudo apt install php php-cli php-curl php-mbstring php-xml php-zip
```

Verifique:
```bash
php -v
```

✅ Se mostrou versão, funcionou!

---

### ✅ Passo 1B: Composer

**Windows:**
1. Vá para: https://getcomposer.org/download
2. Clique em "Composer-Setup.exe"
3. Execute e siga os passos

**macOS:**
```bash
brew install composer
```

**Linux:**
```bash
sudo apt install composer
```

Verifique:
```bash
composer -V
```

✅ Se mostrou versão, funcionou!

---

### ✅ Passo 1C: Node.js + npm

**Todos os SOs:**
1. Vá para: https://nodejs.org/
2. Clique no botão verde "LTS"
3. Baixe e instale

Verifique:
```bash
node -v
npm -v
```

✅ Se mostrou versão 2 vezes, funcionou!

---

### ✅ Passo 1D: MySQL

**Windows:**
1. Vá para: https://www.mysql.com/downloads/
2. Procure "MySQL Community Server"
3. Clique "Download"
4. Clique "No thanks, just start my download"
5. Instale com:
   - Porta: 3306
   - Usuário: root
   - Senha: password

**macOS:**
```bash
brew install mysql
```

**Linux:**
```bash
sudo apt install mysql-server
```

Verifique:
```bash
mysql -u root -p
```
(Digite a senha que criou)

✅ Se conectou, funcionou!

---

## 📁 PASSO 2: PREPARAR O PROJETO (5 min)

### ✅ Abra um Terminal/Cmd

- **Windows**: Tecla Windows + R → digit `cmd` → Enter
- **macOS**: Cmd + Espaço → digit `terminal` → Enter  
- **Linux**: Ctrl + Alt + T

### ✅ Vá até a Pasta do Projeto

```bash
cd caminho/para/laravel-social-gaming
```

Substitua `caminho/para/` pelo caminho real! Exemplo:

**Windows:**
```cmd
cd C:\Users\SeuNome\Documentos\laravel-social-gaming
```

**macOS:**
```bash
cd ~/Documentos/laravel-social-gaming
```

Verifique que você está no lugar certo:
```bash
ls
```

Você deve ver pastas: `casino/`, `resources/`, `public/`, etc.

### ✅ Entre na Pasta casino

```bash
cd casino
```

### ✅ Copie o arquivo de configuração

**Windows:**
```cmd
copy .env.example .env
```

**macOS/Linux:**
```bash
cp .env.example .env
```

### ✅ Edite o arquivo .env

Abra `.env` com um editor de texto (Bloco de Notas, VS Code, etc)

Procure por:
```
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

Altere para:
```
DB_DATABASE=laravel_social_gaming
DB_USERNAME=root
DB_PASSWORD=password
```

**Salve o arquivo!**

### ✅ Gere a Chave da Aplicação

No terminal:
```bash
php artisan key:generate
```

Você deve ver: `Application key set successfully.`

---

## ⚙️ PASSO 3: INSTALAR DEPENDÊNCIAS (15 min)

### ✅ Instalar PHP Packages

Ainda na pasta `casino/`:
```bash
composer install
```

⏳ Vai demorar 3-5 minutos. Deixe rodar!

Quando terminar, você vê: `Generated autoload files`

### ✅ Criar Banco de Dados

```bash
php artisan migrate
```

Você deve ver linhas que começam com `Migrating:`

### ✅ Instalar JavaScript Packages

Volte para a raiz do projeto:
```bash
cd ..
```

Agora instale JavaScript:
```bash
npm install
npm run build
```

⏳ Vai demorar 2-3 minutos. Deixe rodar!

---

## 🎮 PASSO 4: RODAR O PROJETO (2 min)

### ⚠️ IMPORTANTE: ABRA 2 TERMINAIS

Você vai precisar de 2 terminais abertos ao mesmo tempo!

---

### ✅ Terminal 1: Abra o Backend

No primeiro terminal:
```bash
cd casino
php artisan serve
```

**Você deve ver:**
```
INFO  Server running on [http://127.0.0.1:8000]
```

**NÃO feche este terminal!** Deixe rodando!

---

### ✅ Terminal 2: Abra o Frontend

Abra um NOVO terminal (não feche o primeiro!)

```bash
cd laravel-social-gaming
npm run dev
```

**Você deve ver:**
```
VITE vX.X.X ready in XXX ms
```

**NÃO feche este terminal!** Deixe rodando!

---

## 🌐 PASSO 5: ABRIR NO NAVEGADOR (1 min)

Abra seu navegador (Chrome, Firefox, Edge, Safari):

1. Clique na barra de endereço
2. Digite: `http://localhost:8000`
3. Aperte Enter

### ✅ Você deve ver:

- Página de login/registro
- Design bonito
- Sem erros na tela

**Se isso acontecer, FUNCIONOU! 🎉**

---

## 📝 Criar uma Conta de Teste

1. Clique em "Register" (ou "Registrar")
2. Preencha:
   - Email: `teste@example.com`
   - Senha: `password123`
3. Clique "Register"
4. Faça login com essas credenciais

---

## 🆘 Se Não Funcionou

### ❌ Erro: "Port 8000 already in use"

```bash
php artisan serve --port=8001
```

Depois acesse: `http://localhost:8001`

---

### ❌ Erro: "command not found" ou "não reconhecido"

- **PHP não funciona**: Instale novamente
- **Composer não funciona**: Instale novamente
- **npm não funciona**: Instale Node.js novamente

---

### ❌ Erro: "Connection refused"

1. Verifique se Terminal 1 está rodando
2. Verifique se Terminal 2 está rodando
3. Se não, rode novamente

---

### ❌ Página em branco

1. Pressione F12 (abrir Console)
2. Procure erros em vermelho
3. Copie e compartilhe o erro

---

## ✅ Checklist de Sucesso

- [ ] Terminal 1 mostra "Server running on http://127.0.0.1:8000"
- [ ] Terminal 2 mostra "VITE ... ready in"
- [ ] Navegador abre http://localhost:8000
- [ ] Consegue fazer login
- [ ] Não há erros na página

---

## 📚 Depois de Rodar

Agora que o projeto está funcionando:

1. **Explore**: Clique em botões, veja como funciona
2. **Leia**: `QUICK_REFERENCE.md` (comandos úteis)
3. **Aprenda**: `ARCHITECTURE.md` (como funciona)
4. **Desenvolva**: `DEV_GUIDE.md` (criar features novas)

---

## 🎓 Próximos Passos (Opcionais)

Se quiser criar coisas novas, leia:

1. `DEV_GUIDE.md` - Como criar controllers, modelos, etc
2. `QUICK_REFERENCE.md` - Comandos prontos para copiar

---

## 💪 Você Consegue!

Seguindo estes 5 passos, você terá o projeto rodando em **30 minutos máximo**!

Se tiver dúvida em qualquer passo, compartilhe:
- Qual é seu Sistema Operacional (Windows/Mac/Linux)
- Em qual passo ficou travado
- A mensagem de erro exata (copie o texto em vermelho)

Vamos resolver juntos! 🚀

---

**Bom desenvolvimento!** 🎉
