# Piqueno Grande

Site de boas-vindas ao Piqueno.

## Fluxo de trabalho (Gitflow)

- `main`: versão publicada.
- `develop`: integração.
- `feature/*`: uma branch por tarefa, criada a partir de `develop`, com PR para `develop`.
- `release/X.Y.Z`: criada a partir de `develop`, com PR para `main`; depois do merge, tag `vX.Y.Z` e volta para `develop`.

O plano de execução fica em [`docs/PLANO.md`](docs/PLANO.md).
