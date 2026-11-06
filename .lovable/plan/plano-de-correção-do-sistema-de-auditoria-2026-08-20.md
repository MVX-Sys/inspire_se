# Plano de Correção do Sistema de Auditoria

O sistema de auditoria não está registrando as ações administrativas porque a função `logAudit` não está sendo chamada em diversos fluxos críticos (categorias, cupons, banners, usuários, atendentes) e as páginas de visualização (`admin.auditoria.tsx` e `admin.avancado.tsx`) podem estar enfrentando problemas de permissão RLS para exibir os dados, apesar deles existirem no banco.

## Alterações Propostas

### 1. Reforçar o Utilitário de Auditoria
- Revisar `src/lib/audit.ts` para garantir que as inserções sejam resilientes.

### 2. Implementar Auditoria em Múltiplos Fluxos de Gerenciamento
Vou adicionar chamadas à função `logAudit` nos seguintes locais para garantir cobertura total:

#### Gerenciamento de Cupons
- **`src/lib/coupons.functions.ts`**: Adicionar auditoria em `saveCupon` (criação e edição) e `deleteCupon`.

#### Gerenciamento de Usuários e Permissões
- **`src/lib/admin-users.functions.ts`**: Adicionar auditoria em `setUserRole`, `setUserPermissions` e `deleteUserAccess`.

#### Configurações do Site (Banners/Hero)
- **`src/lib/config-site.ts`**: Adicionar auditoria em `updateHeroSlide`, `createHeroSlide` e `deleteHeroSlide`.

#### Gerenciamento de Atendentes
- **`src/lib/atendentes.functions.ts`**: Adicionar auditoria em `createAtendente`, `updateAtendente` e `deleteAtendente`.

#### Gerenciamento de Categorias
- **`src/lib/products.functions.ts`**: Adicionar auditoria nas funções de gerenciamento de categorias (se existirem, ou adicionar nos componentes correspondentes).

### 3. Correção de RLS e Permissões de Visualização
- Aplicar políticas SQL para garantir que usuários com a permissão `auditoria.view` (ou administradores) possam ler a tabela `admin_audit_log`.

### 4. Registro de Login
- **`src/lib/auth.tsx`**: Adicionar um registro de auditoria (`login`) quando um usuário com perfil administrativo entra no sistema.

## Detalhes Técnicos
- As auditorias em `server functions` garantem que o log ocorra apenas após o sucesso da operação.
- Usaremos `try-catch` em volta de `logAudit` para que falhas no log não impeçam a funcionalidade principal, mas reportaremos erros no console.

## Verificação
- Criarei um cupom e editarei um banner para confirmar que os logs aparecem em tempo real no painel de auditoria.
