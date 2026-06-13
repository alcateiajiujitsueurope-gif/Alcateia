# Alcateia Jiu Jitsu — Guia para Claude

## Sobre o projeto
Site institucional da **Alcateia Jiu Jitsu**, academia fundada por Lucas Feitosa em Mönchaltorf, Suíça. Lucas pratica jiu jitsu desde os 7 anos, hoje tem 28, e dirige a academia com a esposa e filho. É um projeto de vida — trate com cuidado.

- **Localização:** Mönchaltorf, 8617, Zürich Oberland, Schweiz
- **Contato do dono:** lucasfeitosabjj@gmail.com
- **Site ao vivo:** alcateiajiujitsu.com (Vercel)

---

## Regra principal — Operação cirúrgica

**Só mude o que foi pedido. Zero alterações por conta própria.**

Nunca:
- Alterar fotos (src, tamanho, ordem, qualidade) sem permissão explícita
- Alterar SEO (title, meta description, h1, URLs, structured data) sem permissão explícita
- Refatorar código que não foi pedido
- Adicionar funcionalidades não solicitadas
- Mudar layout, cores ou tipografia além do pedido
- Incluir links do Claude (`claude.ai/code/...`) em commit messages
- Mencionar Claude/AI em commits, PRs ou qualquer artefato do repositório

Sempre:
- Perguntar antes se a mudança toca em algo além do escopo pedido
- Commits limpos e técnicos, em inglês, sem referência a AI

---

## Stack técnica

- **8 páginas HTML estáticas** — sem framework, sem build step
- **Deploy:** push para `main` → GitHub Actions (`promote.yml`) → Vercel produção (~30s)
- **Branch de trabalho:** sempre `main` (ou branch nomeado pelo usuário na sessão)
- **Nunca** criar PRs sem pedido explícito do Lucas

### Páginas
| Arquivo | Conteúdo |
|---|---|
| index.html | Home (galeria, hero, community, CTA) |
| stundenplan.html | Grade de horários |
| klassen.html | Classes |
| mitgliedschaften.html | Memberships |
| uber-uns.html | Sobre nós |
| kontakt.html | Contato |
| impressum.html | Impressum (legal) |
| datenschutz.html | Datenschutz (privacidade) |

---

## Identidade visual — NÃO ALTERAR

### Fontes
- **Bebas Neue** — títulos, números grandes, horários
- **Montserrat** — corpo de texto, badges, botões

### Cores principais
- Fundo: `#080808` / `#0a0a0a` (dark puro)
- Azul destaque: `#1aa3d9` / `var(--blue)`
- Texto principal: `var(--cream)` / rgba(255,255,255,.85)
- Borda: `var(--border)` / rgba(255,255,255,.08)

### Filosofia visual
- Estética dark, premium, minimalista
- Sem emojis em código ou conteúdo
- Fotos são o coração do site — nunca redimensionar, recortar ou reordenar sem permissão
- Espaço em branco é intencional — não "corrigir" espaçamentos não pedidos

---

## Galeria (index.html) — estado aprovado

A galeria é um scroll horizontal com painéis. **Nunca alterar sem pedido.**

- `.gInner`: `gap:2px` (separador entre painéis)
- `.gPanel` rows: `52vh 31vh` (não `30vh` — causava desalinhamento)
- `.gPanelB` rows: `55vh 28vh`
- `.gPanelD` height: `calc(83vh + 2px)`
- `.gPanelFoto` height: `calc(83vh + 2px)`, `gap:2px`
- `.gPortrait`: `gap:2px` inline no HTML
- **2px é o gap aprovado.** 3px foi testado, 6px foi rejeitado ("ficou horrível")
- Todos os painéis têm altura total `83vh + 2px` para uniformidade

---

## Stundenplan (horários) — estado aprovado

### i18n
- Sistema DE/EN via `data-i18n` + objeto `translations` em JS
- Toda string nova precisa de entrada em **ambos** os blocos `de:{}` e `en:{}`

### Breakpoints
- `> 768px` → grade desktop (`.sgrid`, 7 colunas: hora + 6 dias)
- `< 768px` → cards mobile (`.smobile`, um card por dia)

### Cores dos badges
```
--c-sparring: #9aa0a6
--c-all:      #1aa3d9
--c-drills:   #f5d116
--c-nogi:     cor azul escuro
--c-women:    rosa/magenta
--c-kids1:    verde
--c-kids2:    verde mais escuro
--c-fund:     laranja
--c-adv:      roxo/escuro
```

### Grade atual (confirmada com Lucas)
- **Sexta 19:00–20:00:** Sparring (Gi/NoGi) — Nur Mitglieder ← NÃO é All Levels
- **Sábado 11:00–13:00:** Sparring (Gi/NoGi) — Nur Mitglieder

### Badge "Nur Mitglieder / Members only"
- Mobile: layout `.sdi-2row` — linha 1: [SPARRING] [Gi/NoGi] [NUR MITGLIEDER], linha 2: horário à direita
- Desktop: `.sc-members` com `position:absolute; top:4px; right:4px` dentro da célula `.sc`
- Cor: cinza semi-sólido `rgba(100,110,118,0.5)`, borda `rgba(255,255,255,0.15)`
- Chave i18n: `membersOnly` → DE: `'Nur Mitglieder'` / EN: `'Members only'`

---

## SEO — NUNCA TOCAR SEM PERMISSÃO EXPLÍCITA

As seguintes partes afetam diretamente o ranking no Google. Qualquer alteração deve ser discutida e aprovada antes:

- `<title>` de todas as páginas
- `<meta name="description">` de todas as páginas
- Tags `<h1>` (existe apenas uma por página — é intencional)
- Estrutura de URLs (nomes dos arquivos .html)
- Dados estruturados JSON-LD (se existirem)
- Alt texts das imagens principais

---

## Commits — regras

- Mensagens em inglês, técnicas, descritivas
- **Sem** URLs do Claude (`https://claude.ai/code/...`)
- **Sem** menção a Claude, AI, ou ferramentas de AI
- Exemplo correto: `Fix Friday schedule: Sparring replaces All Levels at 19:00`
- Exemplo errado: `Fix bug as Claude suggested https://claude.ai/code/...`

---

## Deploy

```
git add <arquivo(s)>
git commit -m "mensagem limpa"
git push -u origin main
# Vercel faz deploy automático em ~30 segundos
```

Se o deploy não aparecer → verificar GitHub Actions (`promote.yml`) antes de diagnosticar qualquer outra coisa. Esse foi um problema recorrente em sessões anteriores.

---

## Erros históricos — não repetir

1. **Fotos da galeria:** Em sessões anteriores Claude tentou alterar tamanhos de foto sem permissão — perdeu tempo pedindo atualização de navegador quando o verdadeiro problema era deploy não executado
2. **Gap da galeria:** 6px foi testado e rejeitado. O valor aprovado é **2px**. Não aumentar sem pedido
3. **Deploy manual:** Quando mudanças não aparecem no site, verificar PRIMEIRO se o push foi para `main` (não para branch de feature)
4. **Stundenplan sexta:** A aula das 19:00 de sexta já foi All Levels — foi corrigida para Sparring. Não reverter

---

## Planos futuros (não implementar sem pedido)

- Troca do link do Google Business Profile para `https://alcateiajiujitsu.com` (aguardando apontamento do domínio)
- Segundo Stundenplan para Tatami 2 (futuro)
- Adição de vídeos
- Troca de fotos (Lucas decide quando)
- Posts semanais no GBP

---

## Privacidade

- Repositório é **privado** — não compartilhar conteúdo, estrutura ou código com terceiros

---

## Informações de negócio — verificar antes de editar kontakt.html

| Campo | Valor |
|---|---|
| Endereço | Mettlenbachstrasse 29, 8617 Mönchaltorf, Zürich Oberland, Schweiz |
| WhatsApp | +41 77 267 56 69 |
| WhatsApp URL | `https://wa.me/41772675669` |
| Email | alcateiajiujitsu@protonmail.com |
| Google Maps | https://maps.app.goo.gl/n7ZvPL9iY6zQzTp18 |

Qualquer alteração nesses dados deve ser confirmada com Lucas antes de commitar.

---

## Dois sistemas de footer — NÃO misturar

### Footer A — apenas index.html
Classes próprias, não existem nas outras páginas:
```
.fGrid          → grid desktop (3 colunas)
.fNavTtl        → título de seção do footer
.fSchLink       → link "Stundenplan ansehen →"
.fLegal         → linha legal no rodapé
.fCopy          → texto de copyright
```
Background: `var(--dk2)` | Desktop padding: `56px 40px 28px` | Mobile padding: `32px 20px 20px`

### Footer B — stundenplan, klassen, uber-uns, kontakt, impressum, datenschutz, mitgliedschaften
```
.footer-inner   → container max-width:1280px
.footer-top     → grid 3 colunas com border-bottom
.footer-bottom  → linha de copyright + links legais
.footer-ttl     → título de seção
.footer-hours   → lista de horários
.footer-nav     → links de navegação
```
Background: `#080808` | Desktop padding: `4rem 2.5rem 2rem` | Mobile (≤768px): `2.5rem 1.5rem 1.5rem`

**Nunca usar classes do Footer A em páginas do Footer B e vice-versa.**

---

## Variáveis CSS por página — não são iguais em todos os arquivos

### index.html (sistema próprio)
```css
--dk:  #0a0a0a      /* fundo principal */
--dk2: #101010      /* fundo secundário / footer */
--dk3: #161616      /* fundo cards */
--bl:  #3da0d8      /* azul CTA (mais vivo) */
--blue:#4a9fd4      /* azul links/nav */
--cream: #f0ede6
--mu:  rgba(255,255,255,.52)
--H:   'Bebas Neue'
--B:   'Montserrat'
```

### Outras 7 páginas (sistema comum)
```css
--blue:      #4a9fd4
--dark:      #0a0a0a
--dark-3:    #181818
--dark-4:    #1f1f1f
--cream:     rgba(255,255,255,.88)
--cream-dim: rgba(255,255,255,.55)
--cream-faint:rgba(255,255,255,.25)
--border:    rgba(255,255,255,.08)
--border-strong: rgba(255,255,255,.18)
```

Se for preciso sincronizar alguma cor entre páginas, verificar em qual sistema cada arquivo está antes de copiar variáveis.

---

## Vercel — referência técnica

- **Projeto:** `alcateia-ovel`
- **URL de produção:** alcateiajiujitsu.com
- **Dashboard:** vercel.com/alcateiajiujitsueurope-6795s-projects/alcateia-ovel
- **Deploy trigger:** push para branch `main` via GitHub Actions (`.github/workflows/promote.yml`)
- Se o deploy não acontecer automaticamente → verificar a aba Actions no GitHub antes de qualquer outra coisa
- Não mencionar que o site foi feito/editado por Claude em nenhum lugar público
