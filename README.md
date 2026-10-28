# Állex Brandão — Portfólio

Site de portfólio pessoal em React, com projetos carregados da API do GitHub, formulário de contato e dashboard do dono.

> Em desenvolvimento. Este README é atualizado a cada funcionalidade integrada à `main`.

## Tecnologias

| Área        | Tecnologia                           |
| ----------- | ------------------------------------ |
| Base        | React 19 + Vite                      |
| Linguagem   | JavaScript (JSX)                     |
| Qualidade   | ESLint + Prettier                    |
| Ganchos Git | Husky + lint-staged + commitlint     |
| Testes      | Vitest + Testing Library + jsdom     |
| Deploy      | Vercel (deploy automático na `main`) |

## Como rodar

Pré-requisito: Node.js 20 ou superior.

```bash
git clone https://github.com/Allexbrandao07/allex-brandao-portfolio.git
cd allex-brandao-portfolio
npm install
cp .env.example .env   # preencha as variáveis
npm run dev
```

### Scripts

| Comando                | O que faz                                  |
| ---------------------- | ------------------------------------------ |
| `npm run dev`          | Servidor de desenvolvimento                |
| `npm run build`        | Build de produção em `dist/`               |
| `npm run preview`      | Serve o build localmente                   |
| `npm run lint`         | Verifica o código com ESLint               |
| `npm run format`       | Formata todos os arquivos com Prettier     |
| `npm run format:check` | Verifica a formatação sem alterar arquivos |
| `npm test`             | Roda os testes uma vez                     |
| `npm run test:watch`   | Roda os testes em modo observação          |

### Variáveis de ambiente

Veja [`.env.example`](.env.example). O arquivo `.env` nunca é versionado.

> Toda variável `VITE_*` é embutida no bundle e fica pública no navegador. Não coloque segredos nelas.

## Padrões do projeto

### Commits — Conventional Commits

Formato: `tipo(escopo opcional): descrição no imperativo, em português`

```
feat: adiciona validação do formulário de contato
fix: corrige redirecionamento do dashboard sem login
chore: configura Vitest e Testing Library
```

| Tipo       | Quando usar                                 |
| ---------- | ------------------------------------------- |
| `feat`     | Nova funcionalidade para o usuário          |
| `fix`      | Correção de bug                             |
| `chore`    | Configuração, dependências, ferramentas     |
| `docs`     | Documentação                                |
| `refactor` | Mudança de código sem alterar comportamento |
| `test`     | Criação ou ajuste de testes                 |
| `style`    | Formatação, sem mudança de lógica           |
| `perf`     | Melhoria de performance                     |

O **commitlint** rejeita mensagens fora desse formato.

### Branches — GitHub Flow

- `main` é sempre publicável e protegida: só recebe código via pull request.
- Cada funcionalidade nasce em uma branch curta a partir da `main`:

| Prefixo     | Uso                        | Exemplo                 |
| ----------- | -------------------------- | ----------------------- |
| `feature/`  | Nova funcionalidade        | `feature/dashboard`     |
| `fix/`      | Correção                   | `fix/rota-404`          |
| `chore/`    | Configuração e ferramentas | `chore/tooling`         |
| `docs/`     | Documentação               | `docs/readme`           |
| `refactor/` | Refatoração                | `refactor/project-card` |
| `test/`     | Testes                     | `test/contact-form`     |

Fluxo de uma alteração: `branch → commits → push → pull request (preview na Vercel) → merge na main → deploy de produção automático`.

### Verificação automática

A cada `git commit`, o Husky executa:

1. **pre-commit** → `lint-staged`: roda `eslint --fix` e `prettier --write` apenas nos arquivos do commit.
2. **commit-msg** → `commitlint`: valida a mensagem no padrão Conventional Commits.

Se algo falhar, o commit é bloqueado.

## Decisões técnicas

Para cada ferramenta: o que foi comparado, o critério da escolha e o que se aceitou perder.

| Ferramenta               | Função                              | Comparado com                 | Critério da escolha                                          | O que se aceitou perder                                      |
| ------------------------ | ----------------------------------- | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Vite                     | Build e servidor de desenvolvimento | Create React App, Next.js     | Rápido, simples, sem SSR desnecessário para um portfólio SPA | Renderização no servidor e SEO nativos do Next.js            |
| JavaScript               | Linguagem                           | TypeScript                    | Menor curva de aprendizado dentro do prazo                   | Verificação de tipos em tempo de desenvolvimento             |
| ESLint                   | Encontra erros e más práticas       | Oxlint                        | Padrão do ecossistema, plugins de React e Hooks              | Velocidade do Oxlint                                         |
| Prettier                 | Formatação automática               | Biome                         | Padrão do ecossistema, integra com ESLint e editor           | Uma ferramenta a mais; Biome faria lint e formatação juntos  |
| Husky                    | Executa ganchos do Git              | lefthook, simple-git-hooks    | Mais conhecido e documentado                                 | Ganchos locais podem ser pulados com `--no-verify`           |
| lint-staged              | Verifica só os arquivos do commit   | Rodar lint no projeto inteiro | Commits rápidos, mesmo com o projeto crescendo               | Uma dependência e um bloco de configuração a mais            |
| commitlint               | Valida a mensagem do commit         | commitizen, só documentar     | Aplica o padrão automaticamente, inclusive nos commits da IA | Atrito inicial quando a mensagem sai fora do padrão          |
| Vitest + Testing Library | Testes de componentes               | Jest, Playwright              | Usa a configuração do Vite; testa o que o usuário vê e faz   | jsdom não é um navegador real: não pega bugs de CSS e layout |

### Ajustes de configuração

- **`eslint-config-prettier`**: desliga as regras de estilo do ESLint para não conflitarem com o Prettier.
- **`.gitattributes` com `eol=lf`**: padroniza as quebras de linha em LF. No Windows o Git converte para CRLF, o que faria o Prettier acusar diferença em todos os arquivos.
- **`subject-case` desligada no commitlint**: permite descrições em português com qualquer capitalização.
