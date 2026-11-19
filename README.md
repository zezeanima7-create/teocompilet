# TeoCompile

Plataforma web em português para aprender Lua, praticar programação e entender como compiladores e interpretadores funcionam.

## Funcionalidades

- Trilha introdutória com seis módulos: saída, variáveis, operadores, condições, laços e funções.
- Editor de código com execução de Lua no navegador usando [Fengari](https://fengari.io/).
- Console de saída e mensagens de erro.
- Revisor IA que explica o código, aponta problemas e sugere melhorias.
- Aula visual sobre análise léxica, análise sintática e execução.
- Mini desafio e salvamento local do progresso.

## Executar localmente

Como o projeto usa recursos do navegador, sirva a pasta com um servidor HTTP local (por exemplo, a extensão Live Server do VS Code) e abra `index.html`.

O interpretador Fengari e a fonte da interface são carregados de CDNs, portanto a primeira abertura requer conexão com a internet.

## Ativar a revisão por IA

A interface chama `POST /api/review`. O endpoint fica em `worker/index.js`, para implantação como Cloudflare Worker. A chave de API fica no servidor e **não deve ser colocada no JavaScript do navegador**.

1. Instale o Wrangler: `npm install -g wrangler`.
2. Faça login: `wrangler login`.
3. Na raiz do projeto, configure o segredo: `wrangler secret put OPENAI_API_KEY`.
4. Publique o Worker: `wrangler deploy`.
5. Configure a URL do Worker no frontend ou publique o frontend e o Worker sob o mesmo domínio com uma rota `/api/*`.

Para desenvolvimento local, crie um arquivo `.dev.vars` com `OPENAI_API_KEY=sua-chave` (não faça commit desse arquivo).

## Publicar a interface no GitHub Pages

No GitHub, abra **Settings → Pages**, escolha **Deploy from a branch**, selecione `main` e a pasta `/ (root)`. Salve e aguarde a publicação.

> O GitHub Pages hospeda apenas os arquivos estáticos. A revisão por IA exige o Worker configurado separadamente.

## Estrutura

- `index.html` — estrutura da aplicação.
- `styles.css` — interface responsiva.
- `app.js` — navegação, editor, execução e revisão IA.
- `lessons.js` — conteúdo das aulas.
- `worker/index.js` — API segura para revisão por IA.
- `wrangler.toml` — configuração do Worker.

## Segurança e limitações

O código Lua é executado no navegador por Fengari, não em um servidor isolado dedicado. Use o laboratório para exercícios e exemplos, não para executar código não confiável em ambientes sensíveis. A revisão envia o código escrito para o provedor de IA quando solicitada.