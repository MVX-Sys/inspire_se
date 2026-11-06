# Plano de Ajuste no Menu Administrativo

O objetivo deste plano é simplificar a navegação na aba de produtos do painel administrativo, removendo links redundantes e mantendo apenas a aba de Configurações da Tela Inicial conforme solicitado.

## Alterações

### Painel Administrativo (`src/routes/_authenticated/admin.index.tsx`)

- Localizar a seção "Navigation Tabs" (por volta da linha 371).
- Remover os links (`Link`) para "Produtos", "Vendas" e "Usuários".
- Manter o botão de "Configurações" que abre o modal de configuração da tela inicial.
- Ajustar o layout dessa área para que o botão de configurações fique bem posicionado.

## Detalhes Técnicos

- As opções removidas já existem no menu global do painel administrativo (o componente `Select` no cabeçalho em `admin.tsx`), portanto, a remoção da redundância na sub-página de produtos melhora a clareza da interface.
- A funcionalidade de "Configurações" (que gerencia o Hero e banners da Home) será preservada como o foco principal de sub-navegação nesta tela.

## Verificação

- Acessar o painel administrativo na rota `/admin`.
- Confirmar que os links internos para Produtos, Vendas e Usuários sumiram da sub-barra de navegação.
- Confirmar que o botão "Configurações" permanece funcional e abre o modal esperado.
