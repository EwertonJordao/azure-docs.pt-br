---
title: Métodos de autenticação - Diretório Ativo do Azure
description: Métodos de autenticação disponíveis no Azure AD para MFA e SSPR
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5a82c69575e82a7cf397955f08c3f114e449ba6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968782"
---
# <a name="what-are-authentication-methods"></a>Quais são os métodos de autenticação?

Como administrador, escolhendo métodos de autenticação para autenticação multifatorial do Azure e redefinição de senha de autoatendimento (SSPR), é recomendável que você exija que os usuários registrem vários métodos de autenticação. Quando um método de autenticação não está disponível para um usuário, ele pode optar por autenticar com outro método.

Os administradores podem definir na política quais métodos de autenticação estão disponíveis para usuários do SSPR e do MFA. Alguns métodos de autenticação podem não estar disponíveis para todos os recursos. Para obter mais informações sobre a configuração de suas políticas, consulte os artigos [Como implementar com sucesso a redefinição de senha de autoatendimento](howto-sspr-deployment.md) e o planejamento de uma [autenticação multifatorial baseada na nuvem](howto-mfa-getstarted.md)

A Microsoft recomenda que os administradores habilitem os usuários a selecionar mais do que o número mínimo necessário de métodos de autenticação, caso eles não tenham acesso a um.

|Método de autenticação|Uso|
| --- | --- |
| Senha | MFA e o SSPR |
| Perguntas de segurança | Somente o SSPR |
| Endereço de email | Somente o SSPR |
| Aplicativo Microsoft Authenticator | MFA e o SSPR |
| Token OATH de hardware | Versão prévia pública para MFA e SSPR |
| sms | MFA e o SSPR |
| Chamada de voz | MFA e o SSPR |
| Senhas de aplicativo | MFA somente em determinados casos |

![Métodos de autenticação em uso na tela de login](media/concept-authentication-methods/overview-login.png)

|     |
| --- |
| Os tokens de hardware OATH para MFA e SSPR são recursos de visualização pública do Azure Active Directory. Para obter mais informações sobre visualizações, consulte [Termos de Uso Suplementares para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="password"></a>Senha

Sua senha do AD do Azure é considerada um método de autenticação. É o único método que **não pode ser desabilitado**.

## <a name="security-questions"></a>Perguntas de segurança

As perguntas de segurança estão disponíveis **somente na redefinição de senha de auto-atendimento do Azure AD** para contas que não são de administrador.

Se você usar perguntas de segurança, é recomendável usá-las em conjunto com outro método. As perguntas de segurança podem ser menos seguras do que outros métodos porque algumas pessoas podem conhecer as respostas às perguntas de outros usuários.

> [!NOTE]
> As perguntas de segurança são armazenadas de forma privada e protegida em um objeto de usuário no diretório e somente podem ser respondidas pelos usuários durante o registro. Não há nenhuma maneira de um administrador ler ou modificar as perguntas ou respostas de um usuário.
>

### <a name="predefined-questions"></a>Perguntas predefinidas

* Em qual cidade você conheceu seu primeiro cônjuge/parceiro?
* Em qual cidade seus pais se conheceram?
* Em qual cidade seu irmão mais próximo mora?
* Em qual cidade seu pai nasceu?
* Em qual cidade você teve seu primeiro emprego?
* Em qual cidade sua mãe nasceu?
* Em qual cidade você estava no ano de 2000?
* Qual era o sobrenome de seu professor favorito no ensino médio?
* Qual é o nome de uma faculdade que você tentou entrar, mas que não frequentou?
* Qual é o nome do lugar em que você realizou sua primeira festa de casamento?
* Qual é o segundo nome de seu pai?
* Qual é sua comida favorita?
* Qual é o nome e sobrenome de sua avó materna?
* Qual é o segundo nome de sua mãe?
* Qual é o mês e ano de aniversário de seu irmão mais velho? (por exemplo, novembro de 1985)
* Qual é o segundo nome de seu irmão mais velho?
* Qual é o nome e sobrenome de seu avô paterno?
* Qual é o segundo nome de seu irmão mais novo?
* Em qual escola você concluiu a sexta série?
* Qual era o nome e sobrenome de seu melhor amigo de infância?
* Qual era o nome e sobrenome de seu primeiro parceiro?
* Qual era o sobrenome de seu professor favorito no ensino médio?
* Qual era a marca e o modelo de seu primeiro carro ou sua primeira moto?
* Qual era o nome da primeira escola em que você estudou?
* Qual é o nome do hospital em que você nasceu?
* Qual é o nome da rua da primeira casa em que morou na infância?
* Qual é o nome de seu herói de infância?
* Qual é o nome de seu animal de pelúcia preferido?
* Qual era o nome de seu primeiro animal de estimação?
* Qual era seu apelido de infância?
* Qual era seu esporte favorito no ensino médio?
* Qual foi seu primeiro emprego?
* Quais eram os últimos quatro dígitos de seu primeiro número de telefone?
* Quando criança, o que você queria ser quando crescesse?
* Quem é a pessoa mais famosa que você já conheceu?

Todas as questões de segurança predefinidas são traduzidas e localizadas no conjunto completo de idiomas do Office 365 com base na localidade do navegador do usuário.

### <a name="custom-security-questions"></a>Perguntas de segurança personalizadas

Perguntas de segurança personalizadas não estão localizadas. Todas as perguntas personalizadas são exibidas no mesmo idioma em que são inseridas na interface do usuário administrativo, mesmo se a localidade do navegador do usuário for diferente. Se precisar de perguntas localizadas, você deverá usar as perguntas predefinidas.

O tamanho máximo de uma pergunta de segurança personalizada é de 200 caracteres.

### <a name="security-question-requirements"></a>Requisitos das perguntas de segurança

* O limite mínimo para a resposta é de três caracteres.
* O limite máximo para a resposta é de 40 caracteres.
* Os usuários não podem responder à mesma pergunta mais de uma vez.
* Os usuários não podem fornecer a mesma resposta a mais de uma pergunta.
* Qualquer conjunto de caracteres pode ser usado para definir as perguntas e as respostas, incluindo caracteres Unicode.
* O número de perguntas definidas deve ser maior ou igual ao número de perguntas que foram necessárias para se registrar.

## <a name="email-address"></a>Endereço de email

O endereço de e-mail está disponível **somente na redefinição de senha de auto-atendimento do Azure AD**.

A Microsoft recomenda o uso de uma conta de e-mail que não exigiria a senha do Azure AD do usuário para acesso.

## <a name="microsoft-authenticator-app"></a>Aplicativo Microsoft Authenticator

O aplicativo Microsoft Authenticator fornece um nível adicional de segurança para seu trabalho do Azure AD ou conta de escola ou sua conta da Microsoft.

O aplicativo Microsoft Authenticator está disponível para [Android,](https://go.microsoft.com/fwlink/?linkid=866594) [iOS](https://go.microsoft.com/fwlink/?linkid=866594)e [Windows Phone.](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6)

> [!NOTE]
> Os usuários não terão a opção de registrar seu aplicativo móvel ao se registrarem para redefinição de senha de autoatendimento. Em vez disso, os [https://aka.ms/mfasetup](https://aka.ms/mfasetup) usuários podem registrar seu [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo)aplicativo móvel na pré-visualização de registro de informações de segurança em .
>

### <a name="notification-through-mobile-app"></a>Notificação pelo aplicativo móvel

O aplicativo Microsoft Authenticator pode ajudar a impedir o acesso não autorizado a contas e interromper transações fraudulentas enviando uma notificação para seu smartphone ou tablet. Os usuários visualizam a notificação e, se for legítimo, selecione Verificar. Caso contrário, eles podem selecionar Deny.

> [!WARNING]
> Para redefinição de senha de autoatendimento quando apenas um método é necessário para redefinir, o código de verificação é a única opção disponível para os usuários **garantirem o mais alto nível de segurança**.
>
> Quando dois métodos forem necessários, os usuários poderão redefinir usando notificação **SEJA**** OU** código de verificação, além de qualquer outro método ativado.
>

Se você ativar o uso da notificação por meio do aplicativo móvel e do código de verificação do aplicativo para dispositivos móveis, os usuários que registrarem o aplicativo Microsoft Authenticator usando uma notificação poderão usar a notificação e o código para confirmar sua identidade.

> [!NOTE]
> Se sua organização tem funcionários trabalhando ou viajando para a China, a notificação através do método **de aplicativo móvel** em dispositivos **Android** não funciona naquele país. Métodos alternativos devem ser disponibilizados para esses usuários.

### <a name="verification-code-from-mobile-app"></a>Código de verificação de aplicativo móvel

O aplicativo Microsoft Authenticator ou outros aplicativos de terceiros podem ser usados como um token de software para gerar um código de verificação OATH. Depois de inserir seu nome de usuário e senha, insira o código fornecido pelo aplicativo na tela de login. O código de verificação oferece uma segunda forma de autenticação.

> [!WARNING]
> Para redefinição de senha de autoatendimento quando apenas um método é necessário para redefinir o código de verificação é a única opção disponível para os usuários **para garantir o mais alto nível de segurança**.
>

Os usuários podem ter uma combinação de até cinco tokens de hardware OATH ou aplicativos autenticadores, como o aplicativo Microsoft Authenticator configurado para uso a qualquer momento.

## <a name="oath-hardware-tokens-public-preview"></a>Tokens OATH de hardware (versão prévia pública)

O OATH é um padrão livre que especifica os códigos de OTP (senha única) são gerados. O Azure AD será compatível com o uso de tokens OATH-TOTP SHA-1 das variedades de 30 segundos ou 60 segundos. Os clientes podem adquirir esses tokens do fornecedor de sua escolha. As chaves secretas são limitadas a 128 caracteres, que podem não ser compatíveis com todos os tokens. A chave secreta só pode conter os caracteres *a-z* ou *A-Z* e os dígitos *1-7*, e deve ser codificada em Base32.

![Carregando tokens OATH para a lâmina de tokens MFA OATH](media/concept-authentication-methods/mfa-server-oath-tokens-azure-ad.png)

Os tokens de hardware OATH são suportados como parte de uma visualização pública. Para obter mais informações sobre visualizações, consulte [Termos de Uso Suplementares para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

Uma vez que os tokens sejam adquiridos, eles devem ser carregados em um formato de arquivo de valores separados por comuma (CSV), incluindo o UPN, número de série, chave secreta, intervalo de tempo, fabricante e modelo, conforme mostrado no exemplo a seguir:

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,1234567abcdef1234567abcdef,60,Contoso,HardwareKey
```

> [!NOTE]
> Certifique-se de incluir a linha de cabeçalho no arquivo CSV.

Uma vez formatado corretamente como um arquivo CSV, um administrador pode então entrar no portal Azure, navegar até**os tokens****MFA** > OATH **do Azure Active Directory** > **Security** > e carregar o arquivo CSV resultante.

Dependendo do tamanho do arquivo CSV, ele poderá levar alguns minutos para ser processado. Clique no botão **Atualizar** para obter o status atual. Se houver erros no arquivo, haverá a opção de baixar um arquivo CSV listando os erros para que você possa resolvê-los. Os nomes de campo no arquivo CSV baixado são diferentes da versão enviada.

Depois que os erros forem resolvidos, o administrador poderá ativar cada chave clicando em **Ativar** para o token a ser ativado e inserindo a OTP exibido no token.

Os usuários podem ter uma combinação de até cinco tokens de hardware OATH ou aplicativos autenticadores, como o aplicativo Microsoft Authenticator configurado para uso a qualquer momento.

## <a name="phone-options"></a>Opções de telefone

### <a name="mobile-phone"></a>Telefone celular

Duas opções estão disponíveis para usuários com telefones celulares.

Se os usuários não quiserem que seu número de telefone celular seja visível no diretório, mas eles ainda querem usá-lo para redefinição de senha, os administradores não devem preenchê-lo no diretório. Os usuários devem preencher seu atributo **Telefone de Autenticação** por meio do [portal de registro de redefinição de senha](https://aka.ms/ssprsetup). Os administradores poderão ver essas informações no perfil do usuário, mas elas não serão publicadas em nenhum outro lugar.

Para funcionarem adequadamente, os números de telefone devem estar no formato *+CountryCode PhoneNumber*, por exemplo: +1 4255551234.

> [!NOTE]
> Precisa haver um espaço entre o código do país e o número de telefone.
>
> A redefinição de senha não dá suporte a ramais telefônicos. Mesmo no formato +1 4255551234X12345, as extensões são removidas antes que a chamada seja completada.

A Microsoft não garante a entrega de prompt consistente de Autenticação multifator com base em voz ou SMS pelo mesmo número. Pensando no melhor para nossos usuários, a Microsoft pode adicionar ou remover códigos curtos a qualquer momento, pois fazemos ajustes de rota para melhorar a capacidade de entrega de SMS. A Microsoft não suporta códigos curtos para países/regiões além dos Estados Unidos e Canadá.

#### <a name="text-message"></a>mensagem de texto

Um SMS é enviado para o número do celular que contém um código de verificação. Digite o código de verificação fornecido na interface de login para continuar.

#### <a name="phone-call"></a>chamada telefônica

Uma chamada de voz automatizada é feita para o número de telefone que você fornece. Atenda a chamada e pressione # no teclado do telefone para autenticar

> [!IMPORTANT]
> A partir de março de 2019, as opções de chamadas telefônicas não estarão disponíveis para usuários de MFA e SSPR em inquilinos Ad gratuitos/experimentadores. As mensagens SMS não são impactadas por essa mudança. A chamada telefônica continuará disponível para os usuários em inquilinos ad pagos do Azure. Essa alteração só afeta os inquilinos azure AD gratuitos/trial.

### <a name="office-phone"></a>Telefone comercial

Uma chamada de voz automatizada é feita para o número de telefone que você fornece. O usuário atende à chamada e pressiona # no teclado do telefone para autenticar.

Para funcionarem adequadamente, os números de telefone devem estar no formato *+CountryCode PhoneNumber*, por exemplo: +1 4255551234.

O atributo de telefone do escritório é gerenciado pelo seu administrador.

> [!IMPORTANT]
> A partir de março de 2019, as opções de chamadas telefônicas não estarão disponíveis para usuários de MFA e SSPR em inquilinos Ad gratuitos/experimentadores. As mensagens SMS não são impactadas por essa mudança. A chamada telefônica continuará disponível para os usuários em inquilinos ad pagos do Azure. Essa alteração só afeta os inquilinos azure AD gratuitos/trial.

> [!NOTE]
> Precisa haver um espaço entre o código do país e o número de telefone.
>
> A redefinição de senha não dá suporte a ramais telefônicos. Mesmo no formato +1 4255551234X12345, as extensões são removidas antes que a chamada seja completada.

### <a name="troubleshooting-phone-options"></a>Solução de problemas de opções de telefone

Problemas comuns relacionados aos métodos de autenticação usando um número de telefone:

* Identificação do chamador bloqueada em um único dispositivo
   * Dispositivo de solução de problemas
* Número de telefone errado, código de país incorreto, número de telefone residencial versus número de telefone de trabalho
   * Solucionar problemas do objeto do usuário e configurar métodos de autenticação. Certifique-se de que os números de telefone corretos estão registrados.
* PIN errado inserido
   * Confirme que o usuário usou o PIN correto registrado no Azure MFA Server.
* Chamada encaminhada para correio de voz
   * Certifique-se de que o usuário tem o telefone ligado e que o serviço está disponível em sua área ou usar método alternativo.
* Usuário está bloqueado
   * Tenha o administrador desbloqueie o usuário no portal Azure.
* O SMS não está inscrito no dispositivo
   * Mande os métodos de alteração do usuário ou ative o SMS no dispositivo.
* Provedores de telecomunicações defeituosos (Nenhuma entrada de telefone detectada, problemas de tons DTMF ausentes, ID de chamada bloqueado em vários dispositivos ou SMS bloqueado em vários dispositivos)
   * A Microsoft usa vários provedores de telecomunicações para encaminhar chamadas telefônicas e mensagens SMS para autenticação. Se você estiver vendo qualquer um dos problemas acima, o usuário tente usar o método pelo menos 5 vezes em 5 minutos e tenha as informações desse usuário disponíveis ao entrar em contato com o suporte da Microsoft.

## <a name="app-passwords"></a>Senhas de aplicativo

Alguns aplicativos que não são de navegador não oferecem suporte à autenticação de vários fatores. Se um usuário tiver sido habilitado para autenticação de vários fatores e tentar usar aplicativos sem navegador, eles não poderão se autenticar. Uma senha de aplicativo permite que os usuários continuem a autenticar

Caso você imponha a Autenticação Multifator do Microsoft Azure por meio de políticas de acesso condicional e não por MFA por usuário, não será possível criar senhas de aplicativo. Aplicativos que usam políticas de acesso condicional para controlar o acesso não precisam de senhas de aplicativo.

Se sua organização for federada para o SSO com o Azure AD e você pretender usar o MFA do Azure, esteja ciente dos seguintes detalhes:

* A senha de aplicativo é verificada pelo Azure AD e, portanto, ignora a federação. A federação é usada apenas durante a configuração de senhas de aplicativo. Para usuários federados (SSO), as senhas são armazenadas no ID organizacional. Se o usuário sair da empresa, essa informação deve fluir para o ID da organização usando o DirSync. A desabilitação/exclusão da conta pode levar até três horas para sincronizar, atrasando a desabilitação/exclusão de senhas de aplicativo no Azure AD.
* As configurações do Controle de Acesso do Cliente local não são consideradas pela senha de aplicativo.
* Nenhum recurso de registro/auditoria de autenticação local está disponível para senhas de aplicativo.
* Alguns projetos arquitetônicos avançados podem exigir o uso de uma combinação de nome de usuário e senhas da organização e senhas de aplicativo ao usar a verificação em duas etapas com os clientes, dependendo de onde eles se autenticam. Para clientes que fazem a autenticação em uma infraestrutura local, você usaria um nome de usuário e senha organizacional. Para clientes que se autenticam no Azure AD, você usaria a senha de aplicativo.
* Por padrão, os usuários não podem criar senhas de aplicativo. Se você precisar permitir que os usuários criem senhas de aplicativos, selecione a opção **Permitir que usuários criem senhas de aplicativos para fazer login em aplicativos que não são de navegador** em configurações de serviço.

## <a name="next-steps"></a>Próximas etapas

[Ativar redefinição de senha de autoatendimento para sua organização](quickstart-sspr.md)

[Ativar a autenticação de vários fatores do Azure para sua organização](howto-mfa-getstarted.md)

[Habilite o registro combinado em seu inquilino](howto-registration-mfa-sspr-combined.md)

[Documentação de configuração do método de autenticação do usuário final](https://aka.ms/securityinfoguide)
