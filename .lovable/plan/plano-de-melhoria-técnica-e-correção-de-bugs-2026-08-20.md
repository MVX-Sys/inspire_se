# Plano de Melhoria Técnica e Correção de Bugs

O usuário solicitou uma análise profunda da aplicação para identificar e corrigir bugs, erros e comportamentos inesperados. Embora o pedido tenha vindo como uma "edição de texto visual", o conteúdo da mensagem ("Tente procurar bugs e falhas presentes no código pra tentar corrigí-las") indica uma tarefa de auditoria e manutenção técnica.

## Análise e Auditoria

1. **Revisão de Segurança (RLS e Auth):**
   - Verificar se todas as tabelas possuem políticas RLS adequadas.
   - Garantir que o acesso ao painel administrativo (`/admin/*`) está devidamente protegido.
   - Auditar funções `SECURITY DEFINER` para evitar escalada de privilégios.

2. **Fluxo de Pedidos e Carrinho:**
   - Validar a lógica de estoque em tempo real para evitar vendas de itens esgotados.
   - Verificar se as variações de produtos (cor/tamanho) estão sendo processadas corretamente no checkout.
   - Testar o redirecionamento para o WhatsApp para garantir que os dados do pedido cheguem formatados.

3. **Performance e Experiência do Usuário:**
   - Otimizar o carregamento de imagens (que já foi alvo de melhorias, mas requer vigilância constante devido ao uso de URLs assinadas).
   - Verificar estados de carregamento (skeletons) e tratamento de erros em loaders do TanStack Router.

4. **Integridade de Dados:**
   - Validar esquemas Zod em funções de servidor (`createServerFn`).
   - Verificar se a expiração automática de promoções está funcionando conforme o esperado em todos os cenários.

## Ações Propostas

- **Correção de RLS:** Ajustar quaisquer políticas que possam estar impedindo funcionários de realizar operações básicas, se detectado.
- **Tratamento de Erros:** Melhorar a captura de erros em formulários de administração para fornecer feedbacks mais claros.
- **Limpeza de Código:** Remover possíveis "dead code" ou imports não utilizados que possam estar afetando o bundle size.

## Detalhes Técnicos

- As alterações focarão na robustez do sistema sem alterar a interface visual premium já estabelecida.
- Utilização de `createServerFn` para lógica de backend.
- Manutenção da integração com Lovable Cloud (Supabase).
