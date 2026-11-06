# Trocar a descrição do site

Substituir "Estilo Urbano Sem Limites" por "Atacado para todo o Brasil" onde o texto aparece.

## O que muda

1. Título da página inicial (o que aparece na aba do navegador e no Google): passa a ser "ACHAEBUSCA — Atacado para todo o Brasil".
2. Texto grande do banner da home: hoje o texto padrão gravado no banco é "Estilo Urbano Sem Limites". Vai ser atualizado para "Atacado para todo o Brasil" (valor padrão e a linha já existente na configuração do site).

## Detalhes técnicos

- `src/routes/index.tsx`: atualizar `head()` — `title` e, para manter coerência, `og:title`/descrições que citam o slogan antigo.
- Migração nova: `ALTER TABLE ... ALTER COLUMN hero_title SET DEFAULT 'Atacado para todo o Brasil'` e `UPDATE` da linha de configuração existente que ainda contém o valor antigo.

## Observação

O banco hospedado está pausado no momento; a migração só será aplicada quando ele estiver ativo. A parte do título da página funciona de imediato.
