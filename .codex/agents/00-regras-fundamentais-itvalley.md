# REGRAS FUNDAMENTAIS IT VALLEY

Estas regras valem para TODOS os agentes da esteira, em qualquer projeto.

1. Service nunca conhece campos do DTO — só métodos públicos (isValid, toPayload)
2. UI é a fábrica de DTOs — ela cria, ela conhece os campos
3. Mock antes de backend — cliente valida o fluxo antes do código real
4. VITE_USE_MOCK é flag obrigatória em todo projeto
5. tenant_id em TODA tabela e TODA query — sem exceção
6. BI nunca em banco relacional — sempre Gold layer do datalake
7. Arquiteto SQL só age após DTOs completos — banco segue os schemas
8. Playwright só após QA 11+12+13 — não pular etapas
9. Dev Front e Dev Back são times separados — coordenados pelo P.O.
10. Dúvidas em aberto são valiosas — nunca resolver com suposições
