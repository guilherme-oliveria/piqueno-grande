# Plano de execução: site de boas-vindas ao Piqueno

## Visão do produto

Uma página única, leve e divertida, que dá boas-vindas ao Piqueno. Ao abrir o link, ele vê o nome dele em destaque, uma mensagem pessoal do Guilherme, algumas fotos e um botão que solta confete. O site é estático (HTML, CSS e JavaScript puros, sem frameworks e sem build) e é publicado no GitHub Pages a partir da `main`.

## Decisões padrão (podem ser alteradas pelo Guilherme)

| Tema | Padrão proposto |
|---|---|
| Tom | Descontraído e carinhoso, com humor leve (o apelido "Piqueno" brinca com o nome do repo "piqueno-grande"). |
| Idioma do site | Português do Brasil. |
| Paleta | Fundo creme `#FFF8EC`, primária laranja `#FF7A00`, secundária azul `#1E3A8A`, texto `#1F2937`. |
| Fonte | Fonte do sistema (`system-ui`), sem dependências externas. |
| Fotos | Se ainda não houver fotos, usar 3 placeholders SVG em `assets/img/`, trocados depois. |
| Mensagem | Texto provisório escrito pelo PO, marcado com `<!-- TODO: mensagem do Guilherme -->` até o Guilherme enviar a versão final. |
| Hospedagem | GitHub Pages, publicado por GitHub Actions a cada push na `main`. |

## Fluxo de trabalho

1. Cada tarefa abaixo vira uma branch `feature/<nome>` criada a partir de `develop`.
2. O Dev abre um PR para `develop` com o número da tarefa no título (ex.: `T2: seção de boas-vindas`) e a checklist de critérios de aceite copiada no corpo.
3. O Codex (QA) revisa o PR contra os critérios de aceite desta página e aprova ou pede mudanças.
4. O Guilherme aprova e faz o merge. Só então o Dev começa a próxima tarefa.
5. Ao final (depois da T8), o Dev cria `release/1.0.0` a partir de `develop` e abre o PR dessa branch para `main`. O merge na `main` publica o site e dispara a pipeline de release, que cria a tag `v1.0.0` e abre o PR de retorno `backmerge/v1.0.0` → `develop` (seção "Release 1.0.0").

Regras gerais que valem para todo PR (o QA deve checar sempre):

- O PR toca apenas os arquivos do escopo da tarefa.
- Sem dependências externas em tempo de execução (CDNs, fontes remotas, bibliotecas JS).
- O HTML passa no validador da T1 (a partir da T2).
- Abrir `index.html` direto no navegador funciona sem erros no console.
- Textos visíveis em português, sem erros de digitação.

## Estrutura de pastas alvo

```
index.html
css/styles.css
js/main.js
assets/img/
docs/PLANO.md
.github/workflows/
```

---

## T1: Base do projeto e CI de validação

**Branch:** `feature/base-projeto`

**Objetivo:** criar o esqueleto do site e uma verificação automática que roda em todo PR.

**Escopo:**
- `index.html` com `<!DOCTYPE html>`, `lang="pt-BR"`, `meta charset`, `meta viewport`, `<title>Bem-vindo, Piqueno!</title>`, link para `css/styles.css` e `js/main.js` (com `defer`).
- `css/styles.css` com reset básico e as variáveis CSS da paleta (`--cor-fundo`, `--cor-primaria`, `--cor-secundaria`, `--cor-texto`).
- `js/main.js` vazio ou com um comentário.
- `.editorconfig` (UTF-8, LF, 2 espaços) e `.gitignore` (ex.: `node_modules/`, `.DS_Store`).
- `.github/workflows/validar.yml` rodando em `pull_request` para `develop` e `main`: valida o HTML com `html-validate` (via `npx`) e falha se houver erros.

**Critérios de aceite:**
- [ ] Os arquivos acima existem nos caminhos indicados.
- [ ] `index.html` abre no navegador mostrando uma página em branco com fundo creme e título da aba "Bem-vindo, Piqueno!".
- [ ] As quatro variáveis de cor estão em `:root` em `css/styles.css`.
- [ ] O workflow `validar.yml` roda no próprio PR e termina verde.
- [ ] Nenhuma dependência é adicionada ao repositório (sem `package.json` obrigatório; `npx` no CI é aceito).

---

## T2: Seção de boas-vindas (hero)

**Branch:** `feature/hero`

**Objetivo:** a primeira coisa que o Piqueno vê é o nome dele em destaque.

**Escopo:**
- `<header>` ou `<section id="boas-vindas">` com `<h1>` "Bem-vindo, Piqueno!" e um subtítulo curto (ex.: "Pequeno no apelido, grande na amizade.").
- Seção ocupando pelo menos a altura da tela (`min-height: 100vh` ou equivalente) e conteúdo centralizado.
- Um link âncora "Ver mensagem" que leva para `#mensagem` (a seção será criada na T3; o link pode apontar para ela já).

**Critérios de aceite:**
- [ ] Existe exatamente um `<h1>` na página, com o texto "Bem-vindo, Piqueno!".
- [ ] Em 1366x768 e em 375x667 a seção ocupa a tela inteira e o texto não transborda horizontalmente.
- [ ] O `<h1>` usa `--cor-primaria` ou `--cor-secundaria`, e o contraste com o fundo é de pelo menos 4.5:1.
- [ ] CI verde.

---

## T3: Mensagem do Guilherme

**Branch:** `feature/mensagem`

**Objetivo:** mostrar a mensagem pessoal de boas-vindas.

**Escopo:**
- `<section id="mensagem">` com `<h2>` (ex.: "Um recado pra você") e 2 a 4 parágrafos.
- Assinatura "— Guilherme" no fim.
- Se a mensagem final ainda não tiver sido enviada, usar texto provisório e marcar com `<!-- TODO: mensagem do Guilherme -->`.
- Largura máxima do texto entre 60 e 75 caracteres por linha (ex.: `max-width: 65ch`).

**Critérios de aceite:**
- [ ] A seção tem `id="mensagem"` e o link "Ver mensagem" da T2 rola até ela.
- [ ] Existe um `<h2>` na seção e a assinatura do Guilherme aparece.
- [ ] O bloco de texto tem `max-width` em `ch` entre 60 e 75.
- [ ] Se o texto for provisório, o comentário TODO está presente.
- [ ] CI verde.

---

## T4: Galeria de fotos

**Branch:** `feature/galeria`

**Objetivo:** mostrar algumas fotos ou momentos com o Piqueno.

**Escopo:**
- `<section id="galeria">` com `<h2>` (ex.: "Alguns momentos").
- Grade com 3 a 6 imagens em `assets/img/`. Sem fotos reais, usar placeholders SVG criados no PR.
- Cada `<img>` com `alt` descritivo, `width` e `height` definidos e `loading="lazy"`.
- Layout em CSS Grid: 1 coluna em telas menores que 600px, 2 ou 3 colunas acima disso.
- Cada imagem com no máximo 300 KB.

**Critérios de aceite:**
- [ ] Todas as imagens têm `alt` não vazio, `width`, `height` e `loading="lazy"`.
- [ ] Em 375px de largura a grade tem 1 coluna; em 1366px tem 2 ou 3.
- [ ] Nenhum arquivo em `assets/img/` passa de 300 KB.
- [ ] Nenhuma imagem é carregada de fora do repositório.
- [ ] CI verde.

---

## T5: Botão de confete

**Branch:** `feature/confete`

**Objetivo:** um toque divertido: o Piqueno clica e cai confete na tela.

**Escopo:**
- Um `<button>` (ex.: "Comemorar 🎉") na seção de boas-vindas ou no rodapé.
- Animação de confete feita em `js/main.js` com JavaScript puro (canvas ou elementos DOM), sem bibliotecas.
- A animação dura no máximo 4 segundos e limpa os elementos ou o canvas ao terminar.
- Se o usuário tiver `prefers-reduced-motion: reduce`, não animar; mostrar uma mensagem curta (ex.: "Uhuu! 🎉") no lugar.

**Critérios de aceite:**
- [ ] O botão é um `<button>` real, acessível por teclado (Tab + Enter ou Espaço disparam).
- [ ] Clicar mostra confete e, após no máximo 4s, não sobram elementos de confete no DOM (ou o canvas é removido ou limpo).
- [ ] Clicar várias vezes seguidas não trava a página nem gera erros no console.
- [ ] Com `prefers-reduced-motion: reduce` ativado, não há animação e a mensagem alternativa aparece.
- [ ] Nenhuma biblioteca externa foi adicionada.
- [ ] CI verde.

---

## T6: Rodapé e acabamento visual

**Branch:** `feature/rodape-acabamento`

**Objetivo:** fechar a página e dar consistência visual.

**Escopo:**
- `<footer>` com uma frase curta (ex.: "Feito com carinho pelo Guilherme · 2026").
- Favicon em `assets/` (SVG ou emoji em SVG) referenciado no `<head>`.
- Meta tags `description` e Open Graph (`og:title`, `og:description`, `og:image`) para o link ficar bonito ao ser compartilhado no WhatsApp.
- Espaçamentos e tamanhos de fonte consistentes entre as seções.

**Critérios de aceite:**
- [ ] O `<footer>` existe e aparece no fim da página.
- [ ] O favicon aparece na aba do navegador.
- [ ] `meta name="description"`, `og:title`, `og:description` e `og:image` estão presentes; `og:image` aponta para um arquivo existente no repo.
- [ ] CI verde.

---

## T7: Acessibilidade e responsividade

**Branch:** `feature/acessibilidade`

**Objetivo:** garantir que o site funciona bem para todos e em qualquer tela.

**Escopo:**
- Rodar o Lighthouse (mobile) e corrigir o que for apontado em Acessibilidade e Boas práticas.
- Foco visível em links e botões (`:focus-visible`).
- Revisar hierarquia de títulos (um `h1`, seções com `h2`).
- Adicionar ao PR um print ou o resumo do relatório Lighthouse.

**Critérios de aceite:**
- [ ] Lighthouse mobile: Acessibilidade ≥ 95, Boas práticas ≥ 90, Desempenho ≥ 90 (print ou resumo no PR).
- [ ] Navegando só com Tab, todo elemento interativo recebe foco visível.
- [ ] Sem rolagem horizontal em 320px de largura.
- [ ] CI verde.

---

## T8: Publicação no GitHub Pages e release

**Branch:** `feature/deploy-pages`

**Objetivo:** colocar o site no ar.

**Escopo:**
- `.github/workflows/deploy.yml` que publica a raiz do repositório no GitHub Pages a cada push na `main` (usando `actions/upload-pages-artifact` e `actions/deploy-pages`).
- Atualizar o `README.md` com o link do site publicado e como rodar localmente (abrir `index.html`).
- O README já pode trazer a URL esperada: `https://guilherme-oliveria.github.io/piqueno-grande/`.
- `.github/workflows/release.yml` que roda quando um PR de `release/X.Y.Z` é mergeado na `main` (`pull_request` do tipo `closed` com `merged == true`, base `main` e head começando com `release/`) e:
  1. lê a versão do nome da branch e cria e envia a tag `vX.Y.Z` no commit de merge da `main`;
  2. cria a branch `backmerge/vX.Y.Z` a partir da `main` e abre um PR dela para `develop` (título `Backmerge vX.Y.Z`).

**Ação do Guilherme:** em Settings → Pages, escolher "GitHub Actions" como fonte; em Settings → Actions → General, marcar "Allow GitHub Actions to create and approve pull requests" (necessário para o PR de backmerge).

**Observação para o Dev:** PRs abertos com o `GITHUB_TOKEN` não disparam outros workflows, então o `validar.yml` não roda sozinho no PR de backmerge. Inclua `workflow_dispatch` no `validar.yml` ou documente no README que o QA confere o backmerge pelo diff.

**Critérios de aceite (verificados no PR da T8, antes do merge em `develop`):**
- [ ] O workflow `deploy.yml` tem como gatilho apenas `push` na `main` (e opcionalmente `workflow_dispatch`), então não roda neste PR.
- [ ] O workflow declara `permissions` com `pages: write` e `id-token: write`, usa `actions/configure-pages`, `actions/upload-pages-artifact` (com `path: .`) e `actions/deploy-pages`, e define o ambiente `github-pages`.
- [ ] O workflow `release.yml` só roda em PR mergeado de `release/*` para `main`, declara `permissions` com `contents: write` e `pull-requests: write`, cria a tag `vX.Y.Z` a partir do nome da branch e abre o PR `backmerge/vX.Y.Z` → `develop` partindo da `main` (não da branch de release).
- [ ] Os YAMLs são válidos (ex.: `actionlint` ou `npx yaml-lint` sem erros, saída colada no PR).
- [ ] O `README.md` tem a URL esperada do site e instruções para rodar localmente.
- [ ] CI verde.

---

## Release 1.0.0 (depois da T8)

**Branch:** `release/1.0.0`, criada a partir de `develop` com tudo de T1 a T8 já mergeado.

**Escopo:**
- Abrir PR de `release/1.0.0` para `main` (título `Release 1.0.0`). Na branch de release só entram ajustes finais pequenos (ex.: versão no README); nada de funcionalidade nova.
- Depois do merge na `main`, nada é feito à mão: a pipeline `release.yml` (T8) cria a tag `v1.0.0` no commit de merge da `main` e abre o PR `backmerge/v1.0.0` → `develop`, que passa pelo QA e é mergeado normalmente. Como o backmerge parte da `main`, ele inclui o commit de merge da release.

**Critérios de aceite do PR de release (antes do merge):**
- [ ] A branch se chama `release/1.0.0`, nasceu de `develop` e o PR aponta para `main`.
- [ ] O diff contra `main` contém apenas o que já foi aprovado em T1 a T8 (mais ajustes finais mínimos).
- [ ] CI verde.

**Verificação pós-merge (feita pelo QA ou pelo Guilherme depois do merge na `main`):**
- [ ] O workflow `deploy.yml` rodou na `main` e terminou verde.
- [ ] `https://guilherme-oliveria.github.io/piqueno-grande/` abre o site sem erros no console.
- [ ] A pipeline `release.yml` rodou verde, a tag `v1.0.0` existe e aponta para o commit de merge da release na `main`.
- [ ] O PR `backmerge/v1.0.0` → `develop` foi aberto pela pipeline e, depois do merge, `develop` contém todos os commits da `main` (`git merge-base --is-ancestor origin/main origin/develop` retorna sucesso).

---

## Pendências com o Guilherme

Estas informações melhoram o resultado, mas nenhuma bloqueia o início. O Dev usa os padrões acima até chegarem:

1. A mensagem de boas-vindas (necessária até a T3, senão fica provisória).
2. Fotos com o Piqueno (necessárias até a T4, senão ficam placeholders).
3. Se "Piqueno" é o nome que deve aparecer ou se há um nome real/outro apelido.
4. Se a paleta e o tom propostos estão bons.
