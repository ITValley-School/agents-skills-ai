# CLAUDE.md

Repositorio com agentes IT Valley em pastas individuais.

## Estrutura
- `agents/<agente>/CLAUDE.md`: prompt completo por agente

## Ordem da esteira (core)
- `01-prd-analyst`
- `02-analista-de-tela`
- `03-arquiteto-it-valley-backend`
- `04-arquiteto-it-valley-frontend`
- `05-arquiteto-designer`
- `06-dev-mockado`
- `07-arquiteto-sql-plus-mongodb`
- `08-p-o-product-owner`
- `09-dev-frontend`
- `10-dev-backend`
- `11-qa-unitario`
- `12-qa-integracao`
- `13-qa-tela`
- `14-playwright-e2e`
- `15-guardiao-de-arquitetura`

## Opcionais
- `opc-a-ui-ux-opcional`
- `opc-b-mensageria-opcional`
- `opc-c-engenheiro-de-dados-plus-bi-opcional`

## Estrutura de projeto IT Valley
- Todo projeto tem duas pastas na raiz: `backend/` e `frontend/`
- Backend: tudo na raiz de `backend/` (main.py, routers/, services/, models/, etc.) — SEM pasta `app/` intermediaria
- Frontend: tudo na raiz de `frontend/` (src/, package.json, etc.)
- Isso permite rodar `uvicorn main:app --reload` direto de dentro de `backend/`

## Regras fundamentais IT Valley
1. Service nunca conhece campos do DTO - so metodos publicos (isValid, toPayload)
2. UI e a fabrica de DTOs - ela cria, ela conhece os campos
3. Mock antes de backend - cliente valida o fluxo antes do codigo real
4. VITE_USE_MOCK - flag obrigatoria em todo projeto
5. tenant_id em TODA tabela e TODA query - sem excecao
6. BI nunca em banco relacional - sempre Gold layer do datalake
7. Arquiteto SQL so age apos DTOs completos - banco segue os schemas
8. Playwright so apos QA 11+12+13 - nao pular etapas
9. Dev Front e Dev Back sao times separados - coordenados pelo P.O.
10. Duvidas em aberto sao valiosas - nunca resolver com suposicoes

## Arquitetura Frontend — Clean Code IT Valley

### Estrutura de pastas
```
src/lib/
├── components/
│   ├── ui/           # Genericos reutilizaveis
│   └── [dominio]/    # Componentes por dominio de negocio
├── dtos/             # Classes com readonly, constructor, isValid, toPayload
├── services/         # Metodos static, chama Repository
├── repositories/     # Mock vs real via VITE_USE_MOCK
├── mocks/            # Dados falsos realistas
└── utils/            # Helpers puros (so se usados em 2+ lugares)
```

### Regras
- Componentes organizados por dominio, NAO por tipo tecnico
- NAO usar: atoms/molecules/organisms, barrel exports, pasta por componente
- DTOs imutaveis com readonly + constructor(Record) + isValid() + toPayload()
- Services com metodos static — NUNCA acessam campos do DTO
- Repositories alternam mock/real via VITE_USE_MOCK
- Design tokens centralizados no app.css (cores via @theme, espacamentos via classes CSS)
- Import direto do arquivo, sem barrel exports

### Fluxo de dados
```
+page.svelte → Service.metodo() → Repository.metodo() → new DTO(data)
                                                        ↓
             <Componente dto={dto}> ← $derived(...)
```
