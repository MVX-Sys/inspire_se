# Plano: Múltiplas Áreas de Hero na Página Inicial

Adicionar a funcionalidade de configurar múltiplos "Heros" (banners) na página inicial, permitindo que o administrador gerencie uma lista de áreas com imagem/vídeo de fundo, título e subtítulo, exibidos como um carrossel na vitrine.

## Alterações

### Banco de Dados (Supabase)
- Criar a tabela `public.hero_slides` para armazenar múltiplos banners.
- Colunas: `id` (uuid), `tipo` (enum: gradient, image, video), `media_url` (text), `titulo` (text), `subtitulo` (text), `ordem` (int), `ativo` (bool).
- Migrar os dados atuais de `site_config` para a nova tabela.
- Configurar RLS e GRANTS para a nova tabela.

### Backend/Lib
- Atualizar `src/lib/config-site.ts` para incluir funções de busca e atualização dos slides do Hero.
- Definir tipos `HeroSlide` e `SiteConfig` atualizados.

### Painel Administrativo
- Atualizar o modal de "Configurações da Tela Inicial" em `src/routes/_authenticated/admin.index.tsx`.
- Substituir os campos únicos por um gerenciador de lista de slides.
- Permitir adicionar, editar, remover e ordenar slides.
- Integrar upload de mídia individual para cada slide.

### Vitrine (Página Inicial)
- Refatorar `HeroSection` em `src/routes/index.tsx` para aceitar um array de slides.
- Implementar um carrossel (slider) para navegar entre os múltiplos banners.
- Adicionar controles de navegação (setas e indicadores).

## Detalhes Técnicos
- Utilizar `framer-motion` ou `shadcn` carousel se disponível para transições suaves.
- Garantir que o primeiro slide seja carregado com alta prioridade (`fetchPriority="high"`).
- Manter compatibilidade com o layout atual durante a migração.
