# Migração e publicação segura

## Estado
Este repositório é a nova implantação, separada do site publicado na Lovable. Não existe troca de domínio automática. A publicação só deve ocorrer depois de validar build, autenticação, recuperação de senha, RLS e permissões.

## Supabase
1. Crie um projeto Supabase dedicado para a nova versão.
2. Aplique as migrations em `supabase/migrations` pelo Supabase CLI ou SQL Editor.
3. Em Authentication, desligue o cadastro público (`Allow new users to sign up`). A criação de contas deve ser feita por função server-side autorizada.
4. Configure a URL de redirecionamento de recuperação de senha para o domínio de preview da Vercel e, depois, para o domínio final.
5. Nunca exponha `SUPABASE_SERVICE_ROLE_KEY` ao navegador, ao GitHub ou a variáveis `VITE_*`.
6. Crie a primeira conta de Alta Gestão por procedimento administrativo privado, depois remova/desative qualquer mecanismo temporário de bootstrap público. Nunca publique uma rota em que o primeiro visitante possa se tornar administrador.

## Vercel
1. Importe `gabrielzojota-max/RECRUTAMENTOREC` como um projeto novo.
2. Defina as variáveis do `.env.example` em Project Settings → Environment Variables. A service role key deve existir apenas no ambiente server-side.
3. Faça o deploy em preview primeiro. Não aponte o domínio do site antigo para este projeto durante os testes.
4. Execute `npm ci`/`npm install`, `npm run build`, lint e testes disponíveis. Corrija todos os erros antes de promover para produção.
5. Teste com duas contas: membro e Alta Gestão; confirme que um membro não consegue ler dados de terceiros nem executar ações de gestão por chamadas diretas.

## Checklist antes de convidar a equipe
- [ ] Build e testes aprovados
- [ ] Signup público desligado
- [ ] Bootstrap público ausente
- [ ] RLS habilitado e testado em todas as tabelas
- [ ] Membro só lê os próprios RECs/metas
- [ ] Alta Gestão pode gerir contas, metas e auditoria no servidor
- [ ] Conta desativada não acessa o painel; histórico preservado
- [ ] Recuperação de senha testada
- [ ] Variáveis secretas apenas no ambiente server-side
- [ ] Preview validado; site antigo intacto

Não copie as credenciais do projeto antigo. A nova implantação deve usar projeto Supabase separado até concluir a migração e validar os dados.