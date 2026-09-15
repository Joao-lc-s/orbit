# Publicar o Orbit no GitHub Pages

## 1. Firebase

Instale a Firebase CLI, autentique-se e associe esta pasta ao projeto correto:

```bash
npm install -g firebase-tools
firebase login
firebase use --add
firebase deploy --only firestore:rules,database,storage
```

Depois conclua o checklist de `SECURITY.md`, especialmente domínios autorizados,
restrições da chave web e App Check.

## 2. GitHub

Crie um repositório vazio e execute nesta pasta:

```bash
git init
git add .
git commit -m "Preparar Orbit para GitHub Pages"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/SEU-REPOSITORIO.git
git push -u origin main
```

No repositório, abra **Settings > Pages > Source** e escolha **GitHub Actions**.
O workflow `.github/workflows/pages.yml` publica o site automaticamente. A
página inicial redireciona para `orbit.html`, portanto os caminhos continuam
funcionando em repositórios publicados sob `/NOME-DO-REPOSITORIO/`.

## 3. Teste local

Não abra o HTML diretamente pelo Explorador. Sirva a pasta por HTTP:

```bash
python -m http.server 8000
```

Acesse `http://localhost:8000/`. Teste login, salvar/recarregar, uploads, Widget
Studio, APIs opcionais e logout antes do primeiro push público.
