# Auditoria Técnica e Correções

Este plano detalha a auditoria profunda realizada no projeto, identificando pontos críticos de segurança, lógica, UX e integridade de dados, com as respectivas correções propostas.

## Problemas Identificados

### 1. Segurança e Integridade (Crítico)
- **Status do Pedido vs. Estoque**: Atualmente, qualquer alteração de status para "confirmado" baixa o estoque. Se o status for alterado múltiplas vezes, o estoque será baixado repetidamente.
- **Validação de Estoque no Checkout**: O checkout não valida se os itens no carrinho ainda estão disponíveis no estoque no momento da finalização.
- **Acesso Direto via RPC**: Funções críticas como `has_role` são chamadas via RPC diretamente do frontend em alguns pontos, o que pode ser manipulado se não houver RLS estrito.

### 2. Lógica de Negócio e Backend
- **Sincronização de Sessão**: O `AuthProvider` e o middleware de rota (`_authenticated`) usam métodos diferentes para verificar a sessão (`getSession` vs `getUser`), o que pode causar inconsistências em tokens expirados.
- **Tratamento de Erros de Upload**: Falhas silenciosas ou genéricas ao falhar o upload de imagens de atendentes ou produtos.
- **Falta de Transação**: A criação de pedidos e itens de pedidos não é atômica no nível do banco de dados (pode criar o pedido e falhar nos itens).

### 3. Frontend e UX
- **Performance de Imagens**: Falta de priorização de carregamento (LCP) na Hero e carregamento lento em listas grandes de produtos (falta de lazy loading nativo ou placeholders melhores).
- **Z-Index e Modais**: Possíveis sobreposições entre Modais de QR Code, Filtros Mobile e Cart Drawer.
- **Feedback de Validação**: Inputs de preço no admin permitem valores negativos no nível de componente, embora bloqueados no envio.
- **Consistência de Tipagem**: Uso excessivo de `any` em retornos de funções de pedidos e usuários.

### 4. Responsividade
- **Tabelas de Admin**: As tabelas de Pedidos e Auditoria quebram o layout em telas muito pequenas (necessário scroll horizontal melhorado ou cards mobile).

## Ações de Correção

### Backend (Supabase & Funções)
- Refatorar `updatePedidoStatus` para garantir que o estoque só seja baixado **uma vez** (verificar status anterior).
- Adicionar verificação de estoque no `createOrder` antes de persistir no banco.
- Unificar verificação de sessão no `AuthProvider`.

### Frontend (Componentes & Rotas)
- Melhorar responsividade das tabelas do painel administrativo.
- Implementar tratamento de erro global mais robusto para chamadas de `serverFn`.
- Adicionar validações em tempo real nos campos de preço e estoque.
- Otimizar carregamento de imagens na Home e Galeria.

### Segurança
- Revisar as políticas de RLS nas tabelas `user_roles` e `atendentes`.

## Verificação Final
- Teste de fluxo completo: Compra -> Dashboard Admin -> Baixa de Estoque -> Relatório de Vendas.
- Auditoria de logs após as alterações.
