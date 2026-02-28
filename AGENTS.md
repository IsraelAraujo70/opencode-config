# OpenCode Global Instructions

## Agents Disponiveis

Use `@agent` para invocar um agent especializado:

| Agent | Modelo | Funcao |
|-------|--------|--------|
| `@commit` | glm-4.7-free | Gera commits convencionais e cria PRs com gh cli |
| `@docs` | glm-4.7-free | Documenta codigo no padrao GarantiaBR |
| `@explain` | glm-4.7-free | Explica codigo e arquitetura (read-only) |
| `@websearch` | glm-4.7-free | Busca documentacao e solucoes na web |
| `@bruno` | glm-4.7-free | Gera requisicoes Bruno a partir de contracts |
| `@search` | glm-4.7-free | Busca rapida na codebase (rg, fd) |
| `@review` | glm-4.7-free | Code review focado em bugs e riscos |
| `@diagram` | glm-4.7-free | Converte imagens de diagramas para Mermaid |
| `@e2e` | glm-4.7-free | Testa fluxos E2E em APIs com curl |

## Quando Usar Cada Agent

- **Commit**: Apos fazer alteracoes, use `@commit` para gerar mensagem de commit padronizada e criar PR
- **Docs**: Para criar/atualizar documentacao de APIs, contratos, READMEs
- **Explain**: Para entender codigo sem modificar nada
- **Websearch**: Para buscar docs oficiais, exemplos, solucoes no StackOverflow
- **Bruno**: Para criar requisicoes na API collection do Bruno
- **Search**: Para encontrar arquivos, funcoes, referencias rapidamente
- **Review**: Para revisar codigo antes de merge/commit
- **Diagram**: Para converter imagens de draw.io/diagramas para Mermaid
- **E2E**: Para testar APIs de ponta a ponta (descobre config, roda servidor em background, testa com curl)

## Padroes GarantiaBR

### Estrutura de Documentacao
```
projeto/
  docs/
    llm-guidelines.md        # Arquivo principal
    ARQUITETURA_ATUAL.md     # Arquitetura do sistema
    DOMAINS.md               # Dominios do sistema
    SERVICES.md              # Servicos externos
    TESTING.md               # Padroes de teste
    Contracts/               # Contratos de API
      {rota}/
        get.md
        post.md
        put.md
        delete.md
        sqs.md               # Para filas SQS
  AGENTS.md -> docs/llm-guidelines.md  # Symlink
  CLAUDE.md -> docs/llm-guidelines.md  # Symlink
  GEMINI.md -> docs/llm-guidelines.md  # Symlink
```

### API Collections (Bruno)
- Localizacao: `~/projetos/garantiabr/api-collections/{collection}/`
- Estrutura: `api/{rota}/{Requisicao}.bru`
- Ambientes: `environments/{Ambiente}.bru`

### Conventional Commits
- feat: nova funcionalidade
- fix: correcao de bug
- docs: documentacao
- refactor: refatoracao
- test: testes
- chore: manutencao

## Regras Gerais

1. Sempre prefira comandos nao destrutivos
2. Cite arquivos com path:linha ao apontar problemas
3. Use portugues nas mensagens de commit e documentacao
4. Siga os padroes do projeto existente


No jj, você cria “workspaces” escolhendo o caminho da pasta na hora.

  cd /home/israel/projetos/garantiabr/notifier

  # cria 2 workspaces para agentes
  jj workspace add ../notifier-agent-1 --name agent-1 -r @
  jj workspace add ../notifier-agent-2 --name agent-2 -r @

  # listar workspaces
  jj workspace list

  # ver caminho físico de um workspace
  jj workspace root --name agent-1

  Onde ficam as pastas:

  - As pastas de trabalho ficam exatamente no DESTINATION que você passou (../notifier-agent-1, etc).
  - Não tem “pasta central obrigatória” como no git worktree; você define o layout.
  - Metadados do jj ficam no .jj de cada workspace (com estado de working copy).

  Pra remover:

  jj workspace forget agent-1   # remove do controle do jj
  rm -rf ../notifier-agent-1    # apaga a pasta do disco (opcional)

  Se quiser, eu já monto um padrão único pra todos os repos tipo ../_ws/<repo>/agent-1, agent-2, agent-
