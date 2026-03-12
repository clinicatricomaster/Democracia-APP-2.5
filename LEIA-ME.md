# ⚽ TURMA DEMOCRACIA — GUIA DE CONFIGURAÇÃO FIREBASE

## PASSO 1 — Criar projeto no Firebase

1. Acesse: https://console.firebase.google.com
2. Clique em **"Adicionar projeto"**
3. Nome: `turma-democracia` (ou o que quiser)
4. Desative o Google Analytics (não é necessário) → **Criar projeto**

---

## PASSO 2 — Ativar Authentication

1. No menu lateral: **Build → Authentication**
2. Clique em **"Começar"**
3. Na aba **"Sign-in method"**, habilite **"E-mail/senha"**
4. Vá em **"Users"** → **"Adicionar usuário"**
5. Cadastre seu e-mail e senha de admin

---

## PASSO 3 — Criar Firestore Database

1. No menu lateral: **Build → Firestore Database**
2. Clique em **"Criar banco de dados"**
3. Escolha **"Iniciar no modo de teste"** → Avançar
4. Selecione a região `southamerica-east1` (São Paulo) → **Ativar**

### Regras de segurança do Firestore
Após criar, vá em **"Regras"** e cole o seguinte:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Qualquer um pode LER (jogadores e visitantes)
    match /jogadores/{id} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    
    match /rodadas/{id} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

Clique em **"Publicar"**.

---

## PASSO 4 — Registrar o app Web

1. Na página inicial do projeto, clique no ícone **"</>"** (Web)
2. Nome do app: `democracia-web`
3. NÃO marque "Firebase Hosting" (usaremos GitHub Pages)
4. Clique em **"Registrar app"**
5. Copie os valores do `firebaseConfig` que aparecer

---

## PASSO 5 — Configurar o index.html

Abra o arquivo `index.html` e substitua na seção `FIREBASE_CONFIG`:

```js
const FIREBASE_CONFIG = {
  apiKey:            "AIzaSy...",        // ← cole o seu
  authDomain:        "turma-democracia.firebaseapp.com",
  projectId:         "turma-democracia",
  storageBucket:     "turma-democracia.appspot.com",
  messagingSenderId: "123456789",
  appId:             "1:123456789:web:abc..."
};

// Adicione seu e-mail de admin:
const ADMIN_EMAILS = [
  "seu-email@gmail.com"
];
```

---

## PASSO 6 — Publicar no GitHub Pages

1. Crie conta em https://github.com (se não tiver)
2. Crie repositório público chamado `democracia`
3. Faça upload dos 5 arquivos:
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - `icon-192.png`
   - `icon-512.png`
4. Vá em **Settings → Pages → Branch: main → Save**
5. Acesse: `https://SEU-USUARIO.github.io/democracia`

---

## PASSO 7 — Instalar no celular

### Android (Chrome):
- Abra o link no Chrome → aparecerá banner "Instalar app"
- Toque em **INSTALAR** → ícone aparece na tela inicial

### iPhone (Safari):
- Abra o link no Safari
- Toque no botão **Compartilhar** (quadrado com seta)
- Role para baixo → **"Adicionar à Tela de Início"**
- Toque em **Adicionar**

---

## COMO FUNCIONA

| Quem acessa | O que pode fazer |
|---|---|
| Qualquer pessoa com o link | Ver dashboard, classificação, artilharia, goleiros, resultados |
| Admin (e-mail/senha) | Tudo acima + adicionar/editar/excluir rodadas e jogadores |

### Dados em tempo real
Os dados são atualizados **instantaneamente** para todos que estiverem
com o app aberto — sem precisar recarregar a página.

---

## ADICIONAR MAIS ADMINS

No `index.html`, adicione os e-mails na lista:
```js
const ADMIN_EMAILS = [
  "admin1@gmail.com",
  "admin2@gmail.com"
];
```
E crie os usuários no Firebase Authentication → Users.

---

## SUPORTE
Dúvidas? Firebase Docs: https://firebase.google.com/docs
