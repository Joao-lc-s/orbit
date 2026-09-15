# Segurança do Orbit

O Orbit é uma aplicação estática. A configuração web do Firebase presente em
`main.js` identifica o projeto, mas não concede acesso aos dados por si só. A
proteção efetiva depende das regras do Firestore/Storage, do Firebase Auth e das
restrições configuradas no Google Cloud.

## Credenciais pessoais

As chaves de Gemini, YouTube e Firecrawl são mantidas somente no
`sessionStorage` da aba. Elas não são gravadas no estado local persistente nem
enviadas ao Firebase. Fechar a aba encerra essa sessão e exige informar as
chaves novamente.

Nunca inclua chaves privadas, contas de serviço, arquivos `.env` ou JSON de
credenciais no repositório. Se alguma chave desse tipo já foi publicada,
revogue-a no provedor; apenas removê-la do Git não é suficiente.

## Checklist antes de publicar

1. Publique `firestore.rules`, `database.rules.json` e `storage.rules` no projeto Firebase correto.
2. No Firebase Authentication, autorize `SEU-USUARIO.github.io` e o domínio
   personalizado, se houver.
3. No Google Cloud Console, restrinja a chave web do Firebase aos domínios do
   Orbit e somente às APIs usadas pelo projeto.
4. Ative Firebase App Check para Firestore, Realtime Database e Storage.
5. Crie manualmente o primeiro documento `allowed_users/SEU_EMAIL` com
   `isAdmin: true`. O próprio frontend não pode se promover a administrador.
6. Em GitHub > Settings > Pages, escolha **GitHub Actions** e mantenha **Enforce
   HTTPS** habilitado.

## Limite do GitHub Pages

GitHub Pages não permite configurar todos os cabeçalhos HTTP de segurança. O
Orbit inclui uma Content Security Policy por `<meta>`, mas ainda usa Tailwind
CDN e handlers inline legados; por compatibilidade, a política precisa permitir
`unsafe-inline` e `unsafe-eval`. Para uma CSP estrita, a próxima etapa é compilar
o Tailwind e migrar os handlers inline sem alterar a arquitetura do aplicativo.

As salas colaborativas no Realtime Database ficam disponíveis para usuários
autenticados. Para isolamento forte por participante, será necessário emitir
custom claims por um backend confiável; essa autorização não pode ser decidida
com segurança apenas pelo JavaScript servido no navegador.
