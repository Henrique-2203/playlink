# PlayLink

Plataforma web para encontrar jogadores, formar equipes e participar de comunidades de jogos multiplayer. O sistema recomenda jogadores com base nos jogos em comum, horários e estilos de jogo.

## Tecnologias

- HTML, CSS e JavaScript
- Node.js e Express
- PostgreSQL e PGlite (ambiente local de testes)
- Zod (validação de dados)

## Estrutura

| Caminho | Conteúdo |
| --- | --- |
| `public/` | Interface web |
| `src/` | API, conexão e configuração do banco |
| `api/` | Entrada da API na Vercel |
| `sql/` | Modelagem, dados de exemplo, consultas e testes SQL |
| `test/` | Testes da API e da interface |
| `site-original/` | Protótipo anterior à integração com o banco |
| `diagrama-aprovado.jpg` | Diagrama inicial do banco |

## Execução local

Requer Node.js 20 ou superior.

```sh
npm install
npm start
```

Acesse http://localhost:3100. Sem `DATABASE_URL`, o servidor utiliza um banco temporário com dados de exemplo. Os dados desse modo são descartados ao reiniciar o servidor.

## PostgreSQL

As variáveis `DATABASE_URL` e `SESSION_SECRET` devem ser definidas no ambiente. Consulte `.env.example`. Em Node.js 20.6 ou superior, é possível carregar uma cópia desse arquivo, preenchida e renomeada para `.env`, com:

```sh
node --env-file=.env server.js
```

Para preparar um banco novo e descartável, execute `sql/01_schema.sql` e, se necessário, `sql/02_dados_exemplo.sql`. Também é possível executar os dois scripts com `node --env-file=.env src/setup.js`.

**Atenção:** o script `01_schema.sql` remove e recria o schema `playlink`, excluindo os dados existentes. Não execute em um banco com dados que precisem ser preservados. O script de dados de exemplo é destinado à demonstração.

O banco possui tabelas de usuários, jogos, grupos, avaliações, perfis, mensagens, fórum e demais relacionamentos. Triggers controlam a entrada de membros, a lotação dos grupos e a reputação dos jogadores.

## Testes

```sh
npm test
npm run test:sql
npx playwright install chromium
npm run test:site
```

Para executar os três conjuntos após instalar o Chromium, utilize `npm run test:all`. Os testes utilizam banco temporário.

## Publicação

Na Vercel, utilize a raiz deste repositório como diretório do projeto. Configure `DATABASE_URL` com a conexão PostgreSQL e `SESSION_SECRET` com um segredo exclusivo e estável. Não versione credenciais. Prepare o banco separadamente antes da publicação.

## Resenha

O perfil permite informar um nome de usuário do Resenha e disponibiliza um link externo para a plataforma. Essa associação é declarada pelo jogador, sem verificação de titularidade ou autenticação compartilhada. Os bancos dos dois projetos são independentes.

## Projeto acadêmico

O PlayLink foi desenvolvido como trabalho de faculdade, com foco em desenvolvimento web e modelagem de banco de dados.
