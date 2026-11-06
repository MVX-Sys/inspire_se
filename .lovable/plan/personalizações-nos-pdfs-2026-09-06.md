# Personalizações nos PDFs

## Verificação feita

Conferi o que os PDFs mostram hoje:

- **PDF do pedido**: lista quantidade, QR code, nome do produto, cor, tamanho, preço unitário e subtotal. **Não mostra nenhuma personalização** — nem a marca "Personalizado", nem quais partes foram escolhidas, nem o acréscimo em reais.
- **PDF da ficha do produto**: mostra nome, preço, selos, descrição e grade de cores/tamanhos. **Não mostra as opções de personalização disponíveis** para aquele produto.
- No carrinho e no checkout aparece apenas o selo "Personalizado", sem a lista de partes. Na mensagem do WhatsApp aparece só ", PERSONALIZADO".

Ou seja: as escolhas de personalização já são guardadas no item do carrinho (com o valor de cada uma), mas nenhum documento gerado as exibe.

## Personalizações existentes hoje

- Óculos: 2 Hastes (R$ 2,00), 1 Haste (R$ 1,00), 2 Lentes (R$ 2,00), 1 Lente (R$ 1,00)
- Case: Case (R$ 1,00)
- Lenço: Lenço (R$ 1,00)
- Sandálias com pala: Pala (R$ 2,00), Calcanhar (R$ 1,00), Lateral (R$ 1,00)
- Sandálias com regulagem (Birken): Regulagem no par (R$ 1,00), Lateral (R$ 1,00), Calcanhar (R$ 1,00)

## O que fazer

1. **PDF do pedido**: abaixo do nome/cor/tamanho de cada item, incluir a linha "Personalização: 2 Hastes (+R$ 2,00), Calcanhar (+R$ 1,00)" e mostrar no preço unitário o valor já com o acréscimo, para o total bater.
2. **Mensagem do WhatsApp e tela de checkout**: mostrar a mesma lista de partes escolhidas, no lugar do texto solto "PERSONALIZADO".
3. **Carrinho**: ao lado do selo "Personalizado", listar as partes escolhidas com o valor.
4. **PDF da ficha do produto**: acrescentar uma seção "Personalizações disponíveis" com as opções válidas para aquele produto e o preço de cada uma.

## Detalhes técnicos

- `src/lib/pdf.ts`: em `downloadOrderPDF`, montar a linha extra a partir de `it.personalizacoes` e usar `itemPrecoEfetivo` (que já soma o adicional) — ajustar a altura da linha conforme o texto extra. Em `downloadProductPDF`, chamar `getGruposPersonalizacao(p.nome, categoriaNome)` de `src/lib/personalizacao.ts` e renderizar a lista, com quebra de página como nas outras seções.
- `src/routes/checkout.tsx` (linha ~163 e ~554) e `src/components/cart-drawer.tsx` (linha ~176): mesma formatação a partir de `item.personalizacoes`.
- Criar um helper único de formatação (por exemplo `formatPersonalizacoes(item)`) em `src/lib/cart.tsx` para os quatro pontos usarem o mesmo texto.
- Sem mudanças no banco.
