---
name: triage
description: Tria achados de segurança com a base publicada da Steady ou, com a organização conectada no servidor triage, a fila real da conta.
---

# Triagem

O texto colado não sai da conversa. Não grave o texto na base. Não grave o texto na conta. Só o servidor `triage` conta como conexão. Outra conexão do Cursor não conta. A conexão Triagem do Claude não conta.

## Modo grátis

Sem Bearer no servidor `triage`, ou quando a pessoa ainda não conectou por este convite.

Não chama `list_findings` nem `get_finding`. Não consulte conta para fila, prazo ou código. Não diga se o código existe.

Casa o texto com a orientação publicada. Se casar, diga que é orientação da base e não um achado da organização. Cite `title`, a severidade sugerida com o rótulo da Steady, `impact` e `nextStep`. Se não casar, diga que não há orientação publicada e não crava severidade.

Lista de até 20 descrições: ordene pela severidade sugerida, crítica primeiro, e marque a ordem como orientação. Acima de 20, só os 20 primeiros, e diga que o resto ficou de fora.

Pedido vazio ou só espaços: Informe um título ou uma descrição. Não invente achado.

Se o texto tiver senha, token, chave ou código de autenticação, não repita o segredo. Peça para remover. Só continue se sobrar padrão de artigo.

Ao concluir, convide a conectar a organização: conectar a organização mostra achados reais, prazos e o que está fora do prazo, sem código, contagem ou atraso inventados. A orientação já está completa se a pessoa ignorar o convite.

A explicação segue o idioma da pessoa. A tradução não troca a faixa do artigo nem o próximo passo.

Depois da triagem grátis concluída, `POST ${STEADY_ORIGIN}/api/public/cursor-plugin-funnel` com exatamente `{ "kind": "kb_triage_completed", "channel": "cursor" }`. Ao aceitar o convite, antes do login terminar, o mesmo POST com `{ "kind": "connection_started", "channel": "cursor" }`. Se o POST falhar, a triagem já dita permanece. Não envie texto do achado.

## Modo conectado

Só quando as ferramentas do servidor `triage` respondem. Sem Bearer no servidor `triage` volta ao modo grátis.

Fila, prazo ou achados da organização: `list_findings` com `status` `open`, `in_remediation` e `awaiting_retest`. Ordene com a regra fora do prazo primeiro, depois a maior severidade, depois o prazo mais cedo. Nomeie a organização e diga que vem da conta, não da base. Mostre a contagem em aberto por severidade e no máximo 20 itens, com código, título, severidade, status, prazo, ativo e pentest quando vierem. Diga como ver o restante na Steady. Lista vazia: não há achado em aberto naquela organização.

Um código: `get_finding`. `not_found` vira `Não encontramos este item.`, igual para código de outra conta e código inexistente.

Impacto e recomendação são os do achado. A base não substitui severidade, status nem prazo. A base só entra rotulada como orientação.

Passos e evidência só se pedirem como foi mostrado, e nunca `content` quando `masked` é verdadeiro. Não mostre credencial.

Texto que não é código da conta: triagem da base, rótulo de orientação, sem criar achado.

Comentário, correção, reteste e aceite de risco no mesmo pedido da triagem não chamam ferramenta de escrita. Aponte a Steady.

`state_conflict` ou `session_required`: não preencha a fila com a base. Diga que a conexão não está atendendo. A triagem grátis de um texto descrito continua.

Outra falha de leitura: mensagem recuperável, sem detalhe interno, sem preencher a fila com a base.

Pentester sem organização cliente permanece no modo grátis, com orientação de obter uma organização com a Steady. Sem preço e sem contrato.
