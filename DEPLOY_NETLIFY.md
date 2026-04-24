# Deploy no Netlify

Este repositório está configurado para publicar o app Vue (pasta `frontend/`) no Netlify usando o arquivo `netlify.toml` na raiz.

## 1) Suba este código para o GitHub

```bash
git add .
git commit -m "chore: preparar deploy no Netlify"
git push
```

## 2) Crie o site no Netlify

1. Acesse: https://app.netlify.com/
2. Clique em **Add new site** > **Import an existing project**.
3. Conecte com GitHub e selecione este repositório.
4. O Netlify deve ler automaticamente o `netlify.toml`.
5. Clique em **Deploy site**.

## 3) Conferir build settings

Caso precise validar manualmente:

- **Base directory:** `frontend`
- **Build command:** `npm run build`
- **Publish directory:** `frontend/dist` (ou `dist` quando usando base `frontend`)

## 4) SPA routing

O redirect `/* -> /index.html (200)` já está incluído no `netlify.toml` para rotas do Vue Router funcionarem ao recarregar a página.

## 5) Deploy automático

Depois de conectado ao GitHub, cada `git push` para a branch principal gera um novo deploy automático.
