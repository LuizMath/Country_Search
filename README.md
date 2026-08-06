# 🌍 Country Search

Aplicação web para explorar informações de todos os países do mundo: busca por nome, filtro por continente, página de detalhes com dados completos e navegação pelos países vizinhos. Construída com React, TypeScript e a [REST Countries API](https://restcountries.com/).

🔗 **Demo:** https://luizmath.github.io/country_search

---

## Funcionalidades

- **Listagem completa** de países com bandeira, população, região e capital
- **Busca por nome** em tempo real, sem recarregar a página
- **Filtro por continente** (África, América, Ásia, Europa e Oceania), com requisição dedicada à API
- **Página de detalhes** (`/:name`) com nome nativo, população, região e sub-região, capital, domínio de topo, moedas e idiomas
- **Navegação por fronteiras:** os países vizinhos aparecem como botões clicáveis, resolvidos a partir do código `cca3`
- **Tema claro e escuro**, alternado pelo cabeçalho e propagado por toda a interface
- **Estados de carregamento e de vazio** tratados: spinner durante as requisições e mensagem própria quando a busca não retorna resultados

---

## Stack

| Área | Tecnologia |
|---|---|
| Base | React 18 · TypeScript · Vite |
| Estilos | styled-components (com `ThemeProvider`) |
| Rotas | React Router 6 |
| HTTP | Axios com instância e interceptor configurados |
| Feedback | react-loader-spinner |
| Deploy | GitHub Pages via `gh-pages` |

---

## Decisões técnicas

- **Camada de serviços isolada.** Cada chamada à API vive em `src/services` (`GetAllCountry`, `GetRegionCountry`, `GetIndividualCountry`, `GetAllCountryCode`), tipada com o tipo `Country`. Os componentes não sabem como o dado chega — só que ele chega.

- **Instância única do Axios** em `src/utils/Config.ts`, com `baseURL` vinda de variável de ambiente e um **interceptor de resposta** que, em caso de falha, dispara um `CustomEvent` no `window`. Isso desacopla o tratamento de erro da chamada em si.

- **Tema via Context.** O `ToggleThemeContext` guarda apenas o booleano `isDarkTheme`; a troca de paleta acontece no `ThemeProvider` do styled-components, então nenhum componente precisa saber qual tema está ativo — só consome `theme` no template.

- **Duas estratégias de filtro, de propósito.** A região filtra no servidor (endpoint `region/`), porque a API já oferece isso e reduz o payload. O nome filtra no cliente, sobre a lista já carregada, para dar resposta instantânea enquanto o usuário digita.

- **Estilos co-localizados por componente** em `src/styles/<Componente>/Styles.ts`, mantendo o CSS perto de quem o usa sem misturar com a lógica do `.tsx`.

---

## Rodando localmente

### Pré-requisitos
- [Node.js](https://nodejs.org/) 18+

### Instalação

```bash
git clone https://github.com/LuizMath/country_search.git
cd country_search
npm install
```

### Variável de ambiente

Crie um `.env` na raiz:

```env
VITE_BASE_URL=https://restcountries.com/v3.1/
```

> A barra final é obrigatória — os serviços concatenam os caminhos (`all`, `region/europe`, `name/brazil`, `alpha/BRA`) diretamente sobre essa base.

### Subir o projeto

```bash
npm run dev
```

A aplicação roda em `http://localhost:5173/country_search` — o `base` definido no `vite.config.ts` vale também em desenvolvimento.

---

## Scripts

```bash
npm run dev      # servidor de desenvolvimento
npm run build    # checagem de tipos + build de produção
npm run preview  # pré-visualiza o build localmente
npm run lint     # ESLint (zero warnings permitidos)
npm run deploy   # build e publicação no GitHub Pages
```

---

## Estrutura

```
src/
├── components/   # Header, cards, loader, detalhes, fronteiras, erro
├── routes/       # Home e CountryDetail
├── services/     # chamadas à REST Countries API
├── context/      # ToggleThemeContext (tema claro/escuro)
├── styles/       # styled-components por componente + tema global
├── types/        # Country, Theme, Config
└── utils/        # instância do Axios e interceptor
```

---

## Endpoints da API consumidos

| Serviço | Endpoint | Uso |
|---|---|---|
| `GetAllCountry` | `all` | listagem inicial |
| `GetRegionCountry` | `region/{regiao}` | filtro por continente |
| `GetIndividualCountry` | `name/{nome}` | página de detalhes |
| `GetAllCountryCode` | `alpha/{cca3}` | resolver os países de fronteira |

---

## Próximos passos

- Debounce no campo de busca, para evitar re-renderizações a cada tecla
- Memoizar a lista filtrada com `useMemo` em vez de refiltrar durante a renderização
- Substituir o índice do array por `cca3` como `key` das listas
- Tratar erro de rede com uma mensagem na interface, no lugar do alerta do navegador
- Persistir a escolha de tema entre visitas

---

## Licença

MIT
