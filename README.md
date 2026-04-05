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

- Home
- O que estou assistindo / Assisti
- O que eu sei e aprendi sobre o Autismo
- Recomendados
- Sobre Mim
