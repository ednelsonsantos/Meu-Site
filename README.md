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
/category/audios/        → Player de áudios
/<slug>/                 → Post individual
/audios/                 → Arquivos de áudio (.mp3)
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
2. **Deploy command:** `npx wrangler deploy` sobe os assets via Workers & Pages

### Configuração do build no Cloudflare

| Campo | Valor |
|---|---|
| Build command | `echo "done"` |
| Build output | *(raiz do repositório)* |
| Root directory | *(vazio)* |

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
| Áudios | `/category/audios/` |
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

O fluxo recomendado é usar o app local `app.py`:

```bash
python app.py
```

Abre em `http://127.0.0.1:5000` com duas abas:

- **Post** — cria um novo post HTML completo, insere nos índices e publica no GitHub
- **Áudio** — adiciona uma faixa em `/category/audios/` com player integrado

Alternativamente, via Git diretamente:

```bash
git add <arquivos>
git commit -m "descrição"
git push origin main
```

O Cloudflare Pages faz o deploy automaticamente em ~1 minuto após o push.

## Como criar um novo post (manual)

1. Copie a pasta `novo-post-exemplo/` e renomeie para o slug do post
2. Edite o `index.html` preenchendo os campos `<< >>`
3. Insira o card nas páginas de listagem (`blog/index.html`, `category/*/index.html`, `index.html`)
4. Faça commit e push

## Como adicionar um áudio

1. Coloque o arquivo `.mp3` na pasta `/audios/`
2. Abra `python app.py` → aba **Áudio**
3. Clique em 📂 para selecionar o arquivo, preencha título e data
4. Clique **Adicionar Faixa** → **Publicar**

## Layout

O tema usa layout float-based (herança do WordPress). Ajustes aplicados para a versão estática:

- `#primary { width: 100%; float: none }` — remove o layout de duas colunas (sidebar removida)
- `#content .content-wrap { padding: 0 61px }` — breathing room lateral no conteúdo
- Media query `≤768px`: padding reduz para `16px`
- Footer com Arquivos e Categorias colapsável por clique
- Open Graph / meta description em todos os posts
