## v1.3 — 2026-06-14

**Tipo de alteração:** Correção (OP-W)
**Autorizado por:** Victor Leonardo Arimatea Queiroz
**Status do workflow:** ativo
**Execuções afetadas:** nenhuma
**Skills afetadas:** nenhuma

**Exposição de motivos:** Os Pacotes 1 (abertura) e 2 (fechamento/auditoria)
da Seção 7 terminavam em "Token GitHub com acesso repo: [INSERIR TOKEN]",
incoerente com a doutrina de dois tokens já canônica no PROTOCOLO-SESSAO.md.
A abertura é read-only até a conversão para escrita, e o auditor nunca recebe
token de edição — ambos os pacotes devem pedir o token de leitura. A ambiguidade
foi exposta na largada da sessão de 2026-06-14: a ETAPA 1 pedia "token de
leitura" mas o campo do pacote pedia "acesso repo" (token de edição).

### Alterações realizadas
- `WORKFLOW.md` v1.2 -> v1.3:
  - Pacote 1: campo de token -> "Token de leitura ampla (abertura)"; nota de que
    o token de edição entra só na conversão para escrita (Modo 1)
  - Pacote 2: campo de token -> "Token de leitura (o auditor nunca recebe token
    de edição)"
  - Seção 9 (Histórico): linha v1.3 registrada
  - Cabeçalho e Seção 1 (tabela): v1.2 -> v1.3

---

## v1.2 — 2026-06-12 (correção pós-auditoria)

**Tipo de alteração:** Correção (OP-E) — mesma versão v1.2
**Autorizado por:** Victor Leonardo Arimatea Queiroz
**Detectado por:** auditoria W05 independente de 2026-06-12 (SEV2-D1)

**Exposição de motivos:** A auditoria W05 de fechamento identificou que o
`README.md` deste repositório ainda declarava v1.1 — 2026-06-06, enquanto o
WORKFLOW.md e o sumario.md já registravam v1.2. Divergência de propagação não
capturada na operação principal v1.2 (o plano não incluiu o README).

### Alterações realizadas
- `README.md`: campo Versão v1.1 → v1.2 (alinhamento ao WORKFLOW.md e sumario)

---

## v1.2 — 2026-06-12

**Tipo de alteração:** Melhoria (OP-W)
**Autorizado por:** Victor Leonardo Arimatea Queiroz
**Status do workflow:** ativo
**Execuções afetadas:** nenhuma
**Skills afetadas:** nenhuma

**Exposição de motivos:** A Etapa 2 (abertura) ainda dependia de o mantenedor
colar manualmente o relatório de handoff a cada abertura de sessão — fonte de
atrito diário. Com o handoff agora versionado dentro do relatório de sessão
(W03 v1.3, Bloco III), a abertura pode localizá-lo e lê-lo sozinha via API.
Atende à meta 2 do bloco "Formalização do ciclo de sessão" do ROADMAP.

Decisão de design do mantenedor (2026-06-12): o processo manual de colagem
está aposentado. Se a leitura automática do handoff falhar, o caminho correto
não é reverter à colagem manual, mas acionar uma auditoria W05 nova do zero —
o ecossistema real é fonte de verdade mais confiável que um handoff corrompido,
e gerar uma auditoria nova custa menos que reintroduzir o atrito diário.

### Alterações realizadas
- `WORKFLOW.md` v1.1 → v1.2:
  - Etapa 2 reescrita ("Localização e absorção do Handoff — leitura automática"):
    agente lista `hub-memoria/documentos`, identifica o `SESSAO-*` mais recente,
    lê a seção final (Bloco de Handoff) via API. Responsável passa de
    Humano+Agente para Agente
  - Adicionada **regra de fallback**: falha na extração → recomendar auditoria
    W05 nova do zero, nunca solicitar cola manual
  - Tratamento de primeira sessão (sem handoff anterior) explicitado
  - Seção 1 (tabela de Identificação): campo Versão v1.1 → v1.2
  - Pacote 1 (Seção 7): ETAPA 2 do bloco de mensagem atualizada — sem o
    marcador "[COLAR AQUI O RELATÓRIO W05...]", agora com instrução de
    localização automática
  - Seção 9 (Histórico): linhas v1.1 e v1.2 registradas

---

## v1.1 — 2026-06-10 (correção interna)

**Tipo de alteração:** Correção (OP-W + OP-E)
**Autorizado por:** victorarimatea
**Status do workflow:** ativo
**Exposição de motivos:** Correção de divergências SEV2-04 e SEV4-03
identificadas pelo W05 em auditoria de 2026-06-09.

**O que mudou:**
- Seção 1 (tabela de Identificação): campo `Versão` corrigido de `v1.0`
  para `v1.1` — divergência interna no documento de especificação (SEV2-04)
- Seção de Encerramento, linhas operacionais: termo "perguntas ordenadoras"
  substituído por "perguntas orientadoras" (SEV4-03 / I2 do ROADMAP)
  — apenas texto operacional; registros históricos no backlog preservados

**Nota:** A versão do workflow permanece v1.1 — estas correções resolvem
inconsistências internas sem introduzir conteúdo novo.

---

# backlog-versoes.md — wkf-sessao-agente (W06)

**Repositório:** wkf-sessao-agente
**Mantenedor:** victorarimatea

---

## v1.1 — 2026-06-06

**Tipo de alteração:** Melhoria
**Autorizado por:** victorarimatea
**Status do workflow:** ativo
**Exposição de motivos:** Ajuste do critério de convergência da auditoria W05
na Etapa 7, baseado em aprendizado empírico da sessão de 2026-06-06. O critério
anterior ("dois acionamentos consecutivos com zero divergências") mostrou-se
inalcançável na prática: cada auditoria detalhada — especialmente com modelos
de maior capacidade (Opus 4.8) — encontrava novas divergências de polimento
que as anteriores não viam. A perfeição estrutural absoluta revelou-se uma
assíntota, não um destino.

**Novo critério:** encerramento com ZERO divergências SEV1 e SEV2 em um único
acionamento. SEV3 e SEV4 são aceitas como estado tolerado e levadas via Handoff
para correção planejada. Teto de três iterações mantido como salvaguarda.

**Fundamento:** o que importa para a saúde do ecossistema é integridade
operacional (zero problemas que afetam dados ou execução), não perfeição
estrutural (polimento, indexação, metadados). O novo critério evita ciclos
de correção infinitos, onde cada correção introduz risco de novos erros.

### Alterações realizadas
- Etapa 7 do `WORKFLOW.md`: critério de convergência redefinido
- Adicionada seção "Critério de qualidade — integridade operacional"
- Adicionado fundamento empírico da sessão de 2026-06-06

---

## v1.0 — 2026-06-06

**Tipo de alteração:** Criação
**Autorizado por:** victorarimatea
**Status do workflow:** ativo
**Exposição de motivos:** Criação do W06 — Protocolo de Sessão Assistida por
Agente. Nasce da maturação do processo de trabalho ao longo das sessões de
construção do ecossistema DTD/SETIS. O workflow formaliza o ritual de abertura,
trabalho e fechamento de sessões colaborativas entre humano e agente de IA,
incorporando separação executor/auditor, protocolo de Handoff entre sessões,
perguntas ordenadoras e staging como camada de governança universal.

Decisões de design registradas:
- W06 é processo pai; W03 e W05 são subprocessos filhos
- Fases: Abertura → Trabalho → Fechamento
- Ordem no fechamento: W03 (missão) antes de W05 (conformidade)
- W05 sempre em chat avulso fora do projeto, modelo de maior performance
- Condição de convergência: dois W05 consecutivos zerados, ou máximo três iterações
- Staging como passagem obrigatória para todas as ideias, sem exceção
- Handoff formalizado como passagem de bastão entre sessões

### Arquivos criados
- `README.md` v1.0
- `WORKFLOW.md` v1.0
- `INDICE.md` v1.0
- `backlog-versoes.md` v1.0
- `execucoes/.gitkeep`

---
