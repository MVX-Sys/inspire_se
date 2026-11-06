# Plano: Gráficos Analíticos na Auditoria

Adicionar uma camada de visualização de dados à aba de auditoria, permitindo que administradores visualizem a distribuição de atividades por categoria de produto e por usuário específico através de gráficos interativos.

## Alterações

### Frontend

- **src/routes/_authenticated/admin.auditoria.tsx**
    - Importar componentes do `recharts` (`PieChart`, `BarChart`, `Cell`, `Tooltip`, etc.).
    - Adicionar estado `viewMode` ("lista" ou "graficos").
    - Implementar lógica de processamento de dados para:
        - Distribuição de ações por **Entidade** (ex: quantos produtos foram criados vs. pedidos atualizados).
        - Atividade por **Usuário** (ranking de admins mais ativos).
        - Distribuição por **Categoria** (requer join ou mapeamento dos logs de produtos para suas categorias).
    - Criar interface de alternância entre a lista de logs e o painel de gráficos.
    - Adicionar filtros específicos para os gráficos.

## Detalhes Técnicos

- Utilizar as cores da marca (`#FF5500`, `#001F3F`) nos gráficos.
- Garantir que os filtros de busca e data aplicados na lista também reflitam nos gráficos.
- Otimizar o processamento dos `logs` em memória usando `useMemo` para evitar re-cálculos pesados.
- Para a análise por categoria, extrairemos o `produto_id` dos `detalhes` do log quando a entidade for "produtos" e cruzaremos com os dados de produtos em cache, se disponíveis, ou mostraremos a distribuição baseada nas entidades raiz.

## User Review Required

> [!IMPORTANT]
> A análise por "categoria de produtos" nos logs depende de o log conter o ID do produto e de termos uma relação clara. Se um log for genérico (ex: "login"), ele não terá categoria. Os gráficos mostrarão apenas dados relevantes para cada contexto.
