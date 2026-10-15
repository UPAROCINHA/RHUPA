# GestãoSaúde RH: app fora do Base44

Este app é uma versão reconstruída do app do Base44. Ele usa só dois arquivos:

- `index.html`: o app inteiro (portal do colaborador + área de gestão)
- `supabase_schema.sql`: o banco de dados

## 1. Banco (Supabase)

1. Crie um projeto em supabase.com.
2. Abra **SQL Editor → New query**, cole todo o `supabase_schema.sql` e clique em **Run**.
3. Em **Authentication → Users → Add user**, crie o seu usuário (e-mail e senha).
4. No SQL Editor, rode o comando abaixo para virar administrador:
   `update public.perfis set papel = 'admin' where email = 'luisfelipeseroa@gmail.com';`
5. Em **Authentication → URL Configuration**, coloque o endereço do GitHub Pages em **Site URL**. Isso faz o link de "Esqueci a senha" funcionar.

## 2. Ligar o app ao banco

1. No Supabase, abra **Settings → API** e copie a **Project URL** e a chave **anon public**.
2. No topo do script do `index.html`, cole esses dois valores em `SUPABASE_URL` e `SUPABASE_ANON_KEY`.
   A chave anon pode ficar no código: ela é pública. A segurança é garantida pelas regras do banco.

## 3. Publicar (GitHub Pages)

1. Suba o `index.html` para um repositório.
2. Abra **Settings → Pages → Deploy from branch (main / root)**.

## 4. Importar os profissionais

Na área de gestão, abra **Profissionais → Importar CSV/Excel** e escolha o `profissionais.csv` exportado do Base44.

- O cargo de cada pessoa é convertido automaticamente. O texto original do cargo continua guardado.
- Na exportação do Base44, a coluna "Telefone" às vezes trazia a data de admissão. Nesses casos, a data vai para o campo **Data de admissão**.
- Pode importar de novo sem duplicar ninguém: quem já existe é atualizado pela matrícula.

## Endereços

| Endereço | O que é |
|---|---|
| `#/` | Entrada do portal (pela matrícula) |
| `#/portal/MATRÍCULA` | Portal do colaborador |
| `#/cadastro` | Autocadastro do colaborador |
| `#/admin` | Área de gestão (com login) |

## O que mudou em relação ao Base44

- **Login da gestão:** agora é login do Supabase (e-mail e senha). A senha extra de coordenador deixou de existir.
- **"Notificar coordenadores" e "Enviar resumo por e-mail":** abrem o seu e-mail já preenchido, com destinatários e texto, em vez de enviar sozinhos. Para envio automático, dá para criar depois uma Edge Function no Supabase.
- **Novos usuários da gestão:** são convidados pelo painel do Supabase (Authentication → Users). A página "Usuários" mostra a lista e permite mudar o nível de acesso.
- **"Gerar Escala":** roda no próprio app. As regras são:
  - Diarista: de segunda a sexta.
  - Plantão 12h pares ou ímpares: dias pares ou ímpares do mês.
  - Plantão 24h x 120h: a cada 6 dias.
  - Plantão 24h semanal: um dia fixo por semana.
- **Portal do colaborador:** não acessa as tabelas diretamente. Cada pessoa vê só os próprios dados, pela matrícula.
# RHUPA
novo
