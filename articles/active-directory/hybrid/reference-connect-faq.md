---
title: Perguntas frequentes sobre o Azure Active Directory | Microsoft Docs
description: Este artigo responde perguntas frequentes sobre o Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 08/23/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5aa30bc819531ee8cc9cd337648a6cbc661bb29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77149806"
---
# <a name="azure-active-directory-connect-faq"></a>Perguntas frequentes do Azure Active Directory Connect

## <a name="general-installation"></a>Instalação geral

**P: Como posso endurecer meu servidor Azure AD Connect para diminuir a superfície de ataque de segurança?**

A Microsoft recomenda endurecer o servidor Azure AD Connect para diminuir a superfície de ataque de segurança para este componente crítico do ambiente de TI.  Seguir as recomendações abaixo diminuirá os riscos de segurança para sua organização.

* Implantar o Azure AD Connect em um servidor com um domínio e restringir o acesso administrativo a administradores de domínio ou outros grupos de segurança fortemente controlados

Para obter mais informações, consulte: 

* [Protegendo grupos de administradores](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-g--securing-administrators-groups-in-active-directory)

* [Protegendo contas de administrador incorporadas](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-d--securing-built-in-administrator-accounts-in-active-directory)

* [Melhoria da segurança e sustentação reduzindo superfícies de ataque](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#2-reduce-attack-surfaces )

* [Reduzindo a superfície de ataque do Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface)

**P: instalação funcionará se o Administrador Global do Azure Active Directory (AD do Azure) tem a autenticação de dois fatores (2FA) habilitada?**  
A partir das compilações de fevereiro de 2016, este cenário é suportado.

**P: existe uma forma de instalar o Azure Connect AD autônomo?**  
Instalação do Azure AD Connect é suportada apenas quando você usa o Assistente de instalação. Não há suporte para uma instalação silenciosa e autônoma.

**P: Eu tenho uma floresta em que um domínio não pode ser contatado. Como instalo o Azure AD Connect?**  
A partir das compilações de fevereiro de 2016, este cenário é suportado.

**P: o que o Azure Active Directory Domain Services funcione de agente de integridade do (AD DS Azure) no server core?**  
Sim. Depois de instalar o agente, você pode concluir o processo de registro usando o seguinte cmdlet do PowerShell: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**P: O Azure AD Connect suporta sincronização de dois domínios para um Azure AD?**  
Sim, há suporte para esse cenário. Consulte [Vários domínios](how-to-connect-install-multiple-domains.md).
 
**P: Você pode ter vários conectores para o mesmo domínio do Active Directory no Azure AD Connect?**  
Não há suporte para nenhum, vários conectores para o mesmo domínio do AD. 

**P: Posso mover o banco de dados Azure AD Connect do banco de dados local para uma instância remota do SQL Server?**   
Sim, as etapas a seguir fornecem diretrizes gerais sobre como fazer isso. Estamos trabalhando atualmente em um documento mais detalhado.
1. Fazer backup de banco de dados ADSync do LocalDB.
A maneira mais simples de fazer isso é usar o SQL Server Management Studio instalado no mesmo computador que o Azure AD Connect. Conecte-se a *(LocalDb).\ADSync*e, em seguida, fazer backup do banco de dados ADSync.

2. Restaurar o banco de dados "ADSync" para a instância remota do SQL.

3. Instalar o Azure AD Connect no existente [banco de dados SQL remoto](how-to-connect-install-existing-database.md).
   O artigo demonstra como migrar para o uso de um banco de dados SQL local. Se você estiver migrando para um banco de dados SQL remoto, na etapa 5 do processo, também será necessário inserir uma conta de serviço existente na qual o serviço Windows Sync será executado. Essa conta de serviço do mecanismo de sincronização é descrita aqui:
   
      **Use uma conta de serviço existente**: Por padrão, o Azure AD Connect usa uma conta de serviço virtual para os serviços de sincronização a serem usados. Se você usa uma instância remota do SQL Server ou usa um proxy que exija autenticação, use uma conta de serviço gerenciado ou uma conta de serviço no domínio e saber a senha. Nesses casos, insira a conta a usar. Certifique-se de que os usuários que estão executando a instalação sejam administradores de sistema no SQL, para que as credenciais de login da conta de serviço possam ser criadas. Para obter mais informações, consulte [contas e permissões do Azure AD Connect](reference-connect-accounts-permissions.md#adsync-service-account). 
   
      Com a compilação mais recente, o provisionamento do banco de dados agora pode ser executado fora da banda pelo administrador do SQL e, em seguida, instalado pelo administrador do Azure AD Connect com direitos de proprietário de banco de dados. Para obter mais informações, confira [Instale o Azure AD Connect usando permissões de administrador do SQL delegado](how-to-connect-install-sql-delegation.md).

Para manter as coisas simples, é recomendável que os usuários que instalam o Azure AD Connect ser administradores do sistema no SQL. No entanto, com compilações recentes, agora é possível usar administradores SQL delegados, conforme descrito em [ Instalar o Azure AD Connect usando permissões de administrador delegadas do SQL ](how-to-connect-install-sql-delegation.md).

**P: Quais são algumas das melhores práticas do campo?**  

A seguir, um documento informativo que apresenta algumas das melhores práticas que a engenharia, o suporte e nossos consultores desenvolveram ao longo dos anos.  Isso é apresentado em uma lista de balas que pode ser rapidamente referenciada.  Embora esta lista tente ser abrangente, pode haver práticas recomendadas adicionais que podem não ter chegado à lista ainda.

- Se usar SQL completo, então ele deve permanecer local vs. remoto
    - Menos lúpulos
    - Mais fácil de solucionar problemas
    - Menos complexidade
    - Precisa designar recursos para o SQL e permitir sobrecarga para o Azure AD Connect e o OS
- Ignorar proxy se possível, se você não puder contornar o proxy, então você precisa garantir que o valor de tempo é maior que 5 minutos.
- Se o proxy for necessário, então você deve adicionar o proxy ao arquivo machine.config
- Esteja ciente dos trabalhos e manutenção locais da SQL e como eles impactarão o Azure AD Connect - particularmente a reindexação
- Certifique-se de que o DNS pode resolver externamente
- Certifique-se de que [as especificações do servidor](how-to-connect-install-prerequisites.md#hardware-requirements-for-azure-ad-connect) são por recomendação se você está usando servidores físicos ou virtuais
- Certifique-se de que se você estiver usando um servidor virtual que os recursos necessários são dedicados
- Certifique-se de que a configuração de disco e disco atenda às práticas recomendadas para o SQL Server
- Instale e configure o Azure AD Connect Health para monitoramento
- Use o Limite de exclusão incorporado no Azure AD Connect.
- Revise cuidadosamente as atualizações de versão para estar preparado para todas as alterações e novos atributos que podem ser adicionados
- Backup de tudo
    - Chaves de backup
    - Regras de sincronização de backup
    - Configuração do servidor de backup
    - Fazer Backup de Banco de Dados SQL
- Certifique-se de que não há agentes de backup de terceiros que estejam fazendo backup do SQL sem o SQL VSS Writer (comum em servidores virtuais com instantâneos de terceiros)
- Limitar a quantidade de regras de sincronização personalizadas que são usadas à medida que adicionam complexidade
- Trate os servidores de conexão ad do Azure como servidores de nível 0
- Seja cauteloso em modificar regras de sincronização de nuvem sem grande compreensão do impacto e dos drivers de negócios certos
- Certifique-se de que as portas de URL e Firewall corretas estão abertas para suporte ao Azure AD Connect e ao Azure AD Connect Health
- Aproveite o atributo filtrado na nuvem para solucionar problemas e evitar objetos fantasmas
- Com o Servidor de preparação certifique-se de que você está usando o Documenter de configuração de conexão Azure AD para obter consistência entre servidores
- Servidores de preparação devem estar em data centers separados (Locais Físicos
- Servidores de preparação não são feitos para ser uma solução de alta disponibilidade, mas você pode ter vários servidores de preparação
- A introdução de um "Lag" Staging Servers poderia mitigar algum tempo de inatividade potencial em caso de erro
- Teste e valide todas as atualizações no Servidor de Preparação primeiro
- Valide sempre as exportações antes de passar para o servidor de preparação.  Aproveite o servidor de preparação para importações completas e sincronizações completas para reduzir o impacto nos negócios
- Mantenha a consistência da versão entre os servidores azure AD Connect o máximo possível 

**P: Posso permitir que o Azure AD Connect crie a conta do Azure AD Connector na máquina workgroup?**
Não.  Para permitir que o Azure AD Connect crie automaticamente a conta Azure AD Connector, a máquina deve ser aderida pelo domínio.  

## <a name="network"></a>Rede
**P: Eu tenho um firewall, dispositivo de rede, ou outra coisa que limita o tempo que as conexões podem ficar abertas na minha rede. Qual deve ser o limite de tempo limite do lado do cliente quando uso o Azure AD Connect?**  
Todos os softwares de rede, dispositivos físicos ou qualquer outra coisa que limite o tempo máximo que as conexões podem permanecer abertas deve usar um limiar de pelo menos 300 minutos (300 segundos) para conectividade entre o servidor no qual o cliente do Azure AD Connect está instalado e o Active Directory do Azure. Esta recomendação também se aplica a todas as ferramentas de sincronização do Microsoft Identity lançadas anteriormente.

**P: Os SLDs (domínios de rótulo único) têm suporte?**  
Embora seja altamente não usar essa configuração de rede ([consulte o artigo](https://support.microsoft.com/help/2269810/microsoft-support-for-single-label-domains)), o uso da sincronização do Azure AD Connect com um único domínio de rótulo é compatível, desde que a configuração de rede para o domínio de nível único esteja funcionando corretamente.

**P: as florestas com domínios do AD não contíguos têm suporte?**  
Não, o Azure AD Connect não oferece suporte a florestas locais que contêm namespaces contíguos.

**P: Há suporte a nomes NetBios com pontos?**  
 Não, o Azure AD Connect não oferece suporte a florestas/domínios locais em que o nome NetBios contém um ponto (.) no nome.

**P: há suporte para o ambiente puro do IPv6?**  
Não, o Azure AD Connect não oferece suporte ao ambiente puro do IPv6.

**P:I tem um ambiente multiflorestal e a rede entre as duas florestas está usando NAT (Network Address Translation). O uso do Azure AD Connect entre essas duas florestas é suportado?**</br>
Não, o uso do Azure AD Connect via NAT não é suportado. 

## <a name="federation"></a>Federação
**P: O que devo fazer se receber um email me pedindo para renovar o certificado do Office 365?**  
Para obter orientação sobre como renovar o certificado, consulte [renovar certificados](how-to-connect-fed-o365-certs.md).

**P: Eu tenho "Atualizar automaticamente o partido de confiar" definido para o Office 365 que confia no partido. Tenho que tomar alguma ação quando meu certificado de assinatura de token rolar automaticamente?**  
Use as diretrizes descritas no artigo [Renovar certificados](how-to-connect-fed-o365-certs.md).

## <a name="environment"></a>Ambiente
**P: É possível renomear o servidor após a instalação do Azure AD Connect?**  
Não. Alterar o nome do servidor torna o mecanismo de sincronização incapaz de se conectar à instância do banco de dados SQL e o serviço não pode ser iniciado.

**P: As regras de sincronização criptográfica (NGC) de próxima geração são suportadas em uma máquina habilitada para FIPS?**  
Não.  Não há suporte para isso.

**P. Se eu desabilitei um dispositivo sincronizado (por exemplo: HAADJ) no portal Azure, por que ele é reativado?**<br>
Dispositivos sincronizados podem ser de autoria ou domínio no local. Se um dispositivo sincronizado estiver ativado no local, ele poderá ser reativado no portal Azure, mesmo que tenha sido desativado anteriormente por um administrador. Para desativar um dispositivo sincronizado, use o Active Directory no local para desativar a conta do computador.

**P. Se eu bloquear o login do usuário no portal Office 365 ou Azure AD para usuários sincronizados, por que ele é desbloqueado ao fazer login novamente?**<br>
Os usuários sincronizados podem ser de autoria ou domínio no local. Se a conta estiver habilitada no local, poderá desbloquear o bloco de login colocado pelo administrador.

## <a name="identity-data"></a>Dados de identidade
**P: por que o atributo userPrincipalName (UPN) do Azure AD não corresponde o UPN local?**  
Para obter informações, consulte estes artigos:

* [Os nomes de usuário no Office 365, Azure ou Intune não coincidem com o UPN local ou a ID de logon alternativa](https://support.microsoft.com/kb/2523192)
* [As alterações não são sincronizadas pela ferramenta de sincronização do Azure Active Directory depois que você altera o UPN de uma conta de usuário para usar um domínio federado diferente](https://support.microsoft.com/kb/2669550)

Você também pode configurar o Azure AD para permitir que o mecanismo de sincronização atualize da forma descrita em [Azure AD Connect sync service features (Recursos do serviço de sincronização do Azure AD Connect)](how-to-connect-syncservice-features.md).

** P: É compatível com a compatibilidade de um grupo de AD do Azure local ou com um objeto de contato existente do Azure AD ou um objeto de contato? **  
Sim, essa correspondência flexível se baseará o proxyAddress. A correspondência suave não tem suporte para grupos que não são habilitados para email.

**P: Há suporte para o atributo ImmutableId definido manualmente em objetos de Grupo/Contato existentes do Azure AD para correspondência rígida com objetos de Grupo/Contato do AD locais?**  
Não, definindo manualmente o atributo de ImmutableId em um objeto de contato/grupo do AD do Azure existente para correspondência de disco rígida no momento não há suporte.

## <a name="custom-configuration"></a>Configuração personalizada
**P: Onde os cmdlets do PowerShell para o Azure AD Connect estão documentados?**  
 Com exceção dos cmdlets documentados neste site, não há suporte para outros cmdlets do PowerShell encontrados no Azure AD Connect para uso do cliente.

**P: Posso usar a opção "Exportação do servidor/importação do servidor" encontrada no Synchronization Service Manager para mover a configuração entre servidores?**  
Não. Essa opção não irá recuperar todos os parâmetros de configuração e não deve ser usada. Em vez disso, use o assistente para criar a configuração básica no segundo servidor e usar o editor de regra de sincronização para gerar scripts do PowerShell para mover qualquer regra personalizada entre servidores. Para mais informações, consulte [Migração Swing](how-to-upgrade-previous-version.md#swing-migration).

**P: As senhas podem ser armazenadas em cache para a página de entrada do Azure e esse cache pode ser impedido porque contém um elemento de entrada de senha com o atributo *autocomplete = "false"*?**  
Atualmente, modificar os atributos HTML do campo de entrada de **senha**, incluindo a marca de preenchimento automático, não há suporte. Estamos trabalhando em um recurso que permitirá Javascript personalizado, e permitirá que você adicione qualquer atributo ao campo de **Senha**.

**P: A página de login do Azure exibe os nomes de usuário dos usuários que já fizeram login com sucesso. Esse comportamento pode ser desligado?**  
Atualmente, não é suportado o modificação dos atributos HTML do campo de entrada **Password,** incluindo a tag autocompletar. Estamos trabalhando em um recurso que permitirá Javascript personalizado, e permitirá que você adicione qualquer atributo ao campo de **Senha**.

**P: Existe uma maneira de evitar sessões simultâneas?**  
Não.

## <a name="auto-upgrade"></a>Atualização automática

**P: quais são as vantagens e as consequências do uso da atualização automática?**  
É recomendável que todos os clientes habilitem a atualização automática para a instalação do Azure AD Connect. Os benefícios são que eles sempre receba os patches mais recentes, incluindo atualizações de segurança de vulnerabilidades que foram encontradas no Azure AD Connect. O processo de atualização é simples e ocorrerá automaticamente assim que uma nova versão estiver disponível. Muitos milhares de clientes do Azure AD Connect usam a atualização automática com cada nova versão.

O processo de atualização automática sempre primeiro estabelece se uma instalação está qualificada para atualização automática. Se for qualificado, a atualização é executada e testada. O processo também inclui procurando alterações personalizadas para regras e fatores ambientais específicos. Se os testes mostram que uma atualização é bem-sucedida, a versão anterior é restaurada automaticamente.

Dependendo do tamanho do ambiente, o processo pode levar algumas horas. Enquanto a atualização está em andamento, acontece sem sincronização entre o Windows Server Active Directory e o Azure AD.

**P: Recebi um e-mail dizendo que minha atualização automática não funciona mais e preciso instalar uma nova versão. Por que eu preciso fazer isso?**  
No ano passado, lançamos um versão do Azure AD Connect que, em algumas situações, pode ter desabilitado o recurso de atualização automática no seu servidor. Corrigimos o problema no Azure AD Connect versão 1.1.750.0. Se você tiver sido afetado pelo problema, poderá atenuá-lo executando um script do PowerShell para repará-lo ou atualizando manualmente para a versão mais recente do Azure AD Connect. 

Para executar o script do PowerShell, [ baixe o script ](https://aka.ms/repairaadconnect) e execute-o no servidor do Azure AD Connect em uma janela administrativa do PowerShell. Para saber como executar o script [exibir este vídeo rápido](https://aka.ms/repairaadcau).

Para atualizar manualmente, é necessário baixar e executar a versão mais recente do arquivo AADConnect.msi.
 
-  Se sua versão atual for anterior à 1.1.750.0, [Baixe e atualize para a versão mais recente](https://www.microsoft.com/download/details.aspx?id=47594).
- Se sua versão do Azure AD Connect for 1.1.750.0 ou posterior, nenhuma ação adicional é necessária. Você já está usando a versão que contém a correção de atualização automática. 

**P: Recebi um e-mail me dizendo para atualizar para a versão mais recente para reativar a atualização automática. Estou usando a versão 1.1.654.0. Preciso atualizar?**  
Sim, você precisa atualizar para a versão 1.1.750.0 ou posterior para habilitar novamente a atualização automática. [Faça o download e atualize para a versão mais recente](https://www.microsoft.com/download/details.aspx?id=47594).

**P: Recebi um e-mail me dizendo para atualizar para a versão mais recente para reativar a atualização automática. Se eu usei o PowerShell para ativar o upgrade automático, ainda preciso instalar a versão mais recente?**  
Sim, você ainda precisa atualizar para a versão 1.1.750.0 ou mais recente. Habilitar o serviço de atualização automática com o PowerShell não atenuará o problema encontrado na versão 1.1.750.0 e anteriores.

**P: Eu quero atualizar para uma versão mais recente, mas não tenho certeza de quem instalou o Azure AD Connect, e não temos o nome de usuário e senha. Precisamos disso?**
Você não precisa saber o nome de usuário e a senha que foi usada inicialmente para atualizar o Azure AD Connect. Use qualquer conta do Azure AD que tenha a função de Administrador Global.

**P: Como posso descobrir a versão do Azure AD Connect que estou usando?**  
Para verificar qual versão do Azure AD Connect está instalada no seu servidor, vá para o Painel de Controle e procure a versão instalada do Microsoft Azure AD Connect selecionando**Programas e Recursos**de **Programas,** > como mostrado aqui:

![Versão do Azure AD Connect no Painel de Controle](./media/reference-connect-faq/faq1.png)

**P: como fazer upgrade para a versão mais recente do Azure AD Connect?**  
Para saber como atualizar para a versão mais recente, consulte [do Azure AD Connect: atualização de uma versão anterior para a versão mais recente](how-to-upgrade-previous-version.md). 

**P: Nós já atualizamos para a versão mais recente do Azure AD Connect no ano passado. Precisamos melhorar de novo?**  
A equipe do Azure AD Connect faz atualizações frequentes para o serviço. Para se beneficiar de correções de bugs e atualizações de segurança, bem como novos recursos, é importante manter o servidor atualizado com a versão mais recente. Se você habilitar a atualização automática, sua versão do software é atualizado automaticamente. Para localizar o histórico de versão do Azure AD Connect, consulte [do Azure AD Connect: histórico de versão](reference-connect-version-history.md).

**P: quanto tempo demora para realizar a atualização, e qual é o impacto sobre meus usuários?**  
O tempo necessário para atualizar depende do tamanho do seu locatário. Para organizações maiores, pode ser melhor realizar a atualização à noite ou no fim de semana. Durante a atualização nenhuma atividade de sincronização ocorre.

**P: Acredito que fiz upgrade para o Azure AD Connect, mas o portal do Office ainda menciona DirSync. Por que isso?**  
A equipe do Office está trabalhando para receber atualizações de portal do Office para refletir o nome do produto atual. Ele não reflete a ferramenta de sincronização que você está usando.

**P: Meu status de atualização automática diz: "Suspenso". Por que está suspenso? Devo habilitá-lo?**  
Um bug foi introduzido em uma versão anterior que, em determinadas circunstâncias, deixa o status de atualização automática definido como "Suspenso". Manualmente, permitindo que ele seja tecnicamente possível, mas exige várias etapas complexas. A melhor coisa que você pode fazer é instalar a versão mais recente do Azure AD Connect.

**P: Minha empresa tem rigorosos requisitos de gerenciamento de mudanças, e eu quero controlar quando for empurrada para fora. Posso controlar quando a atualização automática é lançada?**  
Não há nenhum desses recursos hoje mesmo. O recurso está sendo avaliado para um lançamento futuro.

**P: Receberei um e-mail se a atualização automática falhar? Como saberei que foi bem sucedido?**  
Você não será notificado do resultado da atualização. O recurso está sendo avaliado para um lançamento futuro.

**P: você publica uma linha do tempo para quando você planeja enviar por push atualizações automáticas?**  
Atualização automática é a primeira etapa no processo de lançamento de uma versão mais recente. Sempre que houver uma nova versão, as atualizações são enviadas automaticamente. As versões mais recentes do Azure AD Connect são pré-anunciadas no [Roteiro do Azure AD](../fundamentals/whats-new.md).

**P: Atualização automática também atualiza o Azure AAD Connect Health?**  
Sim, atualização automática também atualiza o Azure AAD Connect Health.

**P: você também a atualização automática do Azure AD Connect servidores no modo de preparo?**  
Sim, você pode atualização automática um servidor de conexão do AD do Azure que está no modo de preparo.

**P: se a falha de atualização automática e meu servidor do Azure AD Connect não for iniciado, o que devo fazer?**  
Em casos raros, o serviço do Azure AD Connect não inicia após executar a atualização. Nesses casos, reinicialize o servidor, que geralmente corrige o problema. Se o serviço do Azure AD Connect não iniciar, abra um tíquete de suporte. Para obter mais informações, consulte [criar uma solicitação de serviço, entre em contato com o suporte do Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**P: Eu não tenho certeza quais são os riscos quando eu atualizar para uma versão mais recente do Azure AD Connect. Pode me ligar para me ajudar com a atualização?**  
Se você precisar de ajuda a atualizar para uma versão mais recente do Azure AD Connect, abra um tíquete de suporte em [criar uma solicitação de serviço, entre em contato com o suporte do Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/).

## <a name="troubleshooting"></a>Solução de problemas
**P: Como posso obter ajuda com o Azure AD Connect?**

[Pesquise a Base de Dados de Conhecimento (KB) Microsoft](https://www.microsoft.com/en-us/search/result.aspx?q=azure+active+directory+connect)

* Pesquise a KB para soluções técnicas de conserto de problemas comuns sobre o suporte do Azure AD Connect.

[Perguntas frequentes sobre o Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Pesquisar perguntas e respostas técnicas ou fazer suas próprias perguntas, acessando [a comunidade do Azure AD](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Obter suporte para o Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

**P: Por que estou vendo os eventos 6311 e 6401 ocorrerem após erros de passo de sincronização?**

Os eventos 6311 - **O servidor encontrou um erro inesperado ao realizar um retorno de chamada** e 6401 - o controlador do agente de gerenciamento encontrou um erro **inesperado** - são sempre registrados após um erro de etapa de sincronização. Para resolver esses erros, você precisa limpar os erros da etapa de sincronização.  Para obter mais informações, consulte [Erros de solução de problemas durante a sincronização de sincronização](tshoot-connect-sync-errors.md) e [solucionar problemas com a sincronização do Azure AD Connect](tshoot-connect-objectsync.md)
