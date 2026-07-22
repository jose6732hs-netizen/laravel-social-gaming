# 🎓 Guia Completo para Iniciantes - Projeto Laravel Social Gaming

**⏱️ Tempo total: ~30-45 minutos**

> Você NÃO precisa ser desenvolvedor! Este guia é passo-a-passo e super detalhado.

---

## 📋 Índice Rápido

1. [O que você vai fazer](#o-que-voc%C3%AA-vai-fazer)
2. [Pré-requisitos (Instalações)](#pr%C3%A9-requisitos)
3. [Preparar o Computador](#preparar-o-computador)
4. [Rodar o Projeto](#rodar-o-projeto)
5. [Troubleshooting](#troubleshooting)

---

## 🎯 O que você vai fazer

Ao final deste guia você vai:

✅ Ter **PHP, Composer, Node.js e MySQL** instalados  
✅ Ter o projeto **configurado e pronto**  
✅ Conseguir **abrir no navegador** em `http://localhost:8000`  
✅ Ver o projeto **rodando localmente**  

---

## 🖥️ Pré-requisitos (O que Instalar)

Você precisa instalar 4 programas. Escolha seu sistema operacional:

### 🪟 Se você usa **Windows**

#### 1️⃣ Instalar PHP

1. Acesse: https://www.php.net/downloads
2. Clique em "**Windows: Binaries**"
3. Procure por "**Non Thread Safe**" (versão recente, ex: 8.4)
4. Clique em "**Zip**" para baixar

```
Exemplo: php-8.4.0-nts-Win32-x64.zip
```

5. **Descompacte** em `C:\php`
6. Abra Cmd (tecla Windows + R, digite `cmd`)

```cmd
C:\php\php -v
```

Se aparecer versão, funcionou! ✅

---

#### 2️⃣ Instalar Composer

1. Acesse: https://getcomposer.org/download
2. Clique em "**Composer-Setup.exe**"
3. Execute e siga todos os passos (Next > Next > Install)
4. Na pergunta "Do you want to use the PHP executable...", clique em **Browse**
5. Procure por `C:\php\php.exe`
6. Termine a instalação

Abra Cmd nova janela:

```cmd
composer -V
```

Se aparecer versão, funcionou! ✅

---

#### 3️⃣ Instalar Node.js

1. Acesse: https://nodejs.org/
2. Clique no botão verde grande "**LTS**"
3. Execute o `.msi` que baixou
4. Siga todos os passos (Next > Next > Install)

Abra Cmd nova janela:

```cmd
node -v
npm -v
```

Se aparecer versão, funcionou! ✅

---

#### 4️⃣ Instalar MySQL

1. Acesse: https://www.mysql.com/downloads/
2. Procure por "**MySQL Community Server**"
3. Clique em "Download"
4. Clique em "**No thanks, just start my download**"
5. Execute o `.msi` que baixou
6. **IMPORTANTE**: Na instalação, escolha estas opções:
   - Porta: `3306` (padrão)
   - Usuário: `root`
   - Senha: `password` (simples, só para desenvolvimento)

Depois, verifique:

```cmd
mysql -u root -p
```

Vai pedir senha, digite: `password`

Se conectar, funcionou! ✅ (saia com `exit`)

---

### 🍎 Se você usa **macOS**

#### 1️⃣ Instalar Homebrew (gerenciador de pacotes)

Abra o Terminal e execute:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Depois:

```bash
brew --version
```

---

#### 2️⃣ Instalar PHP, Composer, Node.js e MySQL

```bash
# PHP
brew install php

# Composer
brew install composer

# Node.js
brew install node

# MySQL
brew install mysql
```

Após instalar, verifique:

```bash
php -v
composer -V
node -v
npm -v
mysql -V
```

---

### 🐧 Se você usa **Linux** (Ubuntu/Debian)

```bash
# Atualizar repositórios
sudo apt update && sudo apt upgrade -y

# Instalar tudo de uma vez
sudo apt install -y php php-cli php-curl php-mbstring php-xml php-zip composer nodejs npm mysql-server

# Verificar instalações
php -v
composer -V
node -v
npm -v
mysql -V
```

---

## 📁 Preparar o Computador

Agora que você tem tudo instalado, vamos preparar o projeto.

### Passo 1: Abrir Terminal

- **Windows**: Tecla Windows + R, digite `cmd`, aperte Enter
- **macOS**: Cmd + Espaço, digite `terminal`, aperte Enter
- **Linux**: Ctrl + Alt + T

### Passo 2: Ir até o Projeto

No terminal, você precisa navegar até onde o projeto está.

Se o projeto está em `C:\Users\SeuNome\Documentos\laravel-social-gaming`:

```bash
# Windows
cd C:\Users\SeuNome\Documentos\laravel-social-gaming

# macOS/Linux
cd ~/Documentos/laravel-social-gaming
```

**Substitua `SeuNome` pelo seu nome de usuário real!**

Teste:

```bash
dir
```

(Windows) ou

```bash
ls
```

(macOS/Linux)

Você deve ver pastas como `casino/`, `resources/`, etc.

---

### Passo 3: Copiar Arquivo de Configuração

```bash
cd casino
```

Agora copie o arquivo de exemplo:

**Windows:**
```cmd
copy .env.example .env
```

**macOS/Linux:**
```bash
cp .env.example .env
```

---

### Passo 4: Editar Arquivo .env

Abra o arquivo `.env` com um editor de texto:

**Windows:**
- Clique direito no arquivo `.env`
- Selecione "Abrir com"
- Escolha "Bloco de Notas"

**macOS/Linux:**
```bash
nano .env
```

Procure pela seção de banco de dados e altere:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_social_gaming
DB_USERNAME=root
DB_PASSWORD=password
```

**Salve o arquivo!**

---

### Passo 5: Gerar Chave da Aplicação

No terminal (pasta `casino/`):

```bash
php artisan key:generate
```

Deve aparecer:

```
Application key set successfully.
```

---

### Passo 6: Instalar Dependências PHP

Ainda na pasta `casino/`:

```bash
composer install
```

⏳ Vai demorar 3-5 minutos. Deixe rodar!

---

### Passo 7: Criar Banco de Dados

Continuando no terminal:

```bash
php artisan migrate
```

Deve aparecer algo como:

```
Migrating: ...
```

Isso cria as tabelas no banco de dados.

---

### Passo 8: Instalar Dependências JavaScript

Na pasta raiz do projeto (voltar um nível):

```bash
cd ..
```

Agora:

```bash
npm install
npm run build
```

⏳ Vai demorar 2-3 minutos. Deixe rodar!

---

## 🚀 Rodar o Projeto

**FINALMENTE! Agora vamos rodar o projeto!**

### Abra 2 TERMINAIS (muito importante!)

**Terminal 1: Backend (Laravel)**

```bash
cd casino
php artisan serve
```

Você deve ver:

```
INFO  Server running on [http://127.0.0.1:8000]
```

**NÃO feche este terminal!**

---

**Terminal 2: Frontend (Assets)**

Abra um novo terminal e execute:

```bash
cd laravel-social-gaming
npm run dev
```

Você deve ver algo como:

```
VITE v... ready in 123 ms
```

**NÃO feche este terminal!**

---

## 🌐 Abrir no Navegador

Agora abra seu navegador favorito (Chrome, Firefox, Edge, etc):

1. Clique na barra de endereço
2. Digite: `http://localhost:8000`
3. Aperte Enter

Se tudo funcionou, você vai ver a página de login/registro do projeto! 🎉

---

## 📝 Criar Conta de Teste

1. Clique em "**Register**" (ou similar)
2. Preencha:
   - Email: `teste@example.com`
   - Senha: `password123`
3. Clique em "Register"
4. Faça login

---

## 🛠️ Troubleshooting (Problemas Comuns)

### ❌ Erro: "Port 8000 already in use"

**Problema:** Outra aplicação está usando a porta 8000.

**Solução:**

Terminal 1, use outra porta:

```bash
php artisan serve --port=8001
```

Depois acesse: `http://localhost:8001`

---

### ❌ Erro: "Connection refused" ou "Cannot connect"

**Problema:** Backend não está rodando.

**Solução:**
1. Verifique se o Terminal 1 tem `Server running on...`
2. Se não, execute novamente:
   ```bash
   cd casino
   php artisan serve
   ```

---

### ❌ Erro: "SQLSTATE[HY000]"

**Problema:** Banco de dados não conecta.

**Solução:**
1. Verifique se MySQL está rodando
2. Verifique credenciais em `.env` (usuário, senha, host)
3. Recrie o banco:
   ```bash
   php artisan migrate:refresh
   ```

---

### ❌ Erro: "Command not found: composer"

**Problema:** Composer não está no PATH.

**Solução:**
- **Windows**: Instale Composer novamente
- **macOS/Linux**: Use `brew install composer` ou `sudo apt install composer`

---

### ❌ Erro: "npm: command not found"

**Problema:** Node.js/npm não está instalado.

**Solução:**
1. Acesse https://nodejs.org/
2. Baixe a versão LTS
3. Instale
4. Abra terminal nova e tente novamente

---

### ❌ Página em branco ou com erros

**Solução:**

1. Abra o Console do Navegador (F12)
2. Procure por erros em vermelho
3. Copie o erro
4. Se precisar de ajuda, compartilhe o erro

---

### ❌ "php.exe is not recognized"

**Problema:** PHP não está no PATH (só Windows).

**Solução:**
1. Desinstale PHP
2. Instale novamente
3. Na instalação, escolha "Add to PATH"

---

## ✅ Checklist Final

Antes de começar a desenvolver:

- [ ] Abra `http://localhost:8000` e vê o projeto
- [ ] Consegue fazer login
- [ ] Consegue navegar entre páginas
- [ ] Console do navegador (F12) não tem erros vermelhos
- [ ] Terminal 1 está rodando (não fechou)
- [ ] Terminal 2 está rodando (não fechou)

---

## 📚 Próximas Leituras

Agora que o projeto está rodando:

1. **Leia**: `00_START_HERE.md` (resumo rápido)
2. **Explore**: `QUICK_REFERENCE.md` (comandos úteis)
3. **Entenda**: `ARCHITECTURE.md` (como o projeto funciona)
4. **Desenvolva**: `DEV_GUIDE.md` (como criar coisas novas)

---

## 💡 Dicas de Ouro

1. **Deixe os 2 terminais abertos** - Fechar = projeto para
2. **Ctrl+C** no terminal = para o servidor (use para parar)
3. **Não delete a pasta `node_modules`** - Só se souber o que está fazendo
4. **Use VS Code** - Melhor editor para trabalhar com este projeto
5. **Se tiver dúvida, googla!** - Erro + "laravel" + "npm" ajuda muito

---

## 🆘 Ainda com dúvida?

1. Compartilhe o erro exato (copie de vermelho do terminal)
2. Diga qual é o seu Sistema Operacional (Windows/Mac/Linux)
3. Diga em qual passo está travado

Vou ajudar! 💪

---

**Boa sorte e feliz desenvolvimento!** 🚀
