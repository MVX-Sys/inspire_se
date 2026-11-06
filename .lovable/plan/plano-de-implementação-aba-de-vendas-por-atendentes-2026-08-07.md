# Plano de Implementação: Aba de Vendas por Atendentes

Adição de uma nova funcionalidade no painel administrativo para acompanhar o desempenho de vendas de cada atendente, com filtros temporais (semanal, mensal, anual) e geração de relatórios.

## 1. Backend e Dados

### 1.1 Funções de Servidor (`src/lib/vendas.functions.ts`)
- `getVendasPorAtendente`: Nova função para agrupar pedidos confirmados/entregues por atendente.
- Lógica de agregação: somar `total` e contar `quantidade` de itens por atendente.
- Filtros: suporte a períodos de data (início/fim) para semana, mês e ano.

## 2. Interface Administrativa

### 2.1 Nova Rota (`src/routes/_authenticated/admin.vendas.tsx`)
- Tela de dashboard de vendas.
- **Cards de Métricas Gerais**: Total vendido no período, melhor atendente, média por pedido.
- **Tabela/Lista de Atendentes**:
  - Nome/Foto do atendente.
  - Total de pedidos realizados.
  - Valor total acumulado em vendas.
  - Ticket médio.
- **Controles de Filtro**:
  - Seletor de período: "Esta Semana", "Este Mês", "Este Ano", "Personalizado".
- **Botão de Exportação**: Integrar com o `ExportMenu` para gerar PDF/CSV do relatório de desempenho.

### 2.2 Navegação (`src/routes/_authenticated/admin.tsx`)
- Adicionar "Vendas" ao menu lateral/superior do admin.
- Ícone sugerido: `BarChart3` ou `TrendingUp`.

## 3. Relatórios (`src/lib/pdf.ts`)
- Criar `downloadRelatorioVendasPDF`: Nova função para gerar um PDF formatado com o ranking de vendas dos atendentes e resumo do período selecionado.

## Perguntas para o usuário:
1. Você gostaria de ver um gráfico de desempenho (ex: barras ou linhas) ou apenas a tabela com os números já é o suficiente?
2. O "valor vendido" deve considerar apenas pedidos com status "confirmado" e "entregue", ou deve incluir os "pendentes" também?
