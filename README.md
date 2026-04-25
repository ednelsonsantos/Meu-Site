# Meu Site — Ednelson dos Santos

Blog pessoal estático hospedado no Cloudflare Pages.

## Tecnologias

- HTML estático puro (sem WordPress, sem PHP, sem build step)
- Tema visual: Dark Minimalistblogger (CSS do tema mantido, PHP/plugins removidos)
- Hospedagem: Cloudflare Pages (Workers & Pages)

## Estrutura

```
/                        → Página inicial (estilo terminal)
/blog/                   → Lista de posts
/todos-os-posts/         → Todos os posts
/category/<slug>/        → Posts por categoria
/category/audios/        → Player de áudios
/minha-colecao/          → Coleção física (DVD/Blu-ray/4K) carregada via JSON
/<slug>/                 → Post individual
/audios/                 → Arquivos de áudio (.mp3)
/wp-content/uploads/     → Imagens dos posts e logos
_headers                 → Regras de cache e headers de segurança (Cloudflare)
_redirects               → Redirecionamentos e bloqueios de rotas PHP (Cloudflare)
wrangler.toml            → Configuração do deploy via Cloudflare Workers & Pages
```

## Deploy

O deploy é automático via integração GitHub → Cloudflare Pages.

A cada `git push` na branch `main`, o Cloudflare faz o deploy direto dos arquivos estáticos.

### Configuração do build no Cloudflare

| Campo | Valor |
|---|---|
| Build command | `echo "done"` |
| Build output | *(raiz do repositório)* |
| Root directory | *(vazio)* |

## Categorias e páginas no menu

| Label no nav | URL |
|---|---|
| Home | `/` |
| Assisti/Assisto | `/category/o-que-estou-assistindo/` |
| Recomendados | `/category/recomendados/` |
| Autismo | `/category/o-que-eu-sei-e-aprendi-sobre-o-autismo/` |
| Hobbies | `/category/hobbies/` |
| Meus Posts | `/category/meus-posts/` |
| Áudios | `/category/audios/` |
| Minha Coleção | `/minha-colecao/` |
| Sobre Mim | `/category/sobre-mim/` |

## Meus Posts (Mastodon)

A categoria **Meus Posts** (`/category/meus-posts/`) exibe em tempo real os últimos 10 posts do Mastodon via API pública, sem necessidade de commit ou deploy.

- Conta: `@ednelsonchado@mas.to`
- API: `https://mas.to/api/v1/accounts/:id/statuses?limit=10`
- Atualização: automática a cada visita (JavaScript no cliente)

## Painel XMPP

Todas as páginas têm um painel flutuante na lateral direita para entrar na sala XMPP **Blambers** e links de cadastro em servidores XMPP.

- Sala: `xmpp:blambers@conference.isacloud.cc?join`
- Cadastro: [isacloud.com.br](https://isacloud.com.br/signup) · [xmpp.social.br](https://xmpp.social.br/registrar) · [voz.social.br](https://voz.social.br/registrar)

## Como publicar

O fluxo recomendado é usar o app local `app.py` (não versionado — ver abaixo):

```bash
python app.py
```

Abre em `http://127.0.0.1:5000` com três abas:

- **Post** — cria um novo post HTML completo, insere nos índices (blog, categoria, author, todos-os-posts, home) e publica no GitHub
- **Áudio** — adiciona uma faixa em `/category/audios/` com player integrado
- **Coleção** — adiciona/atualiza itens em `minha-colecao/colecao.json` (match por IMDb ID); a página `/minha-colecao/` lê o JSON dinamicamente via JavaScript

Alternativamente, via Git diretamente:

```bash
git add <arquivos>
git commit -m "descrição"
git push origin main
```

O Cloudflare Pages faz o deploy automaticamente em ~1 minuto após o push.

## Como criar um novo post (manual)

1. Crie a pasta `/<slug>/index.html` baseando-se em um post existente
2. Preencha título, data, categoria, conteúdo e imagem
3. Adicione o card nas páginas de listagem:
   - `blog/index.html`
   - `todos-os-posts/index.html`
   - `category/<categoria>/index.html`
   - `index.html` (home — formato linha terminal)
4. Faça commit e push

**Atenção:** certifique-se de incluir a classe `category-<slug>` no `<article>` do post e de todas as listagens onde ele aparece.

## Como adicionar um áudio

1. Coloque o arquivo `.mp3` na pasta `/audios/`
2. Abra `python app.py` → aba **Áudio**
3. Selecione o arquivo, preencha título e data
4. Clique **Adicionar Faixa** → **Publicar**

## Scripts locais (não versionados)

Os arquivos `*.py` e `*.sh` estão no `.gitignore` e não vão para o repositório. São ferramentas de uso local apenas:

| Arquivo | Função |
|---|---|
| `app.py` | Interface web Flask para criar posts, áudios e gerenciar a coleção |
| `criar-post.py` | Alternativa CLI para criar posts |
| `clean_html.py` | Remove metadados WordPress de HTMLs exportados |
| `publish.sh` | Script de commit e push rápido |

## Layout

A home usa estilo **terminal/texto puro**: lista de posts com data ISO, título e categoria entre colchetes — sem imagens, sem widgets. Fonte monoespaçada, fundo escuro.

Os posts individuais e páginas de categoria mantêm o layout do tema Dark Minimalistblogger com header, navegação e footer padrão.

## Limitações

- Formulário de comentários não funciona (aponta para `/wp-comments-post.php`)
- Feed RSS (`/feed/`) redireciona para `/blog/`
- Busca WordPress não funciona (depende de AJAX/PHP)
