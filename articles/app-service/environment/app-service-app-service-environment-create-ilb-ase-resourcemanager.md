---
title: Criar ILB ASE v1
description: Crie um ambiente de Serviço de Aplicativo com um balanceador de carga interno (ILB ASE). Este doc é fornecido apenas para clientes que usam o Legado v1 ASE.
author: stefsch
ms.assetid: 091decb6-b0de-42a1-9f2f-c18d9b2e67df
ms.topic: article
ms.date: 07/11/2017
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: f05780610a2a6033b069721b143aca5e5efa6c35
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804513"
---
# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Como criar um ASE ILB usando modelos do Azure Resource Manager

> [!NOTE] 
> Este artigo é sobre o Ambiente do Serviço de Aplicativo v1. Há uma versão mais recente do Ambiente de Serviço de Aplicativo que é mais fácil de usar e é executado na infraestrutura mais avançada. Para saber mais sobre a nova versão, comece com a [Introdução ao Ambiente de Serviço do Aplicativo](intro.md).
>

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Visão geral
Ambientes de Serviço de Aplicativo podem ser criados com um endereço interno de rede virtual em vez de um VIP público.  Esse endereço interno é fornecido por um componente do Azure chamado ILB (balanceador de carga interno).  Um ASE ILB pode ser criado usando o portal do Azure.  Ele também pode ser criado usando a automação por meio de modelos do Azure Resource Manager.  Este artigo explica as etapas e a sintaxe necessária para criar um ASE ILB com modelos do Azure Resource Manager.

Há três etapas envolvidas na automatização da criação de um ASE ILB:

1. Primeiro o ASE base é criado em uma rede virtual usando um endereço de balanceador de carga interno em vez de um VIP público.  Como parte dessa etapa, um nome de domínio raiz é atribuído ao ASE ILB.
2. Uma vez que o ILB ASE é criado, um certificado TLS/SSL é carregado.  
3. O certificado TLS/SSL carregado é explicitamente atribuído ao ILB ASE como seu certificado TLS/SSL "padrão".  Este certificado TLS/SSL será usado para tráfego TLS para aplicativos no ILB ASE quando os aplicativos forem endereçados usando o domínio raiz comum atribuído ao ASE (por `https://someapp.mycustomrootcomain.com`exemplo)

## <a name="creating-the-base-ilb-ase"></a>Criando o ASE ILB base
Um modelo do Azure Resource Manager de exemplo e seu arquivo de parâmetros associado estão disponíveis no GitHub [aqui][quickstartilbasecreate].

A maioria dos parâmetros no arquivo *azuredeploy.parameters.json* são comuns para criar os ASEs ILB, bem como ASEs associados a um VIP público.  A lista abaixo, ao criar um ASE ILB, chama parâmetros de anotação especial ou exclusivos:

* *internalLoadBalancingMode*: Na maioria dos casos, defina isso como 3, o que significa que tanto o tráfego HTTP/HTTPS nas portas 80/443, quanto as portas de controle/canal de dados ouvidas pelo serviço FTP na ASE, estarão vinculados a um endereço interno de rede virtual alocado pelo ILB.  Se essa propriedade for definida como 2, somente as portas relacionadas ao serviço FTP (canais de controle e de dados) serão associadas a um endereço ILB, ao passo que o tráfego HTTP/HTTPS permanecerá no VIP público.
* *dnsSufix*: Este parâmetro define o domínio raiz padrão que será atribuído ao ASE.  A variação pública do Serviço de Aplicativo do Azure, o domínio de raiz padrão para todos os aplicativos Web, é *azurewebsites.net*.  No entanto, como um ASE ILB é interno na rede virtual do cliente, não faz sentido usar o domínio de raiz padrão do serviço público.  Em vez disso, um ASE ILB deve ter um domínio de raiz padrão que faça sentido para uso dentro da rede virtual interna da empresa.  Por exemplo, uma empresa hipotética Contoso pode usar um domínio raiz padrão *internal-contoso.com* para aplicativos que se destinam apenas a serem resolvidos e acessados na rede virtual da Contoso. 
* *ipSslAddressCount*: esse parâmetro é automaticamente padronizado para um valor 0 no arquivo *azuredeploy.json* porque os ASEs ILB têm apenas um único endereço ILB.  Não há nenhum endereço IP SSL explícito para um ASE ILB e, portanto, o pool de endereços IP SSL para um ASE ILB deve ser definido como zero. Caso contrário, ocorrerá um erro de provisionamento. 

Quando o arquivo *azuredeploy.parameters.json* for preenchido para um ASE ILB, o ASE ILB pode ser criado usando o snippet de código do Powershell a seguir.  Altere os CAMINHOS do arquivo para corresponder ao local em que os arquivos do modelo do Azure Resource Manager estão localizados no seu computador.  Além disso, lembre-se de fornecer seus próprios valores para o nome de implantação do Azure Resource Manager e para o nome do grupo de recursos.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Depois que o modelo do Azure Resource Manager for enviado, levará algumas horas para o ASE ILB ser criado.  Uma vez concluída a criação, o ASE ILB aparecerá no portal de experiência do usuário na lista de Ambientes de Serviço de Aplicativo para a assinatura que disparou a implantação.

## <a name="uploading-and-configuring-the-default-tlsssl-certificate"></a>Upload e configuração do certificado TLS/SSL "Padrão"
Uma vez que o ILB ASE é criado, um certificado TLS/SSL deve ser associado ao ASE como o uso do certificado TLS/SSL "padrão" para estabelecer conexões TLS/SSL a aplicativos.  Continuando com o hipotético exemplo da Contoso Corporation, se o sufixo *https://some-random-app.internal-contoso.com* DNS padrão da ASE for *internal-contoso.com,* então uma conexão requer um certificado TLS/SSL válido para **.internal-contoso.com*. 

Existem várias maneiras de obter um certificado TLS/SSL válido, incluindo CAs internos, compra de um certificado de um emissor externo e uso de um certificado auto-assinado.  Independentemente da origem do certificado TLS/SSL, os seguintes atributos de certificado precisam ser configurados corretamente:

* *Assunto*: esse atributo deve ser definido para **.domínio-raiz-aqui.com*
* *Nome Alternativo da Entidade*: esse atributo deve incluir **.your-root-domain-here.com* e **.scm.your-root-domain-here.com*.  A razão para a segunda entrada é que as conexões TLS ao site SCM/Kudu associadas a cada aplicativo serão feitas usando um endereço do formulário *your-app-name.scm.your-root-domain-here.com*.

Com um certificado TLS/SSL válido em mãos, são necessárias duas etapas preparatórias adicionais.  O certificado TLS/SSL precisa ser convertido/salvo como um arquivo .pfx.  Lembre-se de que o arquivo .pfx deve incluir todos os certificados intermediários e raiz e também precisa ser protegido com uma senha.

Em seguida, o arquivo .pfx resultante precisa ser convertido em uma seqüência base64 porque o certificado TLS/SSL será carregado usando um modelo do Azure Resource Manager.  Já que os modelos do Azure Resource Manager são arquivos de texto, o arquivo .pfx deve ser convertido em uma cadeia de caracteres de base64 para poder ser incluída como um parâmetro do modelo.

O snippet de código do Powershell abaixo mostra um exemplo de como gerar um certificado autoassinado, exportar o certificado como um arquivo .pfx, converter o arquivo .pfx em uma cadeia de caracteres codificada em base64 e salvar essa cadeia de caracteres em um arquivo separado.  O código do PowerShell para a codificação de base64 foi adaptado do [Blog de Scripts do PowerShell][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

Uma vez que o certificado TLS/SSL tenha sido gerado e convertido com sucesso em uma seqüência codificada base64, o exemplo do azure Resource Manager no GitHub para [configurar o certificado TLS/SSL padrão][configuringDefaultSSLCertificate] pode ser usado.

Os parâmetros no arquivo *azuredeploy.parameters.json* estão listados abaixo:

* *appServiceEnvironmentName*: O nome do ILB ASE sendo configurado.
* *existenteAseLocalização*: Seqüência de texto contendo a região Azure onde o ILB ASE foi implantado.  Por exemplo: "Centro-Sul dos EUA".
* *pfxBlobString*: A representação de string codificada com base64 do arquivo .pfx.  Usando o snippet de código mostrado anteriormente, copie a cadeia de caracteres contida no "exportedcert.pfx.b64" e cole-a como o valor do atributo *pfxBlobString*.
* *password*: a senha usada para proteger o arquivo .pfx.
* *certificadoImpressão de polegar*: Impressão digital do certificado.  Se você recuperar esse valor do Powershell (por exemplo, *$certificate.Thumbprint* do snippet de código anterior), poderá usar o valor como estiver.  No entanto se você copiar o valor da caixa de diálogo Certificado Windows, lembre-se de retirar os espaços estranhos.  O *certificadoThumbprint* deve ser algo parecido com: AF3143EB61D43F672784215BB7F17BBCECAE
* *certificateName*: Um identificador de seqüência amigável de sua própria escolha usado para identificar o certificado.  O nome é usado como parte do identificador exclusivo do Azure Resource Manager para a entidade *Microsoft.Web/certificates* representando o certificado TLS/SSL.  O nome **deve** terminar com o \_seguinte sufixo: yourASENameHere_InternalLoadBalancingASE.  Esse sufixo é usado pelo portal como um indicador de que o certificado é usado para proteger um ASE habilitado por ILB.

Um exemplo abreviado de *azuredeploy.parameters.json* é mostrado abaixo:

    {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Uma vez que o arquivo *azuredeploy.parameters.json* tenha sido preenchido, o certificado TLS/SSL padrão pode ser configurado usando o seguinte trecho de código Powershell.  Altere os CAMINHOS do arquivo para corresponder ao local em que os arquivos do modelo do Azure Resource Manager estão localizados no seu computador.  Além disso, lembre-se de fornecer seus próprios valores para o nome de implantação do Azure Resource Manager e para o nome do grupo de recursos.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Depois que o modelo do Azure Resource Manager for enviado, levará aproximadamente quarenta minutos por front-end do ASE para aplicar a alteração.  Por exemplo, com um ASE padrão dimensionado usando dois front-ends, o modelo levará aproximadamente uma hora e vinte minutos para concluir.  Enquanto o modelo estiver em execução, o ASE não poderá ser dimensionado.  

Uma vez que o modelo seja concluído, os aplicativos no ILB ASE podem ser acessados via HTTPS e as conexões serão protegidas usando o certificado TLS/SSL padrão.  O certificado TLS/SSL padrão será usado quando os aplicativos no ILB ASE forem endereçados usando uma combinação do nome do aplicativo mais o nome de host padrão.  Por *https://mycustomapp.internal-contoso.com* exemplo, usaria o certificado TLS/SSL padrão para **.internal-contoso.com*.

No entanto, assim como aplicativos em execução no serviço público multi-inquilino, os desenvolvedores também podem configurar nomes de host personalizados para aplicativos individuais e, em seguida, configurar vinculações exclusivas de certificadoS SNI TLS/SSL para aplicativos individuais.  

## <a name="getting-started"></a>Introdução
Para se familiarizar com os ambientes de serviço de aplicativo, consulte [Introdução ao ambiente do serviço de aplicativo](app-service-app-service-environment-intro.md)

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: https://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 

