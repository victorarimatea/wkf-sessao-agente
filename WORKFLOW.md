# WORKFLOW.md — Protocolo de Sessão Assistida por Agente

**Versão:** v1.2 — 2026-06-12
**Status:** ativo
**Responsável:** Victor Leonardo Arimatea Queiroz — Diretor de Transformação Digital
**Repositório:** wkf-sessao-agente (W06)

---

## Seção 1 — Identificação

| Campo | Valor |
|---|---|
| Nome do processo | Protocolo de Sessão Assistida por Agente |
| ID | W06 |
| Versão | v1.2 |
| Status | ativo |
| Data de criação | 2026-06-06 |
| Responsável | DTD/SETIS/SES-DF |
| Skill associada | S04 (skl-github-orquestracao) — acionada na fase de Trabalho |
| Subprocessos filhos | W03 (registro de sessão), W05 (auditoria de consistência) |
| Repositório âncora | hub-fonte (CONTEXTO.md + sumario.md como leitura obrigatória) |
| Frequência | Toda sessão de trabalho assistida por agente de IA |

---

## Seção 2 — Missão e contexto organizacional

### Missão

Padronizar o processo de trabalho envolvendo uma sessão de construção,
revisão ou atualização de repositórios e documentações do GitHub relacionadas
ao ecossistema DTD/SETIS, com etapas e instruções claras descritas para
garantir os princípios ordenadores de rastreabilidade, conformidade e auditoria
— do início ao encerramento formal de cada sessão.

### Por que este processo existe

O ecossistema DTD/SETIS é construído em sessões de trabalho intensivo entre o
Diretor de Transformação Digital e um agente de IA (Claude). Cada sessão pode
durar horas, gerar decisões de design, implementar mudanças em múltiplos
repositórios e produzir aprendizados que precisam ser preservados.

Sem um processo de trabalho padronizado, três riscos se materializam repetidamente:

- **Risco de contexto:** o agente começa a sessão com informações desatualizadas
  ou em cache, operando sobre uma visão incorreta do ecossistema
- **Risco de propagação:** erros introduzidos durante a sessão não são detectados
  e se propagam para sessões seguintes, acumulando drift
- **Risco de perda:** decisões, aprendizados e ideias emergentes se perdem
  porque não foram registrados no momento certo

O W06 é a resposta estrutural a esses três riscos.

### Princípios de design

**1. Separação executor/auditor**
O agente que executa a missão não audita o próprio trabalho. A auditoria W05
é sempre executada em chat separado, com contexto limpo, por uma instância
independente do agente.

**2. Fechamento antes de encerrar**
Uma sessão só está encerrada quando o W05 retornar zero divergências em dois
acionamentos consecutivos (ou após três iterações — o que vier primeiro).
Erros detectados são corrigidos na mesma sessão, com o contexto ainda fresco.

**3. Passagem de bastão (Handoff)**
O relatório W05 zerado do fechamento de uma sessão é a pré-condição de abertura
da sessão seguinte. Cada sessão começa sabendo exatamente o estado que herdou.
Este é o protocolo de handoff entre sessões — conceito fundamental em sistemas
distribuídos e orquestração de agentes, e em amadurecimento dentro do ecossistema
DTD/SETIS como padrão para articulação entre múltiplos workflows e chats isolados.

**4. W03 registra a missão, não o processo de conformidade**
O relatório de sessão (W03) captura o que foi construído, decidido e aprendido.
Iterações de auditoria, erros de conformidade detectados e corrigidos, e ajustes
técnicos de consistência não entram no relatório — são domínio do W05.

**5. Leitura crítica sempre via API GitHub**
Arquivos de instrução críticos (S04, CONTEXTO.md, sumario.md) são sempre lidos
via API GitHub, nunca via raw.githubusercontent.com, que possui cache de CDN
com delay indeterminado.

**6. Variabilidade é característica, não falha**
Dois acionamentos consecutivos do W05 podem retornar resultados diferentes.
Isso é esperado dado o ambiente de trabalho (LLM com atenção distribuída,
cache residual, complexidade crescente do ecossistema). O processo absorve
essa variabilidade com iterações múltiplas e critério de convergência.

**7. Staging como camada de governança universal**
Toda ideia emergente — independentemente do destino final (ROADMAP, hub-aprendizagem,
nova skill, novo workflow) — passa obrigatoriamente pela staging area do hub-entrada
antes de ser registrada como fato operacional. A staging preserva o controle do
mantenedor sobre o que entra no ecossistema. Não há atalho que bypasse essa camada.

---

## Seção 3 — Visão geral do processo de trabalho

```
ABERTURA
  └── Etapa 1: Leitura do contexto via API
  └── Etapa 2: Ingestão do relatório W05 herdado (Handoff)
  └── Etapa 3: Declaração da missão do dia

TRABALHO
  └── Etapa 4: Execução da missão (S04 + workflows específicos)

FECHAMENTO
  └── Etapa 5: Registro via W03
  └── Etapa 6: Mineração de ideias para staging
  └── Etapa 7: Auditoria W05 em chat separado (iterações até convergência)
  └── Etapa 8: Encerramento formal + Handoff para próxima sessão
```

---

## Seção 4 — FASE 1: ABERTURA

### Etapa 1 — Leitura do contexto via API

**Responsável:** Agente (Claude)
**Quando:** Primeiro ato de toda sessão, antes de qualquer resposta substantiva

**Instrução ao agente:**
Ler os três arquivos abaixo via API GitHub. Decodificar o campo `content`
de base64 em cada resposta. Nunca usar raw.githubusercontent.com para
esses arquivos — risco de cache de CDN com delay indeterminado.

```
GET https://api.github.com/repos/victorarimatea/hub-fonte/contents/CONTEXTO.md
GET https://api.github.com/repos/victorarimatea/hub-fonte/contents/sumario.md
GET https://api.github.com/repos/victorarimatea/skl-github-orquestracao/contents/SKILL.md
```

**Critério de conclusão:** os três arquivos lidos, versões confirmadas,
estado do ecossistema absorvido e declarado ao mantenedor.

---

### Etapa 2 — Localização e absorção do Handoff (leitura automática)

**Responsável:** Agente (Claude)
**Quando:** Após a leitura do contexto, antes da missão

**Mudança estrutural (v1.2):** o handoff deixou de ser um objeto solto carregado
pelo mantenedor no bloco de notas. Ele agora vive versionado no GitHub, dentro
do relatório de sessão (W03, Bloco III, seção final), em local conhecido. A
abertura da sessão **localiza e lê o handoff sozinha** — o mantenedor não cola
mais nada. Esta mudança elimina o atrito diário que o processo manual gerava.

**Instrução ao agente:**
Localizar e absorver o Bloco de Handoff da sessão anterior, sem depender de
cola manual:

1. Listar o conteúdo de `hub-memoria/documentos` via API GitHub:
   ```
   GET https://api.github.com/repos/victorarimatea/hub-memoria/contents/documentos
   ```
2. Identificar o arquivo `SESSAO-AAAA-MM-DD-*.md` com a **data mais recente**
   no nome.
3. Ler esse arquivo via API e absorver sua seção final — o **Bloco de Handoff**
   (Bloco III da estrutura do W03).
4. Responder, antes de prosseguir:
   - Há divergências SEV1 ou SEV2 abertas não corrigidas no handoff?
   - Se sim: declarar explicitamente e aguardar decisão do mantenedor antes
     de receber a missão. Não iniciar trabalho sobre ecossistema com divergências
     críticas abertas sem decisão explícita.
   - Se não: confirmar o estado herdado e declarar pronto para a missão.

**Regra de fallback (decisão do mantenedor, 2026-06-12):**
Se a extração do Bloco de Handoff falhar — arquivo não encontrado, seção
ausente, conteúdo ilegível — o agente **NÃO** solicita cola manual nem reverte
ao processo anterior. Em vez disso, sinaliza ao mantenedor e recomenda acionar
uma **auditoria W05 nova do zero** (chat separado, sem token) para reconstruir
o estado herdado a partir do ecossistema real — fonte de verdade mais confiável
que um handoff corrompido. O processo manual de colagem está aposentado: é
preferível gerar uma auditoria nova a reintroduzir o atrito que ele causava.

**Primeira sessão (sem handoff anterior):** se não houver nenhum relatório
`SESSAO-*` no hub-memoria, declarar *"Primeira sessão sob este protocolo — W05
será executado no fechamento desta sessão"* e prosseguir.

**Critério de conclusão:** handoff localizado e lido pelo agente, estado herdado
confirmado, divergências críticas declaradas (se houver), mantenedor ciente do
ponto de partida.

---

### Etapa 3 — Declaração da missão do dia

**Responsável:** Humano (Victor)
**Quando:** Após confirmação do estado herdado

**Instrução ao humano:**
Declarar a missão do dia em linguagem livre. Não há formato obrigatório.
A declaração abre formalmente a fase de Trabalho.

**Instrução ao agente:**
Antes de iniciar qualquer operação, responder às seguintes perguntas:
- Qual é o objetivo central da missão declarada?
- A missão envolve operações no GitHub? Se sim, o token foi fornecido?
- Há dependências com outros workflows ou repositórios que precisam ser
  lidos antes de começar?

Aguardar confirmação do mantenedor antes de prosseguir.

**Critério de conclusão:** missão compreendida, confirmada pelo mantenedor,
dependências mapeadas.

---

## Seção 5 — FASE 2: TRABALHO

### Etapa 4 — Execução da missão

**Responsável:** Agente (Claude) + Humano (Victor)
**Quando:** Após declaração e confirmação da missão

**Instrução ao agente:**
Executar a missão seguindo obrigatoriamente a S04 para qualquer operação
nos repositórios GitHub. O W06 não governa o conteúdo do trabalho —
apenas define que esta fase existe e quando ela termina.

Regras operacionais inegociáveis herdadas da S04:
- Nenhuma alteração executada sem aprovação explícita do plano pelo mantenedor
- Toda operação gera registros em backlogs e changelogs
- Texto operacional divergente = corrigir; texto histórico = preservar

**Critério de conclusão:** o mantenedor declara "missão concluída" ou equivalente.
A declaração abre formalmente a fase de Fechamento.

---

## Seção 6 — FASE 3: FECHAMENTO

### Etapa 5 — Registro via W03

**Responsável:** Agente (Claude)
**Quando:** Imediatamente após a declaração de missão concluída

**Instrução ao agente:**
Acionar o W03 (wkf-registro-sessao) para produzir o relatório de sessão.

Antes de redigir, responder às seguintes perguntas:
- Qual foi a missão central desta sessão?
- Quais decisões de design foram tomadas e qual foi a justificativa de cada uma?
- Quais implementações foram realizadas (repositórios e arquivos afetados)?
- Quais aprendizados emergiram que têm valor para sessões futuras?
- Há itens iniciados mas não concluídos que precisam ser declarados como pendentes?

O relatório registra apenas o que responde a essas perguntas.

**O que NÃO entra no relatório W03:**
- Iterações de auditoria W05
- Erros de conformidade detectados e corrigidos durante a sessão
- Ajustes técnicos de consistência (nomes, versões, campos)
- Número de rodadas de auditoria necessárias

**Critério de conclusão:** relatório W03 depositado no hub-memoria,
EXECUCOES.md atualizado, ROADMAP atualizado.

---

### Etapa 6 — Mineração de ideias para staging

**Responsável:** Agente (Claude) + Humano (Victor)
**Quando:** Após o W03, antes do W05

**Instrução ao agente:**
Revisar a sessão respondendo às seguintes perguntas orientadoras.
Cada resposta positiva gera uma proposta de registro submetida à aprovação
do mantenedor antes de qualquer escrita.

**Perguntas orientadoras:**

*Para a staging Seção C (ideias brutas):*
- Surgiu um conceito, instrumento ou estrutura que ainda não existe no
  ecossistema mas que poderia vir a existir?
- Alguma limitação identificada hoje sugere uma solução que ainda não
  foi desenhada?

*Para a staging Seção B (decisões pendentes):*
- Há uma questão de design que precisava ser decidida mas foi adiada?
- Alguma ambiguidade estrutural ficou em aberto e precisa de decisão
  explícita do mantenedor?

*Para o hub-aprendizagem D03 (boas práticas):*
- Algo que aprendemos hoje mudaria como faríamos esse mesmo tipo de
  operação no futuro?
- Identificamos uma causa raiz recorrente que merece ser documentada
  como lição aprendida?

**Estado final desejado desta etapa:**
Cada pergunta respondida com sim ou não. Cada "sim" com proposta de
registro específica apresentada ao mantenedor. Cada proposta aprovada
ou rejeitada explicitamente. Nenhuma ideia registrada sem aprovação.
Todas as ideias aprovadas encaminhadas para a staging area — inclusive
as destinadas ao hub-aprendizagem, que passam pela staging antes de
migrar para D03.

**Critério de conclusão:** todas as perguntas respondidas, staging.md
atualizado com ideias aprovadas. Se nenhuma ideia elegível for identificada,
registrar explicitamente e prosseguir.

---

### Etapa 7 — Auditoria W05 em chat separado

**Responsável:** Humano (Victor)
**Quando:** Após mineração de ideias

**Instrução ao humano:**

1. Abrir uma **nova conversa avulsa** fora do projeto Claude atual
   (contexto limpo, sem influência das instruções de projeto)

2. Selecionar manualmente o **modelo de maior performance disponível**
   na interface antes de enviar a mensagem

3. Colar o **Pacote 2 — Fechamento** com o token GitHub

4. Aguardar o relatório W05

5. Copiar o relatório e colar na sessão principal (este chat)

6. Se o relatório contiver divergências:
   - Corrigir com a S04 nesta sessão
   - Repetir os passos 1–5

7. Continuar iterando até uma das condições de convergência:
   - **Condição A (encerramento):** um acionamento W05 retorna ZERO
     divergências SEV1 e SEV2. Divergências SEV3 e SEV4 remanescentes são
     aceitas como estado tolerado, registradas e levadas via Handoff para
     correção planejada em sessão futura.
   - **Condição B (limite):** três iterações completadas — registrar
     divergências residuais (de qualquer severidade) e aceitar como estado
     tolerado com decisão explícita do mantenedor.

8. Registrar o número de iterações realizadas e as divergências SEV3/SEV4
   toleradas (se houver) para Handoff.

**Critério de qualidade — integridade operacional, não perfeição estrutural:**
O critério de encerramento é a ausência de divergências SEV1 (crítico) e SEV2
(alto) — ou seja, zero problemas que afetam dados operacionais ou integridade
do ecossistema. Divergências SEV3 (estrutura interna sem impacto imediato) e
SEV4 (polimento, indexação, metadados) NÃO bloqueiam o encerramento.

**Fundamento empírico (sessão de 2026-06-06):** observou-se que cada auditoria
suficientemente detalhada — especialmente com modelos de maior capacidade —
encontra novas divergências de polimento que as anteriores não detectaram.
A perfeição estrutural absoluta é uma assíntota, não um destino alcançável.
Exigir "zero divergências absoluto" levaria a ciclos de correção potencialmente
infinitos, onde cada correção introduz risco de novos erros. O critério de
integridade operacional (zero SEV1/SEV2) captura o que realmente importa para
a saúde do ecossistema, enquanto SEV3/SEV4 são tratadas de forma planejada
via Handoff.

**Nota sobre variabilidade:** é esperado que acionamentos consecutivos do W05
possam retornar resultados diferentes. Isso não é falha — é característica do
ambiente (LLM com atenção distribuída, complexidade do ecossistema, capacidade
do modelo acionado). Recomenda-se acionar o W05 com o modelo de maior
performance disponível (ver Etapa 7, passo 2).

**Critério de conclusão:** condição A ou B atingida, número de iterações
registrado, divergências SEV3/SEV4 toleradas anotadas para Handoff, relatório
W05 final em mãos.

---

### Etapa 8 — Encerramento formal (Handoff)

**Responsável:** Agente (Claude) + Humano (Victor)
**Quando:** Após convergência do W05

**Instrução ao agente:**
Produzir o sumário de encerramento respondendo às seguintes perguntas:
- Qual foi a missão executada nesta sessão?
- Quantas iterações W05 foram necessárias para convergência?
- O ecossistema encerra com zero divergências ou com divergências
  toleradas declaradas?
- Há algum item que o mantenedor precisa lembrar ao abrir a próxima sessão?

O sumário de encerramento inclui obrigatoriamente o relatório W05 final
formatado para ser salvo e colado na próxima abertura (Handoff).

**Instrução ao humano:**
- Salvar o relatório W05 final no bloco de notas — será colado na
  próxima abertura de sessão (Etapa 2)
- Revogar o token GitHub: https://github.com/settings/tokens

**Critério de conclusão:** sumário produzido, relatório W05 salvo,
token revogado. A sessão está formalmente encerrada.

---

## Seção 7 — Pacotes de mensagem

### Pacote 1 — Abertura de sessão
*(colar no início de toda sessão de trabalho, dentro do projeto Claude)*

```
Bom dia Claude. Iniciando nova sessão de trabalho no ecossistema DTD/SETIS.

Contexto: Sou Victor Leonardo Arimatea Queiroz, Diretor de Transformação
Digital da DTD/SETIS/SES-DF.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ETAPA 1 — LEITURA OBRIGATÓRIA (via API — nunca raw.githubusercontent.com)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

GET https://api.github.com/repos/victorarimatea/hub-fonte/contents/CONTEXTO.md
GET https://api.github.com/repos/victorarimatea/hub-fonte/contents/sumario.md
GET https://api.github.com/repos/victorarimatea/skl-github-orquestracao/contents/SKILL.md

Decodifique o campo `content` de base64 em cada resposta.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ETAPA 2 — HANDOFF DA SESSÃO ANTERIOR
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[COLAR AQUI O RELATÓRIO W05 DO FECHAMENTO DA SESSÃO ANTERIOR]

Leia e absorva. Há divergências SEV1 ou SEV2 abertas?
Se sim: sinalize antes de receber a missão.
Se não: confirme o estado herdado e declare pronto.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ETAPA 3 — MISSÃO DO DIA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Após confirmar o estado herdado, aguarde a missão do dia.

Regras operacionais:
• Toda operação GitHub segue obrigatoriamente a S04
• Nenhuma alteração sem aprovação explícita do plano
• Toda operação gera registros em backlogs e changelogs
• Texto operacional divergente = corrigir. Texto histórico = preservar

Token GitHub com acesso repo: [INSERIR TOKEN]
```

---

### Pacote 2 — Fechamento de sessão
*(abrir nova conversa AVULSA — fora do projeto — selecionar modelo de maior performance)*

```
Claude, execute exclusivamente o W05 — Auditoria de Consistência do
Ecossistema DTD/SETIS. Nenhuma outra ação.

Contexto: Sou Victor Leonardo Arimatea Queiroz, Diretor de Transformação
Digital da DTD/SETIS/SES-DF. Estou encerrando uma sessão de trabalho.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
INSTRUÇÃO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Leia o W05 via API GitHub:
   GET https://api.github.com/repos/victorarimatea/wkf-auditoria-consistencia/contents/WORKFLOW.md
   Decodifique o campo `content` de base64.

2. Leia a S04 via API GitHub (nunca via raw):
   GET https://api.github.com/repos/victorarimatea/skl-github-orquestracao/contents/SKILL.md

3. Execute as 5 camadas de auditoria conforme o WORKFLOW.md do W05.

4. Apresente o relatório no formato padrão (SEV1/SEV2/SEV3/SEV4).

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REGRA ABSOLUTA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Após o relatório: PARE.
Não proponha correções. Não inicie operações. Não solicite token.
Nenhuma alteração em repositório neste chat — apenas auditoria e relatório.

Token GitHub com acesso repo: [INSERIR TOKEN]
```

---

## Seção 8 — Intenção do Comandante

O estado desejado ao final de todo processo de trabalho W06 é:

> O ecossistema está em estado verificado e consistente. A missão do dia
> foi executada, registrada e auditada. O próximo mantenedor — humano ou
> agente — que abrir uma sessão encontrará contexto claro, relatório W05
> atualizado como Handoff, e zero divergências abertas não declaradas.
> Rastreabilidade, conformidade e auditoria foram preservadas do início
> ao encerramento.

Quando uma situação não coberta por este processo surgir, a decisão correta
é aquela que mais se aproxima desse estado desejado.

---

## Seção 9 — Histórico de versões

| Versão | Data | Tipo | Descrição |
|---|---|---|---|
| v1.0 | 2026-06-06 | Criação | Processo inaugural — nasce da maturação do processo de trabalho ao longo das sessões de construção do ecossistema DTD/SETIS; incorpora separação executor/auditor, protocolo de Handoff entre sessões, perguntas ordenadoras e princípio de staging como camada de governança universal |
