---
title: Autenticar-se no Azure Key Vault
description: Saiba como autenticar com o Azure Key Vault
author: ShaneBala-keyvault
ms.author: sudbalas
ms.date: 08/27/2020
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.openlocfilehash: d247e657e93afd0c43ecee1154c542398304d8dd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89481368"
---
# <a name="authenticate-to-azure-key-vault"></a>Autenticar-se no Azure Key Vault

O Azure Key Vault permite armazenar segredos e controlar a distribuição deles em um repositório de nuvem centralizado e seguro, o que elimina a necessidade de armazenar credenciais em aplicativos. Os aplicativos precisam apenas se autenticar com o Key Vault em tempo de execução para acessarem esses segredos.

## <a name="app-identity-and-security-principals"></a>Entidades de segurança e identidade do aplicativo

A autenticação com o Key Vault funciona em conjunto com o [Azure AD (Azure Active Directory)](/azure/active-directory/fundamentals/active-directory-whatis), que é responsável por autenticar a identidade de qualquer **entidade de segurança**.

Uma entidade de segurança é um objeto que representa um usuário, grupo, serviço ou aplicativo que está solicitando acesso aos recursos do Azure. O Azure atribui uma **ID do objeto** exclusiva a cada entidade de segurança.

* Uma entidade de segurança de **usuário** identifica um indivíduo que tem um perfil no Azure Active Directory.

* Uma entidade de segurança de **grupo** identifica um conjunto de usuários criado no Azure Active Directory. Todas as funções ou permissões atribuídas ao grupo são concedidas a todos os usuários dentro do grupo.

* Uma **entidade de serviço** é um tipo de entidade de segurança que identifica um aplicativo ou serviço, ou seja, um trecho de código, em vez de um usuário ou grupo. A ID do objeto de uma entidade de serviço é conhecida como a **ID do cliente** dessa entidade de serviço e atua como o nome de usuário dela. O **segredo do cliente** da entidade de serviço funciona como a senha dela.

Para aplicativos, há duas maneiras de obter uma entidade de serviço:

* Recomendado: habilitar uma **Identidade gerenciada** atribuída pelo sistema para o aplicativo.

    Com a identidade gerenciada, o Azure gerencia internamente a entidade de serviço do aplicativo e autentica automaticamente o aplicativo com outros serviços do Azure. A identidade gerenciada está disponível para aplicativos implantados em uma variedade de serviços.

    Para obter mais informações, confira [Visão geral da identidade gerenciada](/azure/active-directory/managed-identities-azure-resources/overview). Confira também [Serviços do Azure que dão suporte à identidade gerenciada](/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities), que contém links para artigos que descrevem como habilitar a identidade gerenciada para serviços específicos (como Serviço de Aplicativo, Azure Functions, Máquinas Virtuais etc.).

* Se não for possível usar a identidade gerenciada, você pode, em vez disso, **registrar** o aplicativo com seu locatário do Azure AD, conforme descrito em [Início rápido: Registrar um aplicativo na plataforma de identidade do Azure](/azure/active-directory/develop/quickstart-register-app). O registro também cria um segundo objeto de aplicativo que identifica o aplicativo em todos os locatários.

## <a name="authorize-a-security-principal-to-access-key-vault"></a>Autorizar uma entidade de segurança a acessar o Key Vault

O Key Vault funciona com dois níveis de autorização separados:

- As **políticas de acesso** controlam se um usuário, entidade de serviço ou grupo está autorizado a acessar segredos, chaves e certificados *dentro* de um recurso existente do Key Vault (às vezes chamadas de operações de "plano de dados"). As políticas de acesso são normalmente concedidas a usuários, grupos e aplicativos.

    Para atribuir políticas de acesso, confira os seguintes artigos:

    - [Azure portal](assign-access-policy-portal.md)
    - [CLI do Azure](assign-access-policy-cli.md)
    - [PowerShell do Azure](assign-access-policy-portal.md)

- As **permissões de função** controlam se um usuário, entidade de serviço ou grupo está autorizado a criar, excluir e, de outra forma, gerenciar um recurso do Key Vault (às vezes chamadas de operações de "plano de gerenciamento"). Essas funções, na maioria das vezes, são concedidas apenas a administradores.
 
    Para atribuir e gerenciar funções, confira os seguintes artigos:

    - [Azure portal](/azure/role-based-access-control/role-assignments-portal)
    - [CLI do Azure](/azure/role-based-access-control/role-assignments-cli)
    - [PowerShell do Azure](/azure/role-based-access-control/role-assignments-powershell)

    O Key Vault atualmente dá suporte à função de [Colaborador](/azure/role-based-access-control/built-in-roles#key-vault-contributor), que permite operações de gerenciamento nos recursos do Key Vault. Uma série de outras funções estão atualmente em versão prévia. Você também pode criar funções personalizadas, conforme descrito em [Funções personalizadas do Azure](/azure/role-based-access-control/custom-roles).

    Para obter informações gerais sobre funções, confira [O que é o RBAC (controle de acesso baseado em função) do Azure?](/azure/role-based-access-control/overview).


> [!IMPORTANT]
> Para maior segurança, sempre siga a entidade com o menor nível de privilégio e conceda apenas as políticas de acesso e funções mais específicas necessárias. 
    
## <a name="configure-the-key-vault-firewall"></a>Configure o firewall do Key Vault

Por padrão, o Key Vault permite o acesso a recursos por meio de endereços IP públicos. Para maior segurança, também é possível restringir o acesso a intervalos de IP específicos, pontos de extremidade de serviço, redes virtuais ou pontos de extremidade privados.

Para obter mais informações, confira [Acessar o Azure Key Vault por trás de um firewall](/azure/key-vault/general/access-behind-firewall).


## <a name="the-key-vault-authentication-flow"></a>O fluxo de autenticação do Key Vault

1. Uma entidade de serviço solicita a autenticação com o Azure AD, por exemplo:
    * Um usuário faz logon no portal do Azure usando um nome de usuário e uma senha.
    * Um aplicativo invoca uma API REST do Azure, apresentando uma ID do cliente e um segredo ou um certificado do cliente.
    * Um recurso do Azure, como uma máquina virtual com uma identidade gerenciada, entra em contato com o ponto de extremidade REST do [IMDS (Serviço de Metadados de Instância do Azure)](/azure/virtual-machines/windows/instance-metadata-service) para obtenção de um token de acesso.

1. Se a autenticação com o Azure AD for bem-sucedida, a entidade de serviço receberá um token OAuth.

1. A entidade de serviço faz uma chamada à API REST do Key Vault por meio do ponto de extremidade (URI) do Key Vault.

1. O Firewall do Key Vault verifica os critérios a seguir. Se algum critério for atendido, a chamada será permitida. Caso contrário, a chamada é bloqueada e uma resposta com a mensagem "proibido" é retornada.

    * O firewall está desabilitado e o ponto de extremidade público do Key Vault pode ser acessado pela Internet pública.
    * O chamador é um [Serviço Confiável do Key Vault](/azure/key-vault/general/overview-vnet-service-endpoints#trusted-services), permitindo que ele ignore o firewall.
    * O chamador está listado no firewall do Azure Key Vault por endereço IP, rede virtual ou ponto de extremidade de serviço.
    * O chamador pode acessar o Key Vault por meio de uma conexão de link privado configurada.    

1. Se o firewall permitir a chamada, o Key Vault chamará o Azure AD para validar o token de acesso da entidade de serviço.

1. O Key Vault verifica se a entidade de serviço tem a política de acesso necessária para a operação solicitada. Caso contrário, o Key Vault retornará uma resposta proibida.

1. O Key Vault executa a operação solicitada e retorna o resultado.

O seguinte diagrama ilustra o processo para um aplicativo que faz uma chamada à uma API de Obtenção de Segredo do Key Vault:

![O fluxo de autenticação do Azure Key Vault](../media/authentication/authentication-flow.png)

## <a name="code-examples"></a>Exemplos de código

A tabela a seguir contém links para artigos diferentes que demonstram como trabalhar com Key Vault no código do aplicativo usando as bibliotecas do SDK do Azure para a linguagem de programação em questão. Outras interfaces, como a CLI do Azure e o portal do Azure, estão incluídas para sua conveniência.

| Segredos do Key Vault | Chaves do Key Vault | Certificados do Cofre de Chaves |
|  --- | --- | --- |
| [Python](/azure/key-vault/secrets/quick-create-python) | [Python](/azure/key-vault/keys/quick-create-python) | [Python](/azure/key-vault/certificates/quick-create-python) | 
| [.NET (SDK v4)](/azure/key-vault/secrets/quick-create-net) | -- | -- |
| [.NET (SDK v3)](/azure/key-vault/secrets/quick-create-net-v3) | -- | -- |
| [Java](/azure/key-vault/secrets/quick-create-java) | -- | -- |
| [JavaScript](/azure/key-vault/secrets/quick-create-node) | -- | -- | 
| | | |
| [Azure portal](/azure/key-vault/secrets/quick-create-portal) | [Azure portal](/azure/key-vault/keys/quick-create-portal) | [Azure portal](/azure/key-vault/certificates/quick-create-portal) |
| [CLI do Azure](/azure/key-vault/secrets/quick-create-cli) | [CLI do Azure](/azure/key-vault/keys/quick-create-cli) | [CLI do Azure](/azure/key-vault/certificates/quick-create-cli) |
| [PowerShell do Azure](/azure/key-vault/secrets/quick-create-powershell) | [PowerShell do Azure](/azure/key-vault/keys/quick-create-powershell) | [PowerShell do Azure](/azure/key-vault/certificates/quick-create-powershell) |
| [Modelo de ARM](/azure/key-vault/secrets/quick-create-net) | -- | -- |

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas de política de acesso do Key Vault](troubleshooting-access-issues.md)
- [Códigos de erro da API REST do Key Vault](rest-error-codes.md)
- [Guia do desenvolvedor do Cofre de Chaves](developers-guide.md)
- [O que é o RBAC (controle de acesso baseado em função) do Azure?](/azure/role-based-access-control/overview)
