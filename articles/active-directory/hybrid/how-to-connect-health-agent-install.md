---
title: Instalação do Agente do Azure AD Connect Health | Microsoft Docs
description: Esta é a página do Azure AD Connect Health que descreve a instalação do agente para o AD FS e Sincronização.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
editor: curtand
ms.assetid: 1cc8ae90-607d-4925-9c30-6770a4bd1b4e
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2017
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b9b3857a5ae845f5cc48464152bb6ca600444c1b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82136695"
---
# <a name="azure-ad-connect-health-agent-installation"></a>Instalação do Agente do Azure AD Connect Health

Este documento explica como instalar e configurar os Agentes do Azure AD Connect Health. Você pode baixar os agentes [daqui](how-to-connect-install-roadmap.md#download-and-install-azure-ad-connect-health-agent).

## <a name="requirements"></a>Requisitos

A tabela a seguir é uma lista de requisitos para o uso do Azure AD Connect Health.

| Requisito | Descrição |
| --- | --- |
| AD Premium do Azure |O Azure AD Connect Health é um recurso do Azure AD Premium e requer o Azure AD Premium. <br /><br />Para obter mais informações, consulte [introdução ao Azure ad Premium](../fundamentals/active-directory-get-started-premium.md) <br />Para iniciar uma avaliação gratuita de 30 dias, confira [Iniciar uma avaliação.](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Você deve ser um administrador global do Azure AD para começar a usar o Azure AD Connect Health |Por padrão, somente os administradores globais podem instalar e configurar os agentes de integridade para começar, acessar o portal e realizar operações no Azure AD Connect Health. Para saber mais, consulte [Administrar seu diretório do Azure AD](../fundamentals/active-directory-administer.md). <br /><br />  Usando o Controle de Acesso com Base em Funções, você pode permitir acesso ao Azure AD Connect Health para outros usuários em sua organização. Para saber mais, confira [Controle de Acesso com Base em Função para o Azure AD Connect Health.](how-to-connect-health-operations.md#manage-access-with-role-based-access-control) <br /><br />**Importante:** a conta usada ao instalar os agentes deve ser uma conta corporativa ou de estudante. Não pode ser uma conta da Microsoft. Para obter mais informações, consulte [inscrever-se no Azure como uma organização](../fundamentals/sign-up-organization.md) |
| O Agente do Azure AD Connect Health é instalado em cada servidor de destino | O Azure AD Connect Health requer que os agentes de integridade sejam instalados e configurados nos servidores de destino para receber os dados e fornecer os recursos de Monitoramento e Análise. <br /><br />Por exemplo, para obter dados da sua infraestrutura do AD FS, o agente deverá ser instalado nos servidores do AD FS e do Proxy de Aplicativo Web. Da mesma forma, para obter dados da infraestrutura do AD DS local, o agente deve ser instalado nos controladores de domínio. <br /><br /> |
| Conectividade de saída para os pontos de extremidade de serviço do Azure | Durante a instalação e o runtime, o agente requer conectividade com os pontos de extremidade de serviço do Azure AD Connect Health. Se a conectividade de saída estiver bloqueada por meio de Firewalls, verifique se os pontos de extremidade a seguir foram adicionados à lista de permissões. Veja [Controle de conectividade de saída](how-to-connect-health-agent-install.md#outbound-connectivity-to-the-azure-service-endpoints) |
|Conectividade de saída com base em endereços IP | Para a filtragem baseada em endereço IP em firewalls, veja [Intervalos IP do Azure](https://www.microsoft.com/download/details.aspx?id=41653).|
| A inspeção TLS para o tráfego de saída é filtrada ou desabilitada | A etapa de registro do agente ou as operações de carregamento de dados poderão falhar se houver uma inspeção ou terminação de TLS para o tráfego de saída na camada de rede. Leia mais sobre [como configurar a inspeção de TLS](https://technet.microsoft.com/library/ee796230.aspx) |
| Portas de firewall no servidor que executa o agente |O agente requer que as seguintes portas de firewall estejam abertas para que o agente se comunique com os pontos de extremidade de serviço do Azure AD Health.<br /><br /><li>Porta TCP 443</li><li>Porta TCP 5671</li> <br />Observe que a porta 5671 não é mais necessária para a versão mais recente do agente. Atualize para a versão mais recente para que somente a porta 443 seja exigida. Leia mais sobre [habilitar portas do firewall](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
| Permita os sites a seguir se a segurança reforçada do IE estiver habilitada |Se a Segurança Aprimorada do IE estiver habilitada, os sites a seguir precisarão receber permissão no servidor no qual o agente será instalado.<br /><br /><li>https:\//login.microsoftonline.com</li><li>https:\//secure.aadcdn.microsoftonline-p.com</li><li>https:\//login.windows.net</li><li>https:\//aadcdn.msftauth.net</li><li>O servidor de federação da sua organização confiável pelo Azure Active Directory. Por exemplo: https:\//sts.contoso.com</li> Leia mais sobre [como configurar o IE](https://support.microsoft.com/help/815141/internet-explorer-enhanced-security-configuration-changes-the-browsing). Caso você tenha um proxy em sua rede, consulte a observação abaixo.|
| Certifique-se de que o PowerShell v4.0 ou mais recente esteja instalado | <li>O Windows Server 2008 R2 é fornecido com o PowerShell v 2.0, que não é suficiente para o agente. Atualize o PowerShell como explicado abaixo em [Instalação do agente em servidores do Windows Server 2008 R2](#agent-installation-on-windows-server-2008-r2-servers).</li><li>O Windows Server 2012 é fornecido com o PowerShell v 3.0, que não é suficiente para o agente.  [Atualize](https://www.microsoft.com/download/details.aspx?id=40855) o Windows Management Framework.</li><li>O Windows Server 2012 R2 e posterior é fornecido com uma versão suficientemente recente do PowerShell.</li>|
|Desabilitar FIPS|Não há suporte para FIPS nos agentes do Azure AD Connect Health.|


> [!NOTE]
> Se você tiver um ambiente altamente bloqueado e extremamente restrito, precisará de uma lista de permissões das URLs mencionadas nas listas de pontos de extremidade de serviço abaixo, além daqueles listados na configuração de segurança avançada do IE permitida acima. 
>

### <a name="outbound-connectivity-to-the-azure-service-endpoints"></a>Conectividade de saída para os pontos de extremidade de serviço do Azure

 Durante a instalação e o runtime, o agente requer conectividade com os pontos de extremidade de serviço do Azure AD Connect Health. Se a conectividade de saída estiver bloqueada por firewalls, verifique se as URLs a seguir não estão bloqueadas por padrão. Permita o monitoramento de segurança ou a inspeção dessas URLs, como você faria com outro tráfego de Internet. Elas possibilitam a comunicação com pontos de extremidade de serviço do Azure AD Connect Health. Saiba como [verificar a conectividade de saída com Test-AzureADConnectHealthConnectivity](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-agent-install#test-connectivity-to-azure-ad-connect-health-service).

| Ambiente de Domínio | Pontos de extremidade de serviço do Azure exigidos |
| --- | --- |
| Público geral | <li>&#42;.blob.core.windows.net </li><li>&#42;.aadconnecthealth.azure.com </li><li>&#42;.servicebus.windows.net - Port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https:\//management.azure.com </li><li>https:\//policykeyservice.dc.ad.msft.net/</li><li>https:\//login.windows.net</li><li>https:\//login.microsoftonline.com</li><li>https:\//secure.aadcdn.microsoftonline-p.com </li><li>https:\//www.office.com *esse ponto de extremidade é usado somente para fins de descoberta durante o registro.</li> |
| Azure Alemanha | <li>&#42;.blob.core.cloudapi.de </li><li>&#42;.servicebus.cloudapi.de </li> <li>&#42;.aadconnecthealth.microsoftazure.de </li><li>https:\//management.microsoftazure.de </li><li>https:\//policykeyservice.aadcdi.microsoftazure.de </li><li>https:\//login.microsoftonline.de </li><li>https:\//secure.aadcdn.microsoftonline-p.de </li><li>https:\//www.office.de *esse ponto de extremidade é usado somente para fins de descoberta durante o registro.</li> |
| Azure Government | <li>&#42;.blob.core.usgovcloudapi.net </li> <li>&#42;.servicebus.usgovcloudapi.net </li> <li>&#42;.aadconnecthealth.microsoftazure.us </li> <li>https:\//management.usgovcloudapi.net </li><li>https:\//policykeyservice.aadcdi.azure.us </li><li>https:\//login.microsoftonline.us </li><li>https:\//secure.aadcdn.microsoftonline-p.com </li><li>https:\//www.office.com *esse ponto de extremidade é usado somente para fins de descoberta durante o registro.</li> |


## <a name="download-and-install-the-azure-ad-connect-health-agent"></a>Baixar e instalar o Agente do Azure AD Connect Health

* Verifique se você [atende aos requisitos](how-to-connect-health-agent-install.md#requirements) para o Azure AD Connect Health.
* Introdução ao uso do Azure AD Connect Health do AD FS
    * [Baixe o Agente do Azure AD Connect Health para AD FS.](https://go.microsoft.com/fwlink/?LinkID=518973)
    * [Consulte as instruções de instalação](#installing-the-azure-ad-connect-health-agent-for-ad-fs).
* Introdução ao uso do Azure AD Connect Health para sincronização
    * [Baixe e instale a versão mais recente do Azure AD Connect](https://go.microsoft.com/fwlink/?linkid=615771). O Agente de Integridade para sincronização será instalado como parte da instalação do Azure AD Connect (versão 1.0.9125.0 ou superior).
* Introdução ao uso do Azure AD Connect Health para o AD DS
    * [Baixe o Agente do Azure AD Connect Health para AD DS](https://go.microsoft.com/fwlink/?LinkID=820540).
    * [Consulte as instruções de instalação](#installing-the-azure-ad-connect-health-agent-for-ad-ds).

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Instalando o Agente do Azure AD Connect Health para AD FS

> [!NOTE]
> O servidor do AD FS deve ser diferente do seu servidor de sincronização. Não instale o agente do AD FS em seu servidor de sincronização.
>

Antes da instalação, verifique se o nome de host do servidor do AD FS é exclusivo e não está presente no serviço do AD FS.
Para iniciar a instalação do agente, dê um clique duplo no arquivo .exe que você baixou. Na primeira tela, clique em instalar.

![Verifique o Azure AD Connect Health](./media/how-to-connect-health-agent-install/install1.png)

Depois que a instalação for concluída, clique em Configurar agora.

![Verifique o Azure AD Connect Health](./media/how-to-connect-health-agent-install/install2.png)

Isso abrirá uma janela do PowerShell para iniciar o processo de registro do agente. Quando solicitado, entre com uma conta do Azure AD que tenha acesso para executar o registro do agente. Por padrão, a conta de Administrador Global tem acesso.

![Verifique o Azure AD Connect Health](./media/how-to-connect-health-agent-install/install3.png)

Depois de entrar, o PowerShell continuará. Depois de concluir, feche o PowerShell e a configuração estará concluída.

Neste ponto, os serviços do agente devem ser iniciados automaticamente permitindo que o carregamento do agente os dados necessários para o serviço de nuvem de maneira segura.

Se você não tiver atendido a todos os pré-requisitos descritos nas seções anteriores, serão exibidos avisos na janela do PowerShell. Não se esqueça de concluir os [requisitos](how-to-connect-health-agent-install.md#requirements) antes de instalar o agente. A captura de tela a seguir é um exemplo desses erros.

![Verifique o Azure AD Connect Health](./media/how-to-connect-health-agent-install/install4.png)

Para verificar se o agente foi instalado, procure os serviços a seguir no servidor. Se você tiver concluído a configuração, eles já deverão estar em execução. Caso contrário, eles ficarão parados até a conclusão da configuração.

* Serviço de diagnóstico do AD FS do Azure AD Connect Health
* Serviço do Insights do AD FS do Azure AD Connect Health
* Serviço de Monitoramento do AD FS do Azure AD Connect Health

![Verifique o Azure AD Connect Health](./media/how-to-connect-health-agent-install/install5.png)

### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Instalação do agente em servidores do Windows Server 2008 R2

Etapas para servidores Windows Server 2008 R2:

1. Certifique-se de que o servidor esteja em execução no Service Pack 1 ou superior.
2. Desative a ESC do IE para instalação do agente:
3. Instale o Windows PowerShell 4.0 em cada um dos servidores antes de instalar o agente de integridade do AD. Para instalar o Windows PowerShell 4.0:
   * Instale o [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) usando o link a seguir para baixar o instalador offline.
   * Instale o PowerShell ISE (de recursos do Windows)
   * Instale o [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
   * Instale o Internet Explorer versão 10 ou superior no servidor. (Necessário para que o serviço de integridade faça sua autenticação usando suas credenciais de administrador do Azure).
4. Para saber mais sobre como instalar o Windows PowerShell 4.0 no Windows Server 2008 R2, consulte o artigo no wiki [aqui](https://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Habilitar a auditoria do AD FS

> [!NOTE]
> Esta seção se aplica apenas aos servidores do AD FS. Você não precisa seguir estas etapas nos servidores de Proxy de aplicativo Web.
>

Para que o recurso de Análise de Uso colete e analise dados, o agente do Azure AD Connect Health precisa das informações nos Logs de auditoria do AD FS. Esses logs não estão habilitados por padrão. Use os procedimentos a seguir para habilitar a auditoria do AD FS e localizar os logs de auditoria do AD FS em seus servidores do AD FS.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2008-r2"></a>Para habilitar a auditoria do AD FS no Windows Server 2008 R2

1. Clique em **Iniciar**, aponte para **Programas**, aponte para **Ferramentas administrativas** e, em seguida, clique em **Política de segurança local**.
2. Navegue até a pasta **Configurações de segurança\Políticas locais\Atribuição de Direitos do Usuário** e clique duas vezes em **Gerar auditoria de segurança**.
3. Na guia **Configuração de segurança local**, verifique se a conta de serviço AD FS 2.0 está listada. Se não estiver, clique em **Adicionar Usuário ou Grupo** e adicione-a à lista. Em seguida, clique em **OK**.
4. Para habilitar a auditoria, abra um prompt de comando com privilégios elevados e execute o seguinte comando: <code>auditpol.exe /set /subcategory:{0CCE9222-69AE-11D9-BED3-505054503030} /failure:enable /success:enable</code>
5. Feche a **Política de Segurança Local**.
<br />   -- **As seguintes etapas são exigidas apenas para servidores de AD FS primários.** -- <br />
6. Abra o snap-in **Gerenciamento do AD FS**. Para abrir o snap-in Gerenciamento do AD FS, clique em **Iniciar**, aponte para **Programas**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento do AD FS 2.0**.
7. No painel **Ações**, clique em **Editar Propriedades do Serviço de Federação**.
8. Na caixa de diálogo **Propriedades do Serviço de Federação**, clique na guia **Eventos**.
9. Marque as caixas de seleção **Auditorias com êxito** e **Auditorias com falha**.
10. Clique em **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Para habilitar a auditoria do AD FS no Windows Server 2012 R2

1. Abra **Política de Segurança Local** abrindo o **Gerenciador do Servidor** na tela Iniciar ou o Gerenciador do Servidor na barra de tarefas na área de trabalho e, em seguida, clique em **Ferramentas/Política de Segurança Local**.
2. Navegue até a pasta **Configurações de segurança\Políticas locais\Atribuição de Direitos do Usuário** e clique duas vezes em **Gerar auditoria de segurança**.
3. Na guia **Configuração de segurança local**, verifique se a conta de serviço AD FS está listada. Se não estiver, clique em **Adicionar Usuário ou Grupo** e adicione-a à lista. Em seguida, clique em **OK**.
4. Para habilitar a auditoria, abra um prompt de comando com privilégios elevados e execute o seguinte comando: ```auditpol.exe /set /subcategory:{0CCE9222-69AE-11D9-BED3-505054503030} /failure:enable /success:enable```
5. Feche a **Política de Segurança Local**.
<br />   -- **As seguintes etapas são exigidas apenas para servidores de AD FS primários.** -- <br />
6. Abra o snap-in **Gerenciamento do AD FS** (no Gerenciador do Servidor, clique em Ferramentas e selecione Gerenciamento do AD FS).
7. No painel **Ações**, clique em **Editar Propriedades do Serviço de Federação**.
8. Na caixa de diálogo **Propriedades do Serviço de Federação**, clique na guia **Eventos**.
9. Marque as caixas de seleção **Auditorias com êxito e Auditorias com falha** e clique em **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2016"></a>Para habilitar a auditoria do AD FS no Windows Server 2016

1. Abra **Política de Segurança Local** abrindo o **Gerenciador do Servidor** na tela Iniciar ou o Gerenciador do Servidor na barra de tarefas na área de trabalho e, em seguida, clique em **Ferramentas/Política de Segurança Local**.
2. Navegue até a pasta **Configurações de segurança\Políticas locais\Atribuição de Direitos do Usuário** e clique duas vezes em **Gerar auditoria de segurança**.
3. Na guia **Configuração de segurança local**, verifique se a conta de serviço AD FS está listada. Se ela não estiver presente, clique em **Adicionar Usuário ou Grupo** e adicione a conta de serviço do AD FS à lista e clique em **OK**.
4. Para habilitar a auditoria, abra um prompt de comando com privilégios elevados e execute o seguinte comando: <code>auditpol.exe /set /subcategory:{0CCE9222-69AE-11D9-BED3-505054503030} /failure:enable /success:enable</code>
5. Feche a **Política de Segurança Local**.
<br />   -- **As seguintes etapas são exigidas apenas para servidores de AD FS primários.** -- <br />
6. Abra o snap-in **Gerenciamento do AD FS** (no Gerenciador do Servidor, clique em Ferramentas e selecione Gerenciamento do AD FS).
7. No painel **Ações**, clique em **Editar Propriedades do Serviço de Federação**.
8. Na caixa de diálogo **Propriedades do Serviço de Federação**, clique na guia **Eventos**.
9. Marque as caixas de seleção **Auditorias com êxito e Auditorias com falha** e clique em **OK**. Isso deve ser habilitado por padrão.
10. Abra uma janela do PowerShell e execute o seguinte comando: ```Set-AdfsProperties -AuditLevel Verbose```.

Observe que o nível de auditoria "básico" é habilitado por padrão. Leia mais sobre o [aprimoramento de auditoria do AD FS no Windows Server 2016](https://docs.microsoft.com/windows-server/identity/ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server)


#### <a name="to-locate-the-ad-fs-audit-logs"></a>Para localizar os logs de auditoria do AD FS

1. Abra o **Visualizador de Eventos**.
2. Vá para Logs do Windows e selecione **Segurança**.
3. À direita, clique em **Filtrar Logs Atuais**.
4. Em Origem do Evento, selecione **Auditoria do AD FS**.

    E [Notas de perguntas frequentes](reference-connect-health-faq.md#operations-questions) sobre Logs de auditoria.

![Logs de auditoria do AD FS](./media/how-to-connect-health-agent-install/adfsaudit.png)

> [!WARNING]
> Uma política de grupo pode desabilitar a auditoria do AD FS. Se a auditoria do AD FS for desabilitada, a análise de uso sobre as atividades de logon não estará disponível. Verifique se você não tem uma política de grupo que desabilite a auditoria do AD FS.>
>


## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Instalando o agente do Azure AD Connect Health para sincronização

O agente do Azure AD Connect Health para sincronização é instalado automaticamente na compilação mais recente do Azure AD Connect. Para usar o Azure AD Connect para sincronização, você precisará baixar a versão mais recente do Azure AD Connect e instalá-lo. Você pode baixar a versão mais recente [aqui](https://www.microsoft.com/download/details.aspx?id=47594).

Para verificar se o agente foi instalado, procure os serviços a seguir no servidor. Se você tiver concluído a configuração, eles já deverão estar em execução. Caso contrário, eles ficarão parados até a conclusão da configuração.

* Serviço de Informações do Azure AD Connect Health para Sincronização
* Serviço de Monitoramento do Azure AD Connect Health para Sincronização

![Verificar sincronização do Azure AD Connect Health](./media/how-to-connect-health-agent-install/services.png)

> [!NOTE]
> Lembre-se de que usar o Azure AD Connect Health requer o Azure AD Premium. Se você não tiver o Azure AD Premium, você não poderá concluir a configuração no portal do Azure. Para saber mais, confira a [página de requisitos](how-to-connect-health-agent-install.md#requirements).
>
>

## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Registro manual do Azure AD Connect Health para Sincronização

Se o registro do agente do Azure AD Connect Health para Sincronização falhar após a instalação bem-sucedida do Azure AD Connect, você poderá usar o comando a seguir do PowerShell para registrar manualmente o agente.

> [!IMPORTANT]
> O uso desse comando do PowerShell só será necessário se o registro do agente falhar após a instalação do Azure AD Connect.
>
>

O comando a seguir do PowerShell é necessário somente quando o registro do agente de integridade falha mesmo após a instalação e a configuração bem-sucedidas do Azure AD Connect. Os serviços do Azure AD Connect Health não serão iniciados até que o agente seja registrado com êxito.

Você pode registrar manualmente o agente do Azure AD Connect Health para sincronização usando o seguinte comando do PowerShell:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

O comando usa os seguintes parâmetros:

* AttributeFiltering: $true (padrão) - se o Azure AD Connect não estiver sincronizando o conjunto de atributos padrão e tiver personalizado para usar um conjunto de atributos filtrados. Caso contrário, o valor será $false.
* StagingMode: $false (padrão) - se o servidor do Azure AD Connect NÃO estiver no modo de preparo; o valor será $true se o servidor estiver configurado para o modo de preparo.

Quando for solicitado a fazer a autenticação, você deverá usar a mesma conta do administrador global (como admin@domain.onmicrosoft.com) usada para configurar o Azure AD Connect.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Instalando o Agente do Azure AD Connect Health para AD DS

Para iniciar a instalação do agente, dê um clique duplo no arquivo .exe que você baixou. Na primeira tela, clique em instalar.

![Verifique o Azure AD Connect Health](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install1.png)

Depois que a instalação for concluída, clique em Configurar agora.

![Verifique o Azure AD Connect Health](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install2.png)

Um prompt de comando será iniciado, seguido pelo PowerShell, que executará Register-AzureADConnectHealthADDSAgent. Quando solicitado a entrar no Azure, continue e entre.

![Verifique o Azure AD Connect Health](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install3.png)

Depois de entrar, o PowerShell continuará. Depois de concluir, feche o PowerShell e a configuração estará concluída.

Neste ponto, os serviços devem ser iniciados automaticamente, permitindo que o agente monitore e colete dados. Se você não tiver atendido a todos os pré-requisitos descritos nas seções anteriores, serão exibidos avisos na janela do PowerShell. Não se esqueça de concluir os [requisitos](how-to-connect-health-agent-install.md#requirements) antes de instalar o agente. A captura de tela a seguir é um exemplo desses erros.

![Verificar o Azure AD Connect Health para AD DS](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install4.png)

Para verificar se o agente foi instalado, procure os serviços a seguir no controlador de domínio.

* Serviço do Insights do AD DS do Azure AD Connect Health
* Serviço de Monitoramento do AD DS do Azure AD Connect Health

Se você tiver concluído a configuração, esses serviços já deverão estar em execução. Caso contrário, eles ficarão parados até a conclusão da configuração.

![Verifique o Azure AD Connect Health](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install5.png)

### <a name="quick-agent-installation-in-multiple-servers"></a>Instalação rápida de agente em vários servidores

1. Crie uma conta de usuário no Azure AD com uma senha.
2. Atribua a função de **proprietário** para esta conta local do AAD em Azure ad Connect Health por meio do Portal. Siga as etapas [aqui](how-to-connect-health-operations.md#manage-access-with-role-based-access-control). Atribua a função a todas as instâncias de serviço. 
3. Baixe o arquivo MSI. exe no controlador de domínio local para instalação.
4. Execute o script a seguir no registro. Substitua os parâmetros pela nova conta de usuário criada e sua senha. 

```powershell
AdHealthAddsAgentSetup.exe /quiet
Start-Sleep 30
$userName = "NEWUSER@DOMAIN"
$secpasswd = ConvertTo-SecureString "PASSWORD" -AsPlainText -Force
$myCreds = New-Object System.Management.Automation.PSCredential ($userName, $secpasswd)
import-module "C:\Program Files\Azure Ad Connect Health Adds Agent\PowerShell\AdHealthAdds"
 
Register-AzureADConnectHealthADDSAgent -Credential $myCreds

```

1. Quando terminar, você poderá remover o acesso à conta local seguindo um ou mais destes procedimentos: 
    * Remover a atribuição de função para a conta local para o AAD Connect Health
    * Gire a senha da conta local. 
    * Desabilitar a conta local do AAD
    * Excluir a conta local do AAD  

## <a name="agent-registration-using-powershell"></a>Registro de agente usando o PowerShell

Depois de instalar o agente apropriado com setup.exe, você poderá executar a etapa de registro de agente usando os comandos do PowerShell a seguir, dependendo da função. Abra uma janela do PowerShell e execute o comando apropriado:

```powershell
    Register-AzureADConnectHealthADFSAgent
    Register-AzureADConnectHealthADDSAgent
    Register-AzureADConnectHealthSyncAgent

```

Esses comandos aceitam "Credencial" como um parâmetro para concluir o registro de forma não interativa ou em um computador Server-Core.
* A credencial pode ser capturada em uma variável do PowerShell que é passada como um parâmetro.
* Você pode fornecer qualquer identidade do Azure AD que tenha acesso para registrar os agentes e que NÃO tenha o MFA habilitado.
* Por padrão, administradores globais têm acesso para executar o registro do agente. Você também pode permitir que outras identidades menos privilegiadas executem esta etapa. Leia mais sobre [Controle de acesso com base em função](how-to-connect-health-operations.md#manage-access-with-role-based-access-control).

```powershell
    $cred = Get-Credential
    Register-AzureADConnectHealthADFSAgent -Credential $cred

```

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Configurar agentes do Azure AD Connect Health para usar HTTP Proxy

Você pode configurar agentes do Azure AD Connect Health para trabalhar com um HTTP Proxy.

> [!NOTE]
> * Não há suporte para "Netsh WinHttp set ProxyServerAddress", pois o agente usa System.Net para fazer solicitações da web em vez do Microsoft Windows HTTP Services.
> * O endereço do Http Proxy configurado é usado para mensagens de passagem criptografadas de Https.
> * Não há suporte para proxies autenticados (usando HTTPBasic).
>
>

### <a name="change-health-agent-proxy-configuration"></a>Alterar a configuração de proxy do agente de integridade

Você tem as seguintes opções para configurar o agente do Azure AD Connect Health para usar um HTTP Proxy.

> [!NOTE]
> Todos os serviços do agente do Azure AD Connect Health devem ser reiniciados para que as configurações de proxy sejam atualizadas. Execute o seguinte comando:<br />
> Restart-Service AzureADConnectHealth *
>
>

#### <a name="import-existing-proxy-settings"></a>Importar configurações de proxy existentes

##### <a name="import-from-internet-explorer"></a>Importar do Internet Explorer

As configurações de proxy HTTP do Internet Explorer podem ser importadas para serem usadas por agentes do Azure AD Connect Health. Em cada um dos servidores que executam o agente de integridade, execute o seguinte comando do PowerShell:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importar do WinHTTP

As configurações de proxy WinHTTP podem ser importadas para serem usadas por agentes do Azure AD Connect Health. Em cada um dos servidores que executam o agente de integridade, execute o seguinte comando do PowerShell:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Especificar endereços de Proxy manualmente

Você pode executar manualmente um servidor proxy em cada um dos servidores que estejam executando o Agente de Integridade ao executar o seguinte comando do PowerShell:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Exemplo: *Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress myproxyserver:443*

* "address" pode ser um nome do servidor DNS que pode ser resolvido ou um endereço IPv4
* "port" pode ser omitida. Se omitida, 443 é escolhida como a porta padrão.

#### <a name="clear-existing-proxy-configuration"></a>Limpar a configuração de proxy existente

Você pode limpar a configuração de proxy existente executando o comando a seguir:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Ler configurações de proxy atuais

Você pode ler as configurações de proxy atuais ao executar este comando:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Testar a conectividade com o serviço Azure AD Connect Health

É possível haver problemas que façam com que o agente do Azure AD Connect Health perca a conectividade com o serviço Azure AD Connect Health. Isso inclui problemas de rede, problemas de permissão, entre vários outros.

Se o agente não puder enviar dados para o serviço do Azure AD Connect Health por mais de duas horas, o seguinte alerta será indicado no portal: "Os dados do Serviço de Integridade não estão atualizados." Você pode confirmar se o agente do Azure AD Connect Health afetado consegue carregar dados no serviço Azure AD Connect Health ao executar o seguinte comando do PowerShell:

    Test-AzureADConnectHealthConnectivity -Role ADFS

O parâmetro de função usa os seguintes valores:

* ADFS
* Sincronizar
* ADICIONA

> [!NOTE]
> Para usar a ferramenta de conectividade, você deve primeiro concluir o registro do agente. Se conseguir concluir o registro do agente, verifique se você atende a todos os [requisitos](how-to-connect-health-agent-install.md#requirements) do Azure AD Connect Health. Esse teste de conectividade é executado por padrão durante o registro do agente.
>
>

## <a name="related-links"></a>Links relacionados

* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Operações de Azure AD Connect Health](how-to-connect-health-operations.md)
* [Usando o Azure AD Connect Health com o AD FS](how-to-connect-health-adfs.md)
* [Usando Azure AD Connect Health para sincronização](how-to-connect-health-sync.md)
* [Usar o Azure AD Connect Health com o AD DS](how-to-connect-health-adds.md)
* [Perguntas frequentes do Azure AD Connect Health](reference-connect-health-faq.md)
* [Histórico de versão do Azure AD Connect Health](reference-connect-health-version-history.md)
