# Plan: Reformulação da Interface de Cupons Administrativa

Melhorar a experiência de criação e edição de cupons no painel administrativo, tornando a janela modal mais compacta, rolável e visualmente organizada.

## Alterações Propostas

### UI/UX da Janela de Cupom
- **Tamanho e Rolagem**: Ajustar a altura máxima da modal para `max-h-[90vh]` e adicionar `overflow-y-auto` ao conteúdo do formulário para garantir que todos os campos sejam acessíveis em qualquer tela.
- **Organização Visual**:
  - Agrupar campos relacionados (ex: Valores e Mínimos em colunas).
  - Melhorar o destaque visual das seções de restrição (Produtos e Categorias).
  - Usar ícones e descrições mais claras para auxiliar o preenchimento.
- **Refinamento Estético**:
  - Manter o padrão visual "Premium" com bordas suaves e sombras sutis.
  - Ajustar o espaçamento interno (padding) para um layout mais arejado.

## Detalhes Técnicos
- **Arquivo**: `src/routes/_authenticated/admin.cupons.tsx`
- **Estilo**: Tailwind CSS para layout flexível e responsivo.
- **Componentes**: Reaproveitar os inputs padrão do sistema, garantindo consistência com o restante do painel.

