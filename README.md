# ARSENAL A23 — aprovacao humana protegida (somente metadados)

Este repositório **público** contém exclusivamente este README e um workflow de aprovação. **NÃO inclui** o código-fonte ou dados do ARSENAL, Cotton Flow, Central de Demandas, tokens ou credenciais.

## Fluxo em duas etapas

1. Um operador solicita `workflow_dispatch` com o SHA do candidato local e o digest de suas 22 evidências.
2. Job `a23-request-approval`, sem segredos privados, usa o token **efêmero** do próprio GitHub Actions (`GITHUB_TOKEN`, `actions:write`) para solicitar um **novo** `workflow_dispatch` com `phase=approval`.
3. O novo job `a23-human-approval` fica **bloqueado** pelo environment `arsenal-a23-local-approval`. O revisor designado verifica SHA/digest e escolhe *Approve* ou *Reject* no GitHub.
4. O job aprovado valida somente o SHA/digest e termina. Não executa checkout, build, deploy, push, download de código privado ou qualquer alteração de dados.
5. O verificador local consulta a API **pública e anônima** do GitHub para comprovar a aprovação do job certo, do workflow imutável e do SHA/digest exatos.

## Ambiente protegido (configurado pelo proprietário)

- Required reviewer: `MMarcosLobo`.
- Prevent self-review: `true`.
- Allow administrators bypass: `false`.
- Job alvo: `a23-human-approval` (somente na segunda execução).
- O workflow deve permanecer fixado a commit e hash de blob explícitos no verificador.

## Limitações de segurança

O operador humano não pode ter suas credenciais de aprovação acessíveis ao processo automatizado que avalia o gate. Uma conta GitHub pessoal autenticada na CLI do Windows pode ter autorização para aprovar pela API; nesse caso, a evidência externa não prova necessariamente um *clique humano*. Separar a credencial de aprovação da automação e auditar alterações de configuração do repositório são controles obrigatórios antes de declarar release resistente a agentes locais maliciosos.

Este repositório serve somente para a autorização **LOCAL** do ARSENAL A23. Nenhuma aprovação deste workflow concede permissão de deploy, merge, push em produção ou modificação do Cotton.
