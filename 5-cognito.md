# Amazon Cognito

- Serviço que fornece Web Identity Federation
- Não precisa escrever o código que instrui os usuários a fazer login no Facebook ou no Google em seu aplicativo
- Cognito já faz isso por você

## Cognito User Pools

- Diretório de usuários para login em suas aplicações
- Quando autenticado com sucesso, é gerado um JSON Web Token
- Lida com registro, recovery e autenticação

## Cognito Identity Pools

- São usados ​​para permitir aos usuários acesso temporário a serviços diretos da AWS
- Concedem a você o papel do IAM

## Cognito Sync

- Sincroniza os dados e preferências do usuário sobre múltiplos dispositivos
- Use push notification para atualizar e notificar
- Usa SNS para enviar notificações para todos dispositivos

## Web Identity Federation

- Permite que você dê aos seus usuários acesso aos recursos da AWS depois que eles se autenticam com sucesso em um provedor de identidade baseado na web, como Facebook, Google, Amazon
- Após um login bem-sucedido, o usuário recebe um código de autenticação do provedor de identidade que pode ser usado para obter credenciais temporárias da AWS