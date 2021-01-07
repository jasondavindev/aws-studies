# IAM (Identity and Access Management)

## Componentes

### IAM Identities

- IAM User: Usuários que trabalham pelo console ou programaticamente
- IAM Groups: Grupo de usuários que compartilham um nível de permissionamento
- IAM Roles: Associa permissões à roles e então atribui para User groups

### IAM Policies

- Documento JSON que limita ou dá permissões para um usuário, grupo ou role específica para acessar serviços
- Policies são anexadas à IAM Identities

## Principios

- IAM não é limitado por região, é global
- Uma policy pode ser anexada diretamente à um usuário (inline policy)
- Uma Role pode ter muitas policies anexadas
- Pode anexar roles à recursos (ex EC2)
- O usuário root (dono da conta) tem todo acesso
- Novos usuário não tem permissões. Deve ser anexado a eles
- IAM groups não podem ser aninhados, ou seja, não tem como ter subgroups
- Com IAM Policies é possível tagear recursos
- Você pode tagear recursos de produção e de desenvolvimento
- Pode dar ao usuário acesso somente á recursos de desenvolvimento
- Para acesso via CLI ou SDK é precisa de AWS Access Key ID e Secret Access Key
- Se perder as chaves é só gerar outra
- Pode habilitar MFA para um usuário

### Exemplo

```json
{
  "Version": "2012-10-17",
  "Id": "S3-Account-Permissions",
  "Statement": [{
    "Sid": "1",
    "Effect": "Allow",
    "Principal": {"AWS": ["arn:aws:iam::ACCOUNT-ID-WITHOUT-HYPHENS:root"]},
    "Action": "s3:*",
    "Resource": [
      "arn:aws:s3:::mybucket",
      "arn:aws:s3:::mybucket/*"
    ]
  }]
}
```

## Tipos

- Managed Policies são gerenciadas pela propria AWS e não são editáveis (tem um símbolo de caixa laranja)
- Customer Managed Policies são criadas e podem ser editadas pelo usuário
- Inline policies são diretamente anexadas à usuários

### IAM Password Policy

- Conjunto mínimo de regras para usuários criarem senhas
- Ex pelo menos um número, letra maiúscula ou minúscula
- Possível definir tempo de expiração

### Ordem de prioridade

- Explicit Deny proibe acesso à um recurso. Essa não pode ser anulada
- Explicit Allow permite acesso à um recurso desde que não haja uma Explicit Deny
- Default Deny IAM Identities iniciam sem nenhum acesso
