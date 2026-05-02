# Changelog

Todas as mudanças relevantes do site são documentadas aqui.
Formato baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).

---

## [2026-05-02]

### Corrigido
- **Menu mobile duplicado** — removido `<div class="super-menu">` (segundo botão "Menu") de todos os 99 HTMLs do site; o botão único agora fica fora do `<nav>` para não sumir com `display:none` do tema
- **Botão Menu não aparecia em mobile** — corrigida regra CSS: `.smenu-hide` passa a ter `display:block` diretamente, sem depender da classe `mobile-menu-active`; botão reposicionado abaixo do logo com separador visual
- **jQuery não carregava** — arquivo `/wp-includes/js/jquery/jquery.min.js` ausente no projeto estático; jQuery 3.7.1 adicionado localmente; sem jQuery o clique do menu não funcionava
- **Ícone hamburguer dependia de FontAwesome** — substituído pelo caractere Unicode `☰` (`&#9776;`) inline; CSS do `::before` desativado com `content:none`
- **Widget XMPP inacessível em mobile** — sem hover em toque; adicionada classe `.open` + script de toggle por clique em `#xmpp-tab`; fechar ao tocar fora funciona em todos os 100 HTMLs
- **Imagens dos cards de categoria com altura fixa** — `height:200px` trocado por `height:auto; aspect-ratio:16/9` em mobile (evita corte em imagens com proporções variadas)
- **Página Minha Coleção em branco no mobile** — HTML estava sem os containers `#col-grid`, `#col-stats`, `#col-toolbar` e sem CSS da coleção; página reconstruída com header padrão, CSS completo e estrutura correta
- **`app.py` gerava posts com layout desatualizado** — template `html_post_completo` corrigido: botão Menu fora do nav, `super-menu` removido, XMPP com toggle mobile

---

## [2026-04-25]

### Adicionado
- Página `/minha-colecao/` com grade visual de filmes, séries e animações da coleção física (DVD / Blu-ray / 4K)
  - Dados em `minha-colecao/colecao.json`, carregados via `fetch` no cliente
  - Filtro por categoria, busca por título, estatísticas de totais
- `CHANGELOG.md` — este arquivo

### Corrigido
- Autor exibido como **ednelsonchado** em todos os posts (era "Ednelson")
- Ordem dos itens do menu de navegação corrigida em todas as páginas de categoria e tag
- Post `escola-manager` removido da categoria `meus-posts` incorretamente associada

### Alterado
- Menu de navegação reordenado: Home → Assisti/Assisto → Recomendados → Autismo → Hobbies → Meus Posts → Áudios → Minha Coleção → Sobre Mim
- README atualizado com a nova estrutura de páginas e descrição do app local

### Interno (app.py local — não versionado)
- `api_criar` agora retorna listas `criados` e `atualizados` (corrigia crash silencioso no JS)
- `api_publicar` faz staging seletivo de arquivos HTML e JSON, excluindo `templates/` e scripts; aceita chave `titulo` além de `mensagem`; retorna `msg` na resposta
- `criar_post` atualiza também a página `author/ednelsonchadogmail-com/index.html`
- `esc()` passa a escapar aspas simples (`'` → `&#39;`)
- `prox_post_id()` verifica `index.html` como fallback além de `blog/index.html`
- `inserir_card_homepage()` detecta `\n` dinamicamente em vez de assumir `+1` fixo
- `_ler_colecao()` retorna lista vazia se `colecao.json` não existir (sem crash)
- `_atualizar_colecao_html()` removida — era código morto (`index.html` lê do JSON via JS)
- Template da aba Coleção: título da sidebar atualiza ao trocar de aba; painel direito exibe grade da coleção atual ao abrir a aba

---

## [2026-04-24]

### Adicionado
- Post: *Dreamworks Elio e como se sentir sozinho no mundo*
- Post: *The Pitt — a visão médica de outra maneira*
- Painel flutuante XMPP em todas as páginas (sala Blambers)
- Rodapé com links de navegação em todas as páginas

### Corrigido
- Links e CSS do tema restaurados após limpeza WordPress
- Menu "Home" apontando para `/` em vez de `/category/home/`

---

## [2026-04] — Início do projeto

### Adicionado
- Conversão do site WordPress para HTML estático puro
- Deploy automático via GitHub → Cloudflare Pages
- Todos os posts existentes migrados como arquivos `index.html`
- Player de áudios em `/category/audios/`
- Integração com Mastodon API em `/category/meus-posts/`
