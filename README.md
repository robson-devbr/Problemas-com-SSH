# 🛠 Guia Rápido: Problemas com SSH no GitHub

Este guia tem como objetivo ajudar você a resolver os principais erros de autenticação com o GitHub via SSH ou HTTPS.

---

## 1. 🔐 Chave SSH mal configurada

### ✅ Solução:
```bash
ssh -T git@github.com
````

Se a resposta for:

```
Hi usuario! You've successfully authenticated...
```

Está tudo certo.

Se der erro, você precisa configurar ou carregar a chave SSH.

---

## 2. 🚫 Firewall, Proxy ou VPN bloqueando a porta 22 (SSH padrão)

### ✅ Solução:

Use a porta alternativa 443 (caso a porta 22 esteja bloqueada):

```bash
git clone ssh://git@ssh.github.com:443/usuario/repositorio.git
```

---

## 3. 🧩 Testar conexão SSH e descobrir onde está travando

```bash
ssh -vT git@github.com
```

Se **não aparecer** a palavra `Authenticated`, ou travar em:

```
Connecting to github.com port 22
```

→ Provavelmente a porta 22 está bloqueada, ou sua chave não está sendo usada.

---

## 4. ⚠️ Erro: "Host key verification failed"

Esse erro aparece quando você tenta usar **SSH pela primeira vez** e não confirma que confia no servidor.

### 💬 Mensagem exibida:

```
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

### ✅ Solução:

1. Quando aparecer essa mensagem, digite exatamente:

```bash
yes
```

2. O Git vai registrar o fingerprint do GitHub em `~/.ssh/known_hosts` e **nunca mais perguntará**.

3. Se você respondeu errado da primeira vez, o erro aparecerá como:

```
Host key verification failed.
```

👉 Nesse caso, rode novamente o comando de clone:

```bash
git clone git@github.com:robson-devbr/api-test-02.git
```

E digite `yes` quando solicitado.

---

## 5. 🔁 Trocar o remoto de HTTPS para SSH

### Verifique o remoto atual:

```bash
git remote -v
```

**Exemplo de saída:**

```
origin  https://github.com/robson-devbr/api-test-02.git (fetch)
origin  https://github.com/robson-devbr/api-test-02.git (push)
```

### Troque a URL para SSH:

```bash
git remote set-url origin git@github.com:robson-devbr/api-test-02.git
```

### Confirme:

```bash
git remote -v
```

**Nova saída esperada:**

```
origin  git@github.com:robson-devbr/api-test-02.git (fetch)
origin  git@github.com:robson-devbr/api-test-02.git (push)
```

---

## 6. ✅ Usar SSH corretamente (mais prático a longo prazo)

### 🔹 Gerar uma nova chave SSH:

```bash
ssh-keygen -t ed25519 -C "seu-email@gmail.com"
```

### 🔹 Copiar a chave pública:

```bash
cat ~/.ssh/id_ed25519.pub
```

### 🔹 Adicionar no GitHub:

* Acesse seu perfil no GitHub
* Vá em: **Settings → SSH and GPG keys → New SSH Key**
* Cole a chave
* Dê um nome como: `Notebook Robson`

### 🔹 Clonar com SSH:

```bash
git clone git@github.com:robson-devbr/api-test-02.git
```

---

## 🚧 Por que o erro acontece?

Desde 2021, o GitHub **não permite mais autenticação via senha no HTTPS**. Você precisa usar **Token de Acesso Pessoal (PAT)** ou **chave SSH**.

---

## ✅ Opção 1: Clonar via HTTPS com Token

1. Acesse: [github.com/settings/tokens](https://github.com/settings/tokens)
2. Clique em **"Generate new token" → "Fine-grained token"**
3. Selecione:

   * `Repo:` acesso total (read & write)
   * Escolha o repositório ou **All repositories**
   * Validade: 30 ou 90 dias
4. Copie o token gerado (ex: `ghp_suaChaveAqui`)

### Clonar com o token:

```bash
git clone https://github.com/robson-devbr/api-test-02.git
```

Quando pedir:

```
Username: robson-devbr
Password: cole o token (não sua senha de login)
```

---

## ✅ Opção 2: Usar SSH (recomendado)

### Gere a chave:

```bash
ssh-keygen -t ed25519 -C "seu-email@gmail.com"
```

### Copie:

```bash
cat ~/.ssh/id_ed25519.pub
```

### No GitHub:

* Vá em **Settings → SSH and GPG keys → New SSH Key**
* Cole a chave
* Dê um nome como: `Notebook Robson`

### Clonar com SSH:

```bash
git clone git@github.com:robson-devbr/api-test-02.git
```

---
Bons commits! 🚀


