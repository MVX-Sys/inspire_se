# Plano de Adição de Subtítulo na Configuração da Tela Inicial

Este plano descreve as alterações necessárias para adicionar um campo de subtítulo à configuração da seção Hero da página inicial, permitindo maior personalização do conteúdo exibido aos clientes.

## Alterações

### Banco de Dados (Supabase)

- Adicionar uma coluna `hero_subtitle` (tipo `text`) à tabela `public.site_config`.
- Atualizar as permissões (RLS) se necessário (já devem permitir acesso à tabela).

### Configurações do Site (`src/lib/config-site.ts`)

- Atualizar o tipo `SiteConfig` para incluir `hero_subtitle: string | null`.
- Atualizar a função `getSiteConfig` para retornar um valor padrão para `hero_subtitle` caso a busca falhe.

### Interface Administrativa (`src/routes/_authenticated/admin.index.tsx`)

- No componente de configuração (`ConfigDialog` ou similar dentro de `AdminProductsList`):
    - Adicionar um campo de entrada (input) para o subtítulo do Hero.
    - Atualizar o estado e a lógica de salvamento para incluir `hero_subtitle`.

### Página Inicial (`src/routes/index.tsx`)

- Atualizar o componente `HeroSection` para exibir o subtítulo abaixo do título principal, caso esteja configurado.
- Aplicar estilos consistentes com a identidade visual da marca (fonte Poppins, cores vibrantes).

## Detalhes Técnicos

- O subtítulo será opcional.
- Manter a compatibilidade com configurações existentes (valor nulo por padrão).

## Verificação

- Acessar o painel administrativo -> Configurações da Tela Inicial.
- Inserir um texto no novo campo de subtítulo e salvar.
- Verificar na página inicial se o subtítulo aparece corretamente sob o título do Hero.
