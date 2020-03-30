---
title: Delegar funções por tarefa de administrador - Diretório Ativo do Azure | Microsoft Docs
description: Funções para delegar para tarefas de identidade no Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/03/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ac44661dd5a52ba19a3b2dd461aabec1ec250bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284867"
---
# <a name="administrator-roles-by-admin-task-in-azure-active-directory"></a>Funções de administrador por tarefa de administrador no Azure Active Directory

Neste artigo, você poderá encontrar as informações necessárias para restringir as permissões de administrador de um usuário, atribuindo funções com privilégios mínimos no Azure AD (Azure Active Directory). Você encontrará tarefas de administrador organizadas por área de recurso e a função com privilégios mínimos necessária para executar cada tarefa, além de funções de Administrador não global adicionais que podem executar a tarefa.

## <a name="application-proxy"></a>Proxy de aplicativo

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar aplicativo proxy do aplicativo | Administrador de aplicativos | 
Configurar propriedades do grupo de conectores | Administrador de aplicativos | 
Criar registro de aplicativo quando a capacidade estiver desabilitada para todos os usuários | Desenvolvedor de aplicativos | Administrador de aplicativos de nuvem, Administrador de aplicativos
Criar grupo de conectores | Administrador de aplicativos | 
Excluir grupo de conectores | Administrador de aplicativos | 
Desabilitar proxy de aplicativo | Administrador de aplicativos | 
Baixar serviço do conector | Administrador de aplicativos | 
Ler todas as configurações | Administrador de aplicativos | 

## <a name="b2c"></a>B2C

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Criar diretórios do Azure AD B2C | Todos os usuários não convidados ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Criar aplicativos B2C | Administrador global | 
Criar aplicativos corporativos | Administrador de Aplicativos de Nuvem | Administrador de aplicativos
Criar, ler, atualizar e excluir políticas de B2C | Administrador de políticas b2C IEF | 
Criar, ler, atualizar e excluir provedores de identidade | Administrador do Provedor de Identidade Externa | 
Criar, ler, atualizar e excluir fluxos de usuários de redefinição de senha | Administrador de fluxo de usuário B2C | 
Criar, ler, atualizar e excluir fluxos de usuários de edição de perfil | Administrador de fluxo de usuário B2C | 
Criar, ler, atualizar e excluir fluxos de usuários de entrada | Administrador de fluxo de usuário B2C | 
Criar, ler, atualizar e excluir fluxo de usuários de entrada |Administrador de fluxo de usuário B2C | 
Criar, ler, atualizar e excluir atributos de usuário | Administrador de atributo de fluxo de usuário B2C | 
Criar, ler, atualizar e excluir usuários | Administrador de usuários
Ler todas as configurações | Leitor global | 
Ler os logs de auditoria do B2C | Leitor global[(ver documentação)](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-faqs) | 

> [!NOTE]
> Os leitores azure AD B2C Global não têm as mesmas permissões que os administradores globais do Azure AD. Se você tiver privilégios de administrador global Azure AD B2C, certifique-se de que você está em um diretório Azure AD B2C e não em um diretório Azure AD.

## <a name="company-branding"></a>Identidade visual da empresa

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar identidade visual da empresa | Administrador global | 
Ler todas as configurações | Leitores de diretórios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="company-properties"></a>Propriedades da empresa

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar propriedades da empresa | Administrador global | 

## <a name="connect"></a>Conectar

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Autenticação de passagem | Administrador global | 
Ler todas as configurações | Leitor global | 
Logon único contínuo | Administrador global | 

## <a name="connect-health"></a>Connect Health

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Adicionar ou excluir serviços | Proprietário ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Aplicar correções para erro de sincronização | Colaborador ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Proprietário
Configurar notificações | Colaborador ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Proprietário
Definir configurações | Proprietário ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations)) | 
Configurar notificações de sincronização | Colaborador ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Proprietário
Ler os relatórios de segurança do ADFS | Leitor de segurança | Colaborador, Proprietário
Ler todas as configurações | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Ler os erros de sincronização | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Ler os serviços de sincronização | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Exibir métricas e alertas | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Exibir métricas e alertas | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário
Exibir métricas e alertas do serviço de sincronização | Leitor ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context)) | Colaborador, Proprietário

## <a name="custom-domain-names"></a>Nomes de domínio personalizados

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Gerenciar domínios | Administrador global | 
Ler todas as configurações | Leitores de diretórios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))

## <a name="domain-services"></a>Serviços de Domínio

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Criar instância do Azure AD Domain Services | Administrador global | 
Executar todas as tarefas do Azure Active Directory Domain Services | Grupo Administradores do Azure AD DC ([consulte a documentação](../../active-directory-domain-services/tutorial-create-management-vm.md#administrative-tasks-you-can-perform-on-an-azure-ad-ds-managed-domain)) | 
Ler todas as configurações | Leitor na assinatura do Azure que contém o serviço AD DS | 

## <a name="devices"></a>Dispositivos

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Desabilitar dispositivo | Administrador de dispositivo em nuvem | 
Habilitar dispositivo | Administrador de dispositivo em nuvem | 
Ler a configuração básica | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Ler as chaves do BitLocker | Leitor de segurança | Administrador de senhas, Administrador da segurança

## <a name="enterprise-applications"></a>Aplicativos empresariais

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Consentimento para quaisquer permissões delegadas | Administrador de aplicativos de nuvem | Administrador de aplicativos
Consentimento para permissões de aplicativos que não incluem o Microsoft Graph | Administrador de aplicativos de nuvem | Administrador de aplicativos
Consentimento para permissões de aplicativos para o Microsoft Graph | Administrador de função com privilégios | 
Consentimento para aplicativos acessando dados próprios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Criar aplicativos empresariais | Administrador de aplicativos de nuvem | Administrador de aplicativos
Gerenciar proxy de aplicativo | Administrador de aplicativos | 
Gerenciar configurações de usuário | Administrador global | 
Revisão de acesso de leitura de um grupo ou de um aplicativo | Leitor de segurança | Administrador de segurança, Administrador de usuários
Ler todas as configurações | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 
Atualizar atribuições de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar proprietários de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar propriedades de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar provisionamento de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar autoatendimento de aplicativos empresariais | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos
Atualizar propriedades de logon único | Proprietário de aplicativo empresarial ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador do aplicativo de nuvem, Administrador de aplicativos

## <a name="entitlement-management"></a>Gerenciamento de direitos
Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Adicionar recursos a um catálogo | Administrador de usuários | Com o gerenciamento de direitos, você pode delegar essa tarefa ao proprietário do catálogo[(veja documentação)](../governance/entitlement-management-catalog-create.md#add-additional-catalog-owners)
Adicionar sites SharePoint Online para catalogar | Administrador global


## <a name="groups"></a>Grupos

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Atribuir licença | Administrador de usuários | 
Criar grupo | Administrador de usuários | 
Criar, atualizar ou excluir a revisão de acesso de um grupo ou de um aplicativo | Administrador de usuários | 
Gerenciar expiração de grupo | Administrador de usuários | 
Gerenciar configurações de grupo | Administrador de grupos | Administrador de usuários | 
Ler todas as configurações (exceto associação oculta) | Leitores de diretórios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Ler associação oculta | Membro do grupo | Proprietário do grupo, administrador de senhas, administrador do Exchange, administrador do SharePoint, administrador de equipes, administrador de usuário
Ler membros de grupos com membros ocultos | Administrador de assistência técnica | Administrador de usuário, administrador de equipes
Revogar licença | Administrador de licenças | Administrador de usuários
Atualizar associação de grupo | Proprietário do grupo ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador de usuários
Atualizar proprietários do grupo | Proprietário do grupo ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador de usuários
Atualizar propriedades do grupo | Proprietário do grupo ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | Administrador de usuários

## <a name="identity-protection"></a>Identity Protection

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar notificações de alerta| Administrador de segurança | 
Configurar e habilitar ou desabilitar política de MFA| Administrador de segurança | 
Configurar e habilitar ou desabilitar política de risco de entrada| Administrador de segurança | 
Configurar e habilitar ou desabilitar política de risco do usuário | Administrador de segurança | 
Configurar resumos semanais | Administrador de segurança| 
Descarte todas as detecções de risco | Administrador de segurança | 
Corrigir ou ignorar vulnerabilidade | Administrador de segurança | 
Ler todas as configurações | Leitor de segurança | 
Leia todas as detecções de risco | Leitor de segurança | 
Ler vulnerabilidades | Leitor de segurança | 

## <a name="licenses"></a>Licenças

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Atribuir licença | Administrador de licenças | Administrador de usuários
Ler todas as configurações | Leitores de diretórios | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions))
Revogar licença | Administrador de licenças | Administrador de usuários
Experimentar ou comprar uma assinatura | Administrador de cobrança | 


## <a name="monitoring---audit-logs"></a>Monitoramento - Log de auditoria

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Ler logs de auditoria | Leitor de relatórios | Leitor de segurança, Administrador da segurança

## <a name="monitoring---sign-ins"></a>Monitoramento - Entradas

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Ler logs de entrada | Leitor de relatórios | Leitor de segurança, Administrador da segurança

## <a name="multi-factor-authentication"></a>Autenticação multifator

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Excluir todas as senhas de aplicativos existentes geradas pelos usuários selecionados | Administrador global | 
Desabilitar MFA | Administrador global | 
Habilitar MFA | Administrador global | 
Gerenciar configurações do serviço de MFA | Administrador global | 
Exigir que os usuários selecionados forneçam métodos de contato novamente | Administrador de Autenticação | 
Restaurar a autenticação multifator em todos os dispositivos lembrados  | Administrador de Autenticação | 

## <a name="mfa-server"></a>Servidor MFA

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Bloquear/desbloquear usuários | Administrador global | 
Configurar bloqueio de conta | Administrador global | 
Configurar regras de cache | Administrador global | 
Configurar alerta de fraude | Administrador global
Configurar notificações | Administrador global | 
Configurar bypass avulso | Administrador global | 
Definir configurações de chamada telefônica | Administrador global | 
Configurar provedores | Administrador global | 
Definir configurações do servidor | Administrador global | 
Ler relatório de atividades | Leitor global | 
Ler todas as configurações | Leitor global | 
Ler o status do servidor | Leitor global |  

## <a name="organizational-relationships"></a>Relações organizacionais

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Gerenciar provedores de identidade | Administrador do Provedor de Identidade Externa | 
Gerenciar configurações | Administrador global | 
Gerenciar termos de uso | Administrador global | 
Ler todas as configurações | Leitor global | 

## <a name="password-reset"></a>Redefinição de senha

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar métodos de autenticação | Administrador global |
Configurar personalização | Administrador global |
Configurar notificação | Administrador global |
Configurar integração local | Administrador global |
Configurar propriedades de redefinição de senha | Administrador de usuários | Administrador global
Configurar registro | Administrador global |
Ler todas as configurações | Administrador de segurança | Administrador de usuários |

## <a name="privileged-identity-management"></a>Privileged Identity Management

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Atribuir usuários a funções | Administrador de função com privilégios | 
Definir configurações de função | Administrador de função com privilégios | 
Exibir atividade de auditoria | Leitor de segurança | 
Exibir associações de função | Leitor de segurança | 

## <a name="roles-and-administrators"></a>Funções e administradores

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Gerenciar atribuições de função | Administrador de função com privilégios | 
Revisão de acesso de leitura de uma função do Azure AD  | Leitor de segurança | Administrador da segurança, Administrador de função com privilégios
Ler todas as configurações | Função de usuário padrão ([consulte a documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions)) | 

## <a name="security---authentication-methods"></a>Segurança - Métodos de Autenticação

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar métodos de autenticação | Administrador global | 
Ler todas as configurações | Leitor global | 

## <a name="security---conditional-access"></a>Segurança - Acesso Condicional

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Configurar endereços IP confiáveis de MFA | Administrador de acesso condicional | 
Criar controles personalizados | Administrador de acesso condicional | Administrador de segurança
Criar locais nomeados | Administrador de acesso condicional | Administrador de segurança
Criar políticas | Administrador de acesso condicional | Administrador de segurança
Criar termos de uso | Administrador de acesso condicional | Administrador de segurança
Criar certificado de conectividade VPN | Administrador de acesso condicional | Administrador de segurança
Excluir política clássica | Administrador de acesso condicional | Administrador de segurança
Excluir termos de uso | Administrador de acesso condicional | Administrador de segurança
Excluir certificado de conectividade VPN | Administrador de acesso condicional | Administrador de segurança
Desabilitar política clássica | Administrador de acesso condicional | Administrador de segurança
Gerenciar controles personalizados | Administrador de acesso condicional | Administrador de segurança
Gerenciar locais nomeados | Administrador de acesso condicional | Administrador de segurança
Gerenciar termos de uso | Administrador de acesso condicional | Administrador de segurança
Ler todas as configurações | Leitor de segurança | Administrador de segurança
Ler localizações nomeadas | Leitor de segurança | Administrador de Acesso Condicional, administrador de segurança

## <a name="security---identity-security-score"></a>Segurança - Pontuação de segurança de identidade

Tarefa | Função com privilégios mínimos | Funções adicionais | 
---- | --------------------- | ----------------
Ler todas as configurações | Leitor de segurança | Administrador de segurança
Ler pontuação de segurança | Leitor de segurança | Administrador de segurança
Atualizar status do evento | Administrador de segurança | 

## <a name="security---risky-sign-ins"></a>Segurança - Entradas arriscadas

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Ler todas as configurações | Leitor de segurança | 
Ler as entradas arriscadas | Leitor de segurança | 

## <a name="security---users-flagged-for-risk"></a>Segurança - Usuários sinalizados para risco

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Descartar todos os eventos | Administrador de segurança | 
Ler todas as configurações | Leitor de segurança | 
Ler usuários sinalizados para risco | Leitor de segurança | 

## <a name="users"></a>Usuários

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Adicionar usuário à função do diretório | Administrador de função com privilégios | 
Adicionar usuário ao grupo | Administrador de usuários | 
Atribuir licença | Administrador de licenças | Administrador de usuários
Criar um usuário convidado | Emissor do convite ao convidado | Administrador de usuários
Criar usuário | Administrador de usuários | 
Excluir usuários | Administrador de usuários | 
Invalidar tokens de atualização de administradores limitados (consulte a documentação) | Administrador de usuários | 
Invalidar tokens de atualização de não administradores (consulte a documentação) | Administrador de senha | Administrador de usuários
Invalidar tokens de atualização de administradores com privilégios (consulte a documentação) | Administrador de autenticação privilegiado | 
Ler a configuração básica | Função de usuário padrão[(veja documentação](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions) | 
Redefinir senha para administradores limitados (consulte a documentação) | Administrador de usuários | 
Redefinir senha de não administradores (consulte a documentação) | Administrador de senha | Administrador de usuários
Redefinir senha de administradores com privilégios | Administrador de autenticação privilegiado | 
Revogar licença | Administrador de licenças | Administrador de usuários
Atualizar todas as propriedades, exceto Nome UPN | Administrador de usuários | 
Atualizar nome UPN para administradores limitados (consulte a documentação) | Administrador de usuários | 
Atualizar a propriedade do nome UPN em administradores com privilégios (consulte a documentação) | Administrador global | 
Atualizar configurações do usuário | Administrador global | 


## <a name="support"></a>Suporte

Tarefa | Função com privilégios mínimos | Funções adicionais
---- | --------------------- | ----------------
Enviar tíquete de suporte | Administrador de serviços | Administrador de aplicativos, administrador de proteção de informações do Azure, administrador de faturamento, administrador de aplicativos em nuvem, administrador de conformidade, administrador dinâmico 365, administrador de análisede desktop, administrador de exchange, senha Administrador, Administrador intune, Skype para administrador de empresas, administrador de BI de energia, administrador de autenticação privilegiada, administrador de sharePoint, administrador de comunicações de equipes, administrador de equipes, administrador de usuário, Administrador de Análises de Local de Trabalho

## <a name="next-steps"></a>Próximas etapas

* [Como atribuir ou remover funções de administrador azure AD](directory-manage-roles-portal.md)
* [Referência das funções de administrador do Azure AD](directory-assign-admin-roles.md)
