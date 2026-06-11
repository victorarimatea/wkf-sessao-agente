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
