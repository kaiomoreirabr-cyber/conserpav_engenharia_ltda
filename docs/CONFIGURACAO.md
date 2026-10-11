# Configuração e Operação

Este documento descreve a configuração do Supabase, da autenticação e do ambiente de publicação.

## 1. Configurar o Supabase

1. Crie um projeto no [Supabase](https://supabase.com).
2. Abra o SQL Editor e execute [`supabase/schema.sql`](../supabase/schema.sql).
3. Em Project Settings > API, copie a Project URL e a chave `anon public`.
4. Em Authentication > Providers, mantenha o provedor de e-mail habilitado.
5. Em Authentication > Users, crie o usuário administrador do sistema.

O projeto utiliza um modelo mono-usuário. A aplicação não possui cadastro público: o usuário deve ser criado pelo administrador do projeto Supabase.

## 2. Configurar o ambiente local

Copie `.env.example` para `.env` e preencha:

```env
VITE_SUPABASE_URL=https://seu-projeto.supabase.co
VITE_SUPABASE_ANON_KEY=sua-chave-anon-publica
```

Instale as dependências e inicie o ambiente:

```bash
npm install
npm run dev
```

Não publique o arquivo `.env`. Ele já está incluído no `.gitignore`.

## 3. Recuperação de senha

O fluxo de recuperação depende do serviço de e-mail do Supabase:

1. Verifique se o provedor de e-mail está habilitado.
2. Configure as URLs permitidas em Authentication > URL Configuration.
3. Para testes, use o serviço padrão do Supabase e confira a caixa de spam.
4. Para produção, configure um SMTP transacional próprio.

O usuário pode solicitar a recuperação na tela de login e alterar a senha após abrir o link recebido.

## 4. Publicação

Na Vercel, configure as variáveis `VITE_SUPABASE_URL` e `VITE_SUPABASE_ANON_KEY`. O comando de build é `npm run build` e o diretório gerado é `dist`.

Após o primeiro deploy, valide o login no domínio de produção e confirme se as URLs de redirecionamento do Supabase incluem esse domínio.

## 5. Diagnóstico rápido

### A tela informa que o Supabase não está configurado

Confira se `.env` existe, se as variáveis começam com `VITE_` e reinicie o servidor após alterá-las.

### O login retorna credenciais inválidas

Confirme o e-mail, a senha e o status do usuário em Authentication > Users. Se a confirmação de e-mail estiver habilitada, confirme o usuário antes de testar.

### O login funciona, mas os dados não aparecem

Verifique se o schema atual foi executado no projeto correto e se a sessão permanece ativa. As tabelas estão protegidas por RLS e exigem autenticação.

### O e-mail de recuperação não chega

Confira spam, URL de redirecionamento e configuração SMTP. Consulte os logs do Supabase para identificar falhas de entrega.

### O deploy abre uma rota inexistente

Confirme se o `vercel.json` está no diretório raiz e se o deploy foi feito a partir da raiz do projeto.
