# Meu Site — Ednelson dos Santos

Blog pessoal hospedado no Cloudflare Pages, exportado como site estático a partir do WordPress.

## Tecnologias

- WordPress 6.8 (exportado como HTML estático via LiteSpeed Cache)
- Tema: Dark Minimalistblogger (filho de Minimalistblogger)
- Page builder: Elementor
- Hospedagem: Cloudflare Pages (Workers & Pages)

## Estrutura

```
/                        → Página inicial
/blog/                   → Lista de posts
/category/<slug>/        → Posts por categoria
/<slug>/                 → Post individual
/wp-content/             → Assets (CSS, JS, imagens, fontes)
/wp-includes/            → Assets do WordPress core
_headers                 → Regras de cache e headers de segurança (Cloudflare)
_redirects               → Redirecionamentos e bloqueios de rotas PHP (Cloudflare)
wrangler.toml            → Configuração do deploy via Cloudflare Workers & Pages
```

## Deploy

O deploy é automático via integração GitHub → Cloudflare Pages.

A cada `git push` na branch `main`, o Cloudflare executa:

1. **Build command:** copia os arquivos para a pasta `dist/` excluindo `.git`
2. **Deploy command:** `npx wrangler deploy` sobe os assets via `wrangler.toml`

### Configuração do build no Cloudflare

| Campo | Valor |
|---|---|
| Build command | `mkdir -p dist && find . -mindepth 1 -maxdepth 1 ! -name '.git' ! -name 'dist' ! -name 'README.md' ! -name 'wrangler.toml' -exec cp -r {} dist/ \;` |
| Deploy command | `npx wrangler deploy` |
| Path (output) | `dist` |

## Limitações do site estático

- Formulário de comentários não funciona (aponta para `/wp-comments-post.php`)
- Feed RSS (`/feed/`) redireciona para `/blog/`
- Busca WordPress não funciona (depende de AJAX/PHP)

## Categorias

| Label no nav | URL |
|---|---|
| Home | `/category/home/` |
| Meus Posts | `/category/meus-posts/` |
| Assisti/Assisto | `/category/o-que-estou-assistindo/` |
| Recomendados | `/category/recomendados/` |
| Autismo | `/category/o-que-eu-sei-e-aprendi-sobre-o-autismo/` |
| Hobbies | `/category/hobbies/` |
| Sobre Mim | `/category/sobre-mim/` |

## Meus Posts (Mastodon)

A categoria **Meus Posts** (`/category/meus-posts/`) exibe em tempo real os últimos 10 posts do Mastodon via API pública, sem necessidade de commit ou deploy.

- Conta: `@ednelsonchado@mas.to`
- API: `https://mas.to/api/v1/accounts/:id/statuses?limit=10`
- Atualização: automática a cada visita (JavaScript no cliente)

## Como publicar

```bash
bash publish.sh "descrição do que mudou"
```

O script faz commit dos arquivos HTML e assets, push para `origin/main` e o Cloudflare Pages faz o deploy automaticamente em ~1 minuto.

## Como criar um novo post

1. Copie a pasta `novo-post-exemplo/` e renomeie para o slug do post
2. Edite o `index.html` preenchendo os campos `<< >>`
3. Rode `bash publish.sh "Novo post: título"`

## Layout

O tema usa layout float-based (herança do WordPress). Ajustes aplicados para a versão estática:

- `#primary { width: 100%; float: none }` — remove o layout de duas colunas (sidebar removida)
- `#content .content-wrap { padding: 0 61px }` — breathing room lateral no conteúdo
- Media query `≤768px`: padding reduz para `16px`
