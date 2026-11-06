# Plano de Correção das Imagens

O usuário relatou que as imagens dos produtos "sumiram" e o upload de banners ainda apresenta falhas. A análise técnica revelou que, embora tenhamos criado uma política de acesso público para o bucket `product-images`, a política atual de `SELECT` para o papel `public` pode não ser suficiente se o Supabase esperar permissões explícitas para `anon` ou se houver conflitos com políticas existentes. Além disso, as imagens dos produtos estão retornando erro 404/403 no navegador, indicando um problema de permissão ou caminho.

## Diagnóstico
- As imagens dos produtos usam o caminho `id_produto/uuid.ext`.
- A política de RLS para o bucket `product-images` foi definida apenas para `public`.
- No Supabase, o papel `anon` é o utilizado para acessos não autenticados na web.

## Ações

### 1. Corrigir Políticas de Armazenamento (Storage)
Vou aplicar uma migração SQL robusta para garantir que:
- O bucket `product-images` seja explicitamente público.
- As permissões de `SELECT` sejam concedidas aos papéis `anon` e `authenticated`.
- As permissões de `INSERT` e `UPDATE` sejam garantidas para funcionários e administradores.

### 2. Verificar e Corrigir o Bucket via Tool
Garantir que o bucket `product-images` esteja configurado como `public: true` usando a ferramenta nativa, pois isso afeta como o Supabase serve os arquivos (URL pública vs. URL assinada).

### 3. Ajustar o Componente de Upload
Reforçar a lógica de tratamento de erros no upload para fornecer feedback mais claro se a falha persistir.

## Detalhes Técnicos (SQL)
```sql
-- Garantir que o bucket existe e é público (via tool)
-- Aplicar políticas de RLS
CREATE POLICY "Acesso Publico Imagens 2026" ON storage.objects
FOR SELECT TO anon, authenticated
USING (bucket_id = 'product-images');

CREATE POLICY "Upload Imagens Admin 2026" ON storage.objects
FOR INSERT TO authenticated
WITH CHECK (bucket_id = 'product-images');

CREATE POLICY "Update Imagens Admin 2026" ON storage.objects
FOR UPDATE TO authenticated
USING (bucket_id = 'product-images');
```

---
Vou prosseguir com a aplicação dessas correções para restaurar a visibilidade das imagens imediatamente.
