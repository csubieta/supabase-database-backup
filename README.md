# Backup do Banco de Dados Supabase com GitHub Actions

Este repositório fornece uma maneira simples e automatizada de criar backups do seu banco de dados Supabase usando o GitHub Actions. Ele cria backups diários dos papéis (roles), esquema (schema) e dados do banco de dados, armazenando-os no seu repositório. Também inclui um mecanismo para restaurar facilmente o banco de dados caso algo dê errado.

---

## Funcionalidades

- **Backups Diários Automáticos:** Backups agendados são executados todos os dias à meia-noite.
- **Separação de Papéis, Esquema e Dados:** Cria arquivos de backup modulares para papéis, esquema e dados.
- **Controle Flexível do Workflow:** Permite ativar ou desativar os backups com uma simples variável de ambiente.
- **Integração com GitHub Action:** Utiliza o GitHub Actions, gratuito e confiável, para automação.
- **Restauração Fácil do Banco de Dados:** Instruções claras para restaurar o banco de dados a partir dos backups.

---

## Primeiros Passos

### 1. **Configurar Variáveis do Repositório**

Vá até as configurações do seu repositório e navegue até **Actions > Variables**.  
Adicione o seguinte:

- **Secrets:**

  - `SUPABASE_DB_URL`: Sua string de conexão PostgreSQL do Supabase. Formato:  
    `postgresql://<USUÁRIO>:<SENHA>@<HOST>:5432/postgres`

- **Variáveis:**
  - `BACKUP_ENABLED`: Defina como `true` para habilitar os backups ou `false` para desativá-los.

---

### 2. **Como o Workflow Funciona**

O workflow do GitHub Actions é acionado em:

- Pushes ou pull requests para as branches `main` ou `dev`.
- Execução manual via interface do GitHub.
- Agendamento diário à meia-noite.

O workflow executa as seguintes etapas:

1. Verifica se os backups estão habilitados usando a variável `BACKUP_ENABLED`.
2. Executa o Supabase CLI para criar três arquivos de backup:
   - `roles.sql`: Contém papéis e permissões.
   - `schema.sql`: Contém a estrutura do banco de dados.
   - `data.sql`: Contém os dados das tabelas.
3. Faz o commit dos backups no repositório usando uma ação de auto-commit.

---

### 3. **Restaurando Seu Banco de Dados**

Para restaurar seu banco de dados:

1. Instale o [Supabase CLI](https://supabase.com/docs/guides/cli).
2. Abra um terminal e navegue até a pasta que contém seus arquivos de backup.
3. Execute os seguintes comandos na ordem:

```bash
supabase db execute --db-url "<SUPABASE_DB_URL>" -f roles.sql
supabase db execute --db-url "<SUPABASE_DB_URL>" -f schema.sql
supabase db execute --db-url "<SUPABASE_DB_URL>" -f data.sql
```

Isso restaura os papéis, o esquema e os dados, retornando o banco de dados ao estado salvo no backup.

---

### Alternância do Workflow

Use a variável `BACKUP_ENABLED` para controlar se os backups serão executados:

- Defina como `true` para habilitar os backups.  
- Defina como `false` para pular os backups sem precisar editar o arquivo do workflow.

---

## Requisitos

- Um projeto Supabase com banco de dados PostgreSQL.  
- Supabase CLI instalado para restauração manual.  
- Um repositório GitHub com Actions habilitado.

---

## Contribuindo

Contribuições são bem-vindas!  
Se você tiver melhorias ou correções, sinta-se à vontade para enviar um *pull request*.

---

## Licença

Este projeto está licenciado sob a Licença MIT.  
Consulte o arquivo LICENSE para mais detalhes.
