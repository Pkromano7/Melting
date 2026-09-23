# Melting · Gestão de Contratos

Sistema interno da Melting para gestão de contratos, cotações, clientes e
planilhas — Fase 1 (piloto).

## Estrutura do projeto

```
melting-gestao-contratos/
├── index.html          → sistema principal
├── login.html           → tela de login
├── css/
│   ├── styles.css       → estilos do sistema
│   └── login.css        → estilos do login
├── js/
│   ├── app.js            → lógica do sistema (dados, filtros, régua, permissões, etc.)
│   ├── login.js           → lógica do login
│   └── supabase-client.js → configuração de conexão com o Supabase
├── assets/               → logos e imagens
└── supabase/              → scripts SQL (tabelas, permissões e colunas do banco)
    ├── schema.sql
    ├── perfis-e-permissoes.sql
    └── colunas-adicionais.sql
```

## Como rodar localmente

Não precisa de servidor nem instalação — é só abrir o `login.html` direto
no navegador.

## Configurar o Supabase

1. No [supabase.com](https://supabase.com), crie um projeto (ou use um já existente).
2. No **SQL Editor**, rode os três scripts da pasta `supabase/`, nesta ordem:
   `schema.sql` → `perfis-e-permissoes.sql` → `colunas-adicionais.sql`.
3. Em `js/supabase-client.js`, preencha `SUPABASE_URL` e `SUPABASE_ANON_KEY`
   com os dados do seu projeto (**Project Settings → API**).
4. Crie os usuários em **Authentication → Users**. O primeiro usuário nasce
   como `gerente` (somente leitura); para virar `admin` (acesso total), rode
   no SQL Editor:
   ```sql
   update public.perfis set papel = 'admin' where email = 'seu-email@melting.com.br';
   ```

## Permissões

- **admin** — acesso total (cria, edita e exclui contratos, cotações e clientes).
- **gerente** — acesso somente leitura (dashboard, contratos, cotações,
  planilhas e clientes), sem os botões de cadastro/edição/exclusão.

As regras são aplicadas tanto na interface quanto no banco (Row Level
Security), então mesmo sem os botões visíveis, o banco recusa qualquer
escrita de quem não é admin.
