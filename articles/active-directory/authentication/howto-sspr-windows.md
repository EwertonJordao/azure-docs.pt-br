---
title: Redefinição de senha de autoatendimento para Windows-Azure Active Directory
description: Como habilitar a redefinição de senha de autoatendimento usando a senha esquecida na tela de logon do Windows
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4f08161daf1d9c1a4431d9e3fba3ca741d88b16
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80743351"
---
# <a name="how-to-enable-password-reset-from-the-windows-login-screen"></a>Como habilitar a redefinição de senha na tela de logon do Windows

Para computadores que executam o Windows 7, 8, 8,1 e 10, você pode permitir que os usuários redefinam sua senha na tela de logon do Windows. Os usuários não precisam mais localizar um dispositivo com um navegador da Web para acessar o [portal do SSPR](https://aka.ms/sspr).

![Exemplo de telas de logon do Windows 7 e 10 com o link SSPR mostrado](./media/howto-sspr-windows/windows-reset-password.png)

## <a name="general-limitations"></a>Limitações gerais

- Atualmente, a redefinição de senha não tem suporte de uma Área de Trabalho Remota ou de sessões avançadas do Hyper-V.
- Alguns provedores de credenciais de terceiros são conhecidos por causar problemas com esse recurso.
- A desabilitação do UAC por meio da modificação da [chave do registro EnableLUA](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpsb/958053ae-5397-4f96-977f-b7700ee461ec) é conhecida por causar problemas.
- Esse recurso não funciona para redes com autenticação de rede 802.1 x implantada e a opção "executar imediatamente antes do logon do usuário". Para redes com autenticação de rede 802.1x implantada, é recomendável usar a autenticação de computador para habilitar esse recurso.
- Os computadores ingressados no Azure AD híbrido devem ter a linha de visão de conectividade de rede para um controlador de domínio para usar a nova senha e atualizar as credenciais armazenadas em cache.
- Se estiver usando uma imagem, antes de executar o Sysprep, verifique se o cache da Web foi limpo para o administrador interno antes de executar a etapa CopyProfile. Mais informações sobre essa etapa podem ser encontradas no artigo de suporte [desempenho ruim ao usar o perfil de usuário padrão personalizado](https://support.microsoft.com/help/4056823/performance-issue-with-custom-default-user-profile).
- As configurações a seguir são conhecidas por interferir na capacidade de usar e redefinir senhas em dispositivos Windows 10
    - Se as teclas de atalho Ctrl+Alt+Del forem exigidas pela política nas versões do Windows 10 anteriores a v1809, a **Redefinição de senha** não funcionará.
    - Se as notificações de tela de bloqueio estiverem desativadas, a **Redefinição de senha** não funcionará.
    - HideFastUserSwitching é definido como habilitado ou 1
    - DontDisplayLastUserName é definido como habilitado ou 1
    - NoLockScreen é definido como habilitado ou 1
    - EnableLostMode é definido no dispositivo
    - Explorer.exe é substituído por um shell personalizado
- A combinação das três configurações específicas a seguir pode fazer com que esse recurso não funcione.
    - Logon interativo: não exigir CTRL + ALT + DEL = Disabled
    - DisableLockScreenAppNotifications = 1 ou habilitado
    - O Windows SKU não é home ou Professional Edition

## <a name="windows-10-password-reset"></a>Redefinição de senha do Windows 10

### <a name="windows-10-prerequisites"></a>Pré-requisitos do Windows 10

- Um administrador deve habilitar a redefinição de senha de autoatendimento do Azure AD no portal do Azure.
- **Os usuários devem se registrar no SSPR antes de usar esse recurso**
- Requisitos de proxy de rede
   - Dispositivos Windows 10 
       - Porta 443 para `passwordreset.microsoftonline.com` e`ajax.aspnetcdn.com`
       - Dispositivos Windows 10 dão suporte apenas à configuração de proxy no nível de computador
- Execute pelo menos o Windows 10, versão de abril de 2018 atualização (v1803) e os dispositivos devem ser:
    - Adicionado ao Azure AD
    - Adicionado ao Azure AD híbrido

### <a name="enable-for-windows-10-using-intune"></a>Habilitar para Windows 10 usando o Intune

Implantar a alteração de configuração para habilitar a redefinição de senha da tela de logon usando o Intune é o método mais flexível. O Intune permite que você implante a alteração de configuração para um grupo específico de computadores que você definir. Esse método requer o registro do dispositivo no Intune.

#### <a name="create-a-device-configuration-policy-in-intune"></a>Criar uma política de configuração do dispositivo no Intune

1. Entre no [portal do Azure](https://portal.azure.com) e clique em **Intune**.
1. Crie um novo perfil de configuração de dispositivo acessando **dispositivo** > **perfis** > de configuração**Criar perfil**
   - Forneça um nome significativo para o perfil
   - Opcionalmente, forneça uma descrição significativa do perfil
   - Plataforma **Windows 10 e posterior**
   - Tipo de perfil **Personalizado**
1. Definir **configurações**
   - **Adicionar** a seguinte configuração de OMA-URI para habilitar o link Redefinir senha
      - Forneça um nome significativo para explicar o que a configuração está fazendo
      - Opcionalmente, forneça uma descrição significativa da configuração
      - **OMA-URI** definido como `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      - **Tipo de dados** definido como **Inteiro**
      - **Valor** definido como **1**
      - Clique em **OK**
   - Clique em **OK**
1. Clique em **Criar**
1. Essa política pode ser atribuída a usuários, dispositivos ou grupos específicos. Mais informações podem ser encontradas no artigo [atribuir perfis de usuário e de dispositivo no Microsoft Intune](https://docs.microsoft.com/intune/device-profile-assign).

### <a name="enable-for-windows-10-using-the-registry"></a>Habilitar para Windows 10 usando o registro

1. Entre no Windows PC usando credenciais administrativas
1. Execute o **regedit** como administrador
1. Defina a seguinte chave do registro
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      - `"AllowPasswordReset"=dword:00000001`

#### <a name="troubleshooting-windows-10-password-reset"></a>Solucionando problemas de redefinição de senha do Windows 10

O log de auditoria do Microsoft Azure AD inclui informações sobre o endereço IP e o ClientType em que a redefinição de senha ocorreu.

![Exemplo de redefinição de senha do Windows 7 no log de auditoria do Azure AD](media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

Quando os usuários redefinem sua senha na tela de logon de um dispositivo Windows 10, uma conta temporária de `defaultuser1` baixo privilégio chamada é criada. Essa conta é usada para manter o processo de redefinição de senha seguro. A conta em si tem uma senha gerada aleatoriamente, não aparece para entrar no dispositivo e será removida automaticamente depois que o usuário redefinir sua senha. Vários `defaultuser` perfis podem existir, mas podem ser ignorados com segurança.

## <a name="windows-7-8-and-81-password-reset"></a>Redefinição de senha do Windows 7, 8 e 8,1

### <a name="windows-7-8-and-81-prerequisites"></a>Pré-requisitos do Windows 7, 8 e 8,1

- Um administrador deve habilitar a redefinição de senha de autoatendimento do Azure AD no portal do Azure.
- **Os usuários devem se registrar no SSPR antes de usar esse recurso**
- Requisitos de proxy de rede
   - Dispositivos Windows 7, 8 e 8,1
       - Porta 443 para`passwordreset.microsoftonline.com`
- Sistema operacional Windows 7 ou Windows 8.1 corrigido.
- TLS 1.2 habilitado usando a diretriz encontrada em [Configurações de registro do protocolo TLS](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12).
- Se mais de um provedor de credenciais de terceiros estiver habilitado em seu computador, os usuários verão mais de um perfil de usuário na tela de logon.

> [!WARNING]
> O TLS 1,2 deve ser habilitado, não apenas definido como negociação automática

### <a name="install"></a>Instalar

1. Baixe o instalador correto para a versão do Windows que você quer habilitar.
   - O software está disponível no centro de download da Microsoft em[https://aka.ms/sspraddin](https://aka.ms/sspraddin)
1. Entre no computador em que você quer instalar e execute o instalador.
1. Após a instalação, uma reinicialização é altamente recomendável.
1. Após a reinicialização, na tela de logon, escolha um usuário e clique em "esqueceu a senha?" para iniciar o fluxo de trabalho de redefinição de senha.
1. Conclua o fluxo de trabalho seguindo as etapas na tela para redefinir sua senha.

![Exemplo de clique em "Esqueceu a senha?" no Windows 7 Fluxo de SSPR](media/howto-sspr-windows/windows-7-sspr.png)

#### <a name="silent-installation"></a>Instalação silenciosa

- Para instalação silenciosa, use o comando "msiexec/i SsprWindowsLogon. PROD. msi/qn"
- Para desinstalação silenciosa, use o comando "msiexec/x SsprWindowsLogon. PROD. msi/qn"

#### <a name="troubleshooting-windows-7-8-and-81-password-reset"></a>Solução de problemas de redefinição de senha do Windows 7, 8 e 8,1

Os eventos serão registrados no computador e no Azure AD. Os eventos do Azure AD incluirão informações sobre o endereço IP e o ClientType em que a redefinição de senha ocorreu.

![Exemplo de redefinição de senha do Windows 7 no log de auditoria do Azure AD](media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

Se for necessário outro registro em log, uma chave do Registro no computador poderá ser alterada para habilitar o registro em log detalhado. Habilite o registro detalhado somente para fins de solução de problemas.

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{86D2F0AC-2171-46CF-9998-4E33B3D7FD4F}`

- Para habilitar o log detalhado, crie `REG_DWORD: "EnableLogging"`um e defina-o como 1.
- Para desabilitar o `REG_DWORD: "EnableLogging"` log detalhado, altere para 0.

## <a name="what-do-users-see"></a>O que os usuários veem

Agora que você configurou a redefinição de senha para seus dispositivos Windows, o que muda para o usuário? Como eles sabem que podem redefinir sua senha na tela de logon?

![Exemplo de telas de logon do Windows 7 e 10 com o link SSPR mostrado](./media/howto-sspr-windows/windows-reset-password.png)

Quando os usuários tentam entrar, eles agora veem um link **Redefinir senha** ou **esqueceu a senha** que abre a experiência de redefinição de senha de autoatendimento na tela de logon. Essa funcionalidade permite aos usuários redefinir a senha sem a necessidade de usar outro dispositivo para acessar um navegador da Web.

Os usuários podem encontrar orientações sobre esse recuso em [Redefinir sua senha corporativa ou de estudante](../user-help/active-directory-passwords-update-your-own-password.md)

## <a name="next-steps"></a>Próximas etapas

[Planejar métodos de autenticação para permitir](concept-authentication-methods.md)

[Configurar o Windows 10](https://docs.microsoft.com/windows/configuration/)
