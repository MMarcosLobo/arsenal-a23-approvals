# ARSENAL A23 — GitHub approval-only (proposta local)

**Nao publicado e nao ativado.** Este pacote sera publicado somente se o usuario autorizar explicitamente um repositorio PUBLICO de metadados separado de seu codigo.

Repositorio pretendido: `MMarcosLobo/arsenal-a23-approvals`. O codigo do ARSENAL, `arsenal-a23-benchmark`, Cotton Flow e Central de Demandas permanecem privados e inalterados.

## Ambiente protegido obrigatorio no GitHub
1. `arsenal-a23-local-approval` com Required reviewers incluindo `MMarcosLobo`.
2. `Prevent self-review` ativado, `Prevent admins bypass` ativado.
3. Workflow pinado a um SHA especifico, independente do SHA do projeto ARSENAL.
4. Workflow disparado por um BOT/GitHub App que NAO seja o revisor humano.
5. Usuario revisa o titulo com SHA `ac5bc2c41521ab7b5a0adaf1b6d8067c3bb39ea6` e digest das 22 dependencias, e aprova no GitHub.
6. O workflow possui um job de verificacao simples, sem deploy ou publicacao.

## Verificacao independente
O script `CONTROL-PLANE/scripts/a23-github-approval-check.mjs` consulta a API PUBLICA do GitHub em modo ANONIMO (sem token) e valida: identidade do repo, SHA do workflow pinado, ambiente protegido, reviewer humano, self-review negada, bypass administrativo negado, acionador distinto, run aprovada e recente com SHA/digest exatos, job correto.

A policy `.ai/A23-GITHUB-APPROVAL-POLICY.json` permanece `enabled:false` ate que a infraestrutura externa exista. O A23 permanece bloqueado.

## Bloqueio importante
O cliente `gh` atual no Windows esta autenticado como `MMarcosLobo`, conta humana que seria a revisora. Se essa credencial tiver capacidade de aprovar o run, qualquer agente local com acesso ao token pode tentar aprovar sozinho via API. A separacao de credenciais/autoridades e requisito antes de ativar o gate, nao apenas opcao.

Em repositórios privados, GitHub Free/Pro/Team nao oferece required reviewers. O repositorio minimo publico seria APENAS de aprovacao e NAO incluiria codigo-fonte, secrets, artefatos ou manifestos completos. A alternativa de manter o fluxo privado e contratar plano GitHub Enterprise e definir protecoes equivalentes.

Nenhum commit, push, workflow_dispatch ou deploy foi feito nesta preparacao.
