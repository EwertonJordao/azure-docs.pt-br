---
title: Funções de modelo – cadeia de caracteres
description: Descreve as funções a serem usadas em um modelo do Azure Resource Manager para trabalhar com cadeias de caracteres.
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: c0517375b273384f263e8ba421995d4afb6c193b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80982407"
---
# <a name="string-functions-for-arm-templates"></a>Funções de cadeia de caracteres para modelos ARM

O Gerenciador de recursos fornece as seguintes funções para trabalhar com cadeias de caracteres em seus modelos de Azure Resource Manager (ARM):

* [Base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [Concat](#concat)
* [contains](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [esvaziá](#empty)
* [endsWith](#endswith)
* [primeiro](#first)
* [format](#format)
* [volume](#guid)
* [indexOf](#indexof)
* [última](#last)
* [lastIndexOf](#lastindexof)
* [length](#length)
* [newGuid](#newguid)
* [padLeft](#padleft)
* [replace](#replace)
* [saltar](#skip)
* [Divisão](#split)
* [startsWith](#startswith)
* [cadeia de caracteres](#string)
* [Subcadeia](#substring)
* [ter](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [Trim](#trim)
* [uniqueString](#uniquestring)
* [URI](#uri)
* [uriComponent](#uricomponent)
* [uriComponentToString](#uricomponenttostring)

## <a name="base64"></a>base64

`base64(inputString)`

Retorna a representação base64 da cadeia de caracteres de entrada.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| inputString |Sim |cadeia de caracteres |O valor a retornar como uma representação base64. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres que contém a representação base64.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) a seguir mostra como usar a função base64.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| base64Output | Cadeia de caracteres | b25lLCB0d28sIHRocmVl |
| toStringOutput | Cadeia de caracteres | um, dois, três |
| toJsonOutput | Objeto | {"one": "a", "two": "b"} |

## <a name="base64tojson"></a>base64ToJson

`base64tojson`

Converte uma representação base64 em um objeto JSON.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| base64Value |Sim |cadeia de caracteres |A representação base64 a ser convertida em um objeto JSON. |

### <a name="return-value"></a>Valor retornado

Um objeto JSON.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) a seguir usa a função base64ToJson para converter um valor base64:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| base64Output | Cadeia de caracteres | b25lLCB0d28sIHRocmVl |
| toStringOutput | Cadeia de caracteres | um, dois, três |
| toJsonOutput | Objeto | {"one": "a", "two": "b"} |

## <a name="base64tostring"></a>base64ToString

`base64ToString(base64Value)`

Converte uma representação base64 em uma cadeia de caracteres.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| base64Value |Sim |cadeia de caracteres |A representação base64 a ser convertida em uma cadeia de caracteres. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres do valor base64 convertido.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) a seguir usa a função base64ToString para converter um valor base64:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| base64Output | Cadeia de caracteres | b25lLCB0d28sIHRocmVl |
| toStringOutput | Cadeia de caracteres | um, dois, três |
| toJsonOutput | Objeto | {"one": "a", "two": "b"} |

## <a name="concat"></a>concat

`concat (arg1, arg2, arg3, ...)`

Combina vários valores de cadeia de caracteres e retorna a cadeia de caracteres concatenada ou combina várias matrizes e retorna a matriz concatenada.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| arg1 |Sim |cadeia de caracteres ou matriz |A primeira cadeia de caracteres ou matriz para concatenação. |
| argumentos adicionais |Não |cadeia de caracteres ou matriz |Cadeias de caracteres ou matrizes adicionais em ordem sequencial para concatenação. |

Essa função pode conter qualquer número de argumentos e pode aceitar cadeias de caracteres ou matrizes como parâmetros. No entanto, você não pode fornecer matrizes e cadeias de caracteres para parâmetros. As cadeias de caracteres são concatenadas apenas com outras cadeias de caracteres.

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres ou matriz de valores concatenados.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-string.json) a seguir mostra como combinar dois valores de cadeia de caracteres e retornar uma cadeia de caracteres concatenada.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| concatOutput | Cadeia de caracteres | prefix-5yj4yjf5mbg72 |

O próximo [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-array.json) mostra como combinar duas matrizes.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| return | Array | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

## <a name="contains"></a>contains

`contains (container, itemToFind)`

Verifica se uma matriz contém um valor, um objeto contém uma chave ou uma cadeia de caracteres contém uma subcadeia de caracteres. A comparação de cadeia de caracteres diferencia maiúsculas de minúsculas. No entanto, ao testar se um objeto contém uma chave, a comparação não diferencia maiúsculas de minúsculas.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| contêiner |Sim |matriz, objeto ou cadeia de caracteres |O valor que contém o valor a ser encontrado. |
| itemToFind |Sim |cadeia de caracteres ou inteiro |O valor a ser encontrado. |

### <a name="return-value"></a>Valor retornado

**True** se o item for encontrado; caso contrário, **False**.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/contains.json) a seguir mostra como usar contains com tipos diferentes:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| stringTrue | Bool | verdadeiro |
| stringFalse | Bool | Falso |
| objectTrue | Bool | verdadeiro |
| objectFalse | Bool | Falso |
| arrayTrue | Bool | verdadeiro |
| arrayFalse | Bool | Falso |

## <a name="datauri"></a>dataUri

`dataUri(stringToConvert)`

Converte um valor em um URI de dados.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToConvert |Sim |cadeia de caracteres |O valor a ser convertido em um URI de dados. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres formatada como um URI de dados.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) a seguir converte um valor em um URI de dados e um URI de dados em uma cadeia de caracteres:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| dataUriOutput | Cadeia de caracteres | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | Cadeia de caracteres | Hello, World! |

## <a name="datauritostring"></a>dataUriToString

`dataUriToString(dataUriToConvert)`

Converte um valor formatado como um URI de dados em uma cadeia de caracteres.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Sim |cadeia de caracteres |Os valor de URI de dados a ser convertido. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres que contém o valor convertido.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) a seguir converte um valor em um URI de dados e um URI de dados em uma cadeia de caracteres:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| dataUriOutput | Cadeia de caracteres | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | Cadeia de caracteres | Hello, World! |

## <a name="empty"></a>vazio

`empty(itemToTest)`

Determina se uma matriz, objeto ou uma cadeia de caracteres está vazio.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| itemToTest |Sim |matriz, objeto ou cadeia de caracteres |O valor para verificar se ele está vazio. |

### <a name="return-value"></a>Valor retornado

Retorna **True** se o valor é vazio; caso contrário, **False**.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/empty.json) a seguir verifica se uma matriz, um objeto e uma cadeia de caracteres estão vazios.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| arrayEmpty | Bool | verdadeiro |
| objectEmpty | Bool | verdadeiro |
| stringEmpty | Bool | verdadeiro |

## <a name="endswith"></a>endsWith

`endsWith(stringToSearch, stringToFind)`

Determina se uma cadeia de caracteres termina com um valor. A comparação não diferencia maiúsculas de minúsculas.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sim |cadeia de caracteres |O valor que contém o item a ser encontrado. |
| stringToFind |Sim |cadeia de caracteres |O valor a ser encontrado. |

### <a name="return-value"></a>Valor retornado

**True** se o último caractere ou caracteres da cadeia de caracteres corresponderem ao valor; caso contrário, **False**.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) a seguir mostra como usar as funções startsWith e endsWith:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| startsTrue | Bool | verdadeiro |
| startsCapTrue | Bool | verdadeiro |
| startsFalse | Bool | Falso |
| endsTrue | Bool | verdadeiro |
| endsCapTrue | Bool | verdadeiro |
| endsFalse | Bool | Falso |

## <a name="first"></a>first

`first(arg1)`

Retorna o primeiro caractere da cadeia de caracteres ou o primeiro elemento da matriz.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| arg1 |Sim |matriz ou cadeia de caracteres |O valor para recuperar o primeiro elemento ou caractere. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres do primeiro caractere ou o tipo (cadeia de caracteres, inteiro, matriz ou objeto) do primeiro elemento em uma matriz.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/first.json) a seguir mostra como usar a primeira função com uma matriz e cadeia de caracteres.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| arrayOutput | Cadeia de caracteres | one |
| stringOutput | Cadeia de caracteres | O |

## <a name="format"></a>format

`format(formatString, arg1, arg2, ...)`

Cria uma cadeia de caracteres formatada a partir de valores de entrada.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| formatString | Sim | cadeia de caracteres | A cadeia de caracteres de formato de composição. |
| arg1 | Sim | Cadeia de caracteres, inteiro ou booliano | O valor a ser incluído na cadeia de caracteres formatada. |
| argumentos adicionais | Não | Cadeia de caracteres, inteiro ou booliano | Valores adicionais a serem incluídos na cadeia de caracteres formatada. |

### <a name="remarks"></a>Comentários

Use essa função para formatar uma cadeia de caracteres em seu modelo. Ele usa as mesmas opções de formatação que o método [System. String. Format](/dotnet/api/system.string.format) no .net.

### <a name="examples"></a>Exemplos

O modelo de exemplo a seguir mostra como usar a função Format.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "greeting": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "name": {
            "type": "string",
            "defaultValue": "User"
        },
        "numberToFormat": {
            "type": "int",
            "defaultValue": 8175133
        }
    },
    "resources": [
    ],
    "outputs": {
        "formatTest": {
            "type": "string",
            "value": "[format('{0}, {1}. Formatted number: {2:N0}', parameters('greeting'), parameters('name'), parameters('numberToFormat'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| formatTest | Cadeia de caracteres | Olá, usuário. Número formatado: 8.175.133 |

## <a name="guid"></a>guid

`guid(baseString, ...)`

Cria um valor no formato de um identificador global exclusivo com base nos valores fornecidos como parâmetros.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| baseString |Sim |cadeia de caracteres |O valor usado na função de hash para criar o GUID. |
| parâmetros extras conforme necessário |Não |cadeia de caracteres |Você pode adicionar quantas cadeias de caracteres forem necessárias para criar o valor que especifica o nível de exclusividade. |

### <a name="remarks"></a>Comentários

Essa função é útil quando você precisa criar um valor no formato de um identificador global exclusivo. Você fornece valores de parâmetros que limitam o escopo de exclusividade para o resultado. Você pode especificar se o nome é exclusivo para a assinatura, grupo de recursos ou implantação.

O valor retornado não é uma cadeia de caracteres aleatória, mas sim o resultado de uma função de hash nos parâmetros. O valor retornado tem 36 caracteres. Não é globalmente exclusivo. Para criar um novo GUID que não seja baseado nesse valor de hash dos parâmetros, use a função [newGuid](#newguid) .

Os exemplos a seguir mostram como usar guid para criar um valor exclusivo para níveis usados com mais frequência.

Escopo exclusivo para a assinatura

```json
"[guid(subscription().subscriptionId)]"
```

Escopo exclusivo para o grupo de recursos

```json
"[guid(resourceGroup().id)]"
```

Escopo exclusivo para a implantação de um grupo de recursos

```json
"[guid(resourceGroup().id, deployment().name)]"
```

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres que contém 36 caracteres no formato de um identificador global exclusivo.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/guid.json) abaixo retorna os resultados de guid:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [],
    "outputs": {
        "guidPerSubscription": {
            "value": "[guid(subscription().subscriptionId)]",
            "type": "string"
        },
        "guidPerResourceGroup": {
            "value": "[guid(resourceGroup().id)]",
            "type": "string"
        },
        "guidPerDeployment": {
            "value": "[guid(resourceGroup().id, deployment().name)]",
            "type": "string"
        }
    }
}
```

## <a name="indexof"></a>indexOf

`indexOf(stringToSearch, stringToFind)`

Retorna a primeira posição de um valor em uma cadeia de caracteres. A comparação não diferencia maiúsculas de minúsculas.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sim |cadeia de caracteres |O valor que contém o item a ser encontrado. |
| stringToFind |Sim |cadeia de caracteres |O valor a ser encontrado. |

### <a name="return-value"></a>Valor retornado

Um inteiro que representa a posição do item a ser encontrado. O valor é baseado em zero. Se o item não for encontrado,-1 será retornado.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) a seguir mostra como usar as funções indexOf e lastIndexOf:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| firstT | Int | 0 |
| lastT | Int | 3 |
| firstString | Int | 2 |
| lastString | Int | 0 |
| NotFound | Int | -1 |

## <a name="last"></a>last

`last (arg1)`

Retorna o último caractere da cadeia de caracteres ou o último elemento da matriz.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| arg1 |Sim |matriz ou cadeia de caracteres |O valor para recuperar o último elemento ou caractere. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres do último caractere ou o tipo (cadeia de caracteres, inteiro, matriz ou objeto) do último elemento em uma matriz.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/last.json) a seguir mostra como usar a última função com uma matriz e cadeia de caracteres.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| arrayOutput | Cadeia de caracteres | três |
| stringOutput | Cadeia de caracteres | e |

## <a name="lastindexof"></a>lastIndexOf

`lastIndexOf(stringToSearch, stringToFind)`

Retorna a última posição de um valor em uma cadeia de caracteres. A comparação não diferencia maiúsculas de minúsculas.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sim |cadeia de caracteres |O valor que contém o item a ser encontrado. |
| stringToFind |Sim |cadeia de caracteres |O valor a ser encontrado. |

### <a name="return-value"></a>Valor retornado

Um inteiro que representa a última posição do item a ser encontrado. O valor é baseado em zero. Se o item não for encontrado,-1 será retornado.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) a seguir mostra como usar as funções indexOf e lastIndexOf:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| firstT | Int | 0 |
| lastT | Int | 3 |
| firstString | Int | 2 |
| lastString | Int | 0 |
| NotFound | Int | -1 |

## <a name="length"></a>comprimento

`length(string)`

Retorna o número de caracteres em uma cadeia de caracteres, elementos em uma matriz ou propriedades de nível raiz em um objeto.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| arg1 |Sim |matriz, Cadeia de caracteres ou objeto |A matriz a ser usada para obter o número de elementos, a cadeia de caracteres a ser usada para obter o número de caracteres ou o objeto a ser usado para obter o número de propriedades no nível raiz. |

### <a name="return-value"></a>Valor retornado

Um inteiro. 

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/length.json) a seguir mostra como usar length com uma matriz e cadeia de caracteres:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {
                "propA": "one",
                "propB": "two",
                "propC": "three",
                "propD": {
                    "propD-1": "sub",
                    "propD-2": "sub"
                }
            }
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        },
        "objectLength": {
            "type": "int",
            "value": "[length(parameters('objectToTest'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| arrayLength | Int | 3 |
| stringLength | Int | 13 |
| ObjectLength | Int | 4 |

## <a name="newguid"></a>newGuid

`newGuid()`

Retorna um valor no formato de um identificador global exclusivo. **Essa função só pode ser usada no valor padrão para um parâmetro.**

### <a name="remarks"></a>Comentários

Você só pode usar essa função dentro de uma expressão para o valor padrão de um parâmetro. O uso dessa função em qualquer outro lugar em um modelo retorna um erro. A função não é permitida em outras partes do modelo porque ela retorna um valor diferente cada vez que é chamada. Implantar o mesmo modelo com os mesmos parâmetros não produziria com confiança os mesmos resultados.

A função newGuid difere da função [GUID](#guid) porque ela não assume nenhum parâmetro. Quando você chama o GUID com o mesmo parâmetro, ele retorna o mesmo identificador a cada vez. Use o GUID quando precisar gerar com confiança o mesmo GUID para um ambiente específico. Use newGuid quando precisar de um identificador diferente a cada vez, como a implantação de recursos em um ambiente de teste.

A função newGuid usa a [estrutura de GUID](/dotnet/api/system.guid) na .NET Framework para gerar o identificador global exclusivo.

Se você usar a [opção para reimplantar uma implantação bem-sucedida anterior](rollback-on-error.md)e a implantação anterior incluir um parâmetro que usa newGuid, o parâmetro não será reavaliado. Em vez disso, o valor do parâmetro da implantação anterior é automaticamente reutilizado na implantação de reversão.

Em um ambiente de teste, talvez seja necessário implantar repetidamente os recursos que só moram por um curto período de tempo. Em vez de construir nomes exclusivos, você pode usar newGuid com [uniquestring](#uniquestring) para criar nomes exclusivos.

Tenha cuidado ao reimplantar um modelo que dependa da função newGuid para um valor padrão. Quando você reimplanta e não fornece um valor para o parâmetro, a função é reavaliada. Se você quiser atualizar um recurso existente em vez de criar um novo, passe o valor do parâmetro da implantação anterior.

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres que contém 36 caracteres no formato de um identificador global exclusivo.

### <a name="examples"></a>Exemplos

O modelo de exemplo a seguir mostra um parâmetro com um novo identificador.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "guidValue": {
            "type": "string",
            "defaultValue": "[newGuid()]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "guidOutput": {
            "type": "string",
            "value": "[parameters('guidValue')]"
        }
    }
}
```

A saída do exemplo anterior varia de acordo com cada implantação, mas será semelhante a:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| guidOutput | cadeia de caracteres | b76a51fc-bd72-4a77-b9a2-3c29e7d2e551 |

O exemplo a seguir usa a função newGuid para criar um nome exclusivo para uma conta de armazenamento. Esse modelo pode funcionar para o ambiente de teste em que a conta de armazenamento existe por um curto período e não é reimplantada.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "guidValue": {
            "type": "string",
            "defaultValue": "[newGuid()]"
        }
    },
    "variables": {
        "storageName": "[concat('storage', uniqueString(parameters('guidValue')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "location": "West US",
            "apiVersion": "2018-07-01",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {}
        }
    ],
    "outputs": {
        "nameOutput": {
            "type": "string",
            "value": "[variables('storageName')]"
        }
    }
}
```

A saída do exemplo anterior varia de acordo com cada implantação, mas será semelhante a:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| nameOutput | cadeia de caracteres | storagenziwvyru7uxie |


## <a name="padleft"></a>padLeft

`padLeft(valueToPad, totalLength, paddingCharacter)`

Retorna uma cadeia de caracteres alinhada à direita adicionando caracteres à esquerda até alcançar o comprimento total especificado.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| valueToPad |Sim |cadeia de caracteres ou inteiro |O valor para alinhar à direita. |
| totalLength |Sim |INT |O número total de caracteres na cadeia de caracteres retornada. |
| paddingCharacter |Não |caractere único |O caractere a ser usado para o preenchimento à esquerda até que o tamanho total seja atingido. O valor padrão é um espaço. |

Se a cadeia de caracteres original for mais longa que o número de caracteres a ser preenchido, nenhum caractere será adicionado.

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres com, pelo menos, o número de caracteres especificado.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/padleft.json) a seguir mostra como preencher o valor de parâmetro fornecido pelo usuário adicionando o caractere zero até que ele atinja o número total de caracteres. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| stringOutput | Cadeia de caracteres | 0000000123 |

## <a name="replace"></a>substituir

`replace(originalString, oldString, newString)`

Retorna uma nova cadeia de caracteres com todas as instâncias de uma cadeia de caracteres substituídas por outra cadeia de caracteres.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| originalString |Sim |cadeia de caracteres |O valor que tem todas as instâncias de uma cadeia de caracteres substituídas por outra cadeia de caracteres. |
| oldString |Sim |cadeia de caracteres |A cadeia de caractere a ser removida da cadeia de caracteres original. |
| newString |Sim |cadeia de caracteres |A cadeia de caracteres a ser adicionada no lugar da cadeia removida. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres com os caracteres substituídos.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/replace.json) a seguir mostra como remover todos os traços da cadeia de caracteres fornecida pelo usuário e como substituir parte da cadeia de caracteres por outra cadeia de caracteres.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secondOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| firstOutput | Cadeia de caracteres | 1231231234 |
| secondOutput | Cadeia de caracteres | 123-123-xxxx |

## <a name="skip"></a>skip

`skip(originalValue, numberToSkip)`

Retorna uma cadeia de caracteres com todos os caracteres após o número especificado de caracteres ou uma matriz com todos os elementos após o número especificado de elementos.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| originalValue |Sim |matriz ou cadeia de caracteres |A matriz ou cadeia de caracteres a ser usada para ignorar. |
| numberToSkip |Sim |INT |O número de elementos ou caracteres a ser ignorado. Se esse valor for 0 ou menos, todos os elementos ou caracteres no valor serão retornados. Se for maior do que o comprimento da matriz ou da cadeia de caracteres, uma matriz ou cadeia de caracteres vazia será retornada. |

### <a name="return-value"></a>Valor retornado

Uma matriz ou cadeia de caracteres.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/skip.json) a seguir ignora o número especificado de elementos na matriz e o número especificado de caracteres em uma cadeia de caracteres.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| arrayOutput | Array | ["three"] |
| stringOutput | Cadeia de caracteres | two three |

## <a name="split"></a>split

`split(inputString, delimiter)`

Retorna uma matriz de cadeias de caracteres que contém as subcadeias de caracteres da cadeia de caracteres de entrada que são delimitadas por delimitadores especificados.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| inputString |Sim |cadeia de caracteres |A cadeia de caracteres a dividir. |
| delimiter |Sim |cadeia de caracteres ou matriz de cadeias de caracteres |O delimitador a ser usado para dividir a cadeia de caracteres. |

### <a name="return-value"></a>Valor retornado

Uma matriz de cadeias de caracteres.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/split.json) a seguir divide a cadeia de caracteres de entrada com uma vírgula e com uma vírgula ou um ponto-e-vírgula.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| firstOutput | Array | ["one", "two", "three"] |
| secondOutput | Array | ["one", "two", "three"] |

## <a name="startswith"></a>startsWith

`startsWith(stringToSearch, stringToFind)`

Determina se uma cadeia de caracteres começa com um valor. A comparação não diferencia maiúsculas de minúsculas.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sim |cadeia de caracteres |O valor que contém o item a ser encontrado. |
| stringToFind |Sim |cadeia de caracteres |O valor a ser encontrado. |

### <a name="return-value"></a>Valor retornado

**True** se o primeiro caractere ou caracteres da cadeia de caracteres corresponderem ao valor; caso contrário, **False**.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) a seguir mostra como usar as funções startsWith e endsWith:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| startsTrue | Bool | verdadeiro |
| startsCapTrue | Bool | verdadeiro |
| startsFalse | Bool | Falso |
| endsTrue | Bool | verdadeiro |
| endsCapTrue | Bool | verdadeiro |
| endsFalse | Bool | Falso |

## <a name="string"></a>string

`string(valueToConvert)`

Converte o valor especificado em uma cadeia de caracteres.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| valueToConvert |Sim | Qualquer |O valor a ser convertido em cadeia de caracteres. Qualquer tipo de valor pode ser convertido, incluindo objetos e matrizes. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres do valor convertido.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/string.json) a seguir mostra como converter diferentes tipos de valores em cadeias de caracteres:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| objectOutput | Cadeia de caracteres | {"valueA":10,"valueB":"Example Text"} |
| arrayOutput | Cadeia de caracteres | ["a","b","c"] |
| intOutput | Cadeia de caracteres | 5 |

## <a name="substring"></a>substring

`substring(stringToParse, startIndex, length)`

Retorna uma subcadeia de caraceteres que começa na posição do caractere especificado e contém o número especificado de caracteres.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToParse |Sim |cadeia de caracteres |A cadeia original da qual a subcadeia de caracteres é extraída. |
| startIndex |Não |INT |A posição inicial do caractere baseada em zero para a subcadeia de caracteres. |
| comprimento |Não |INT |O número de caracteres para a subcadeia de caracteres. Deve se referir a uma localização dentro da cadeia de caracteres. Deve ser zero ou maior. |

### <a name="return-value"></a>Valor retornado

A subcadeia de caracteres. Ou, uma cadeia de caracteres vazia se o comprimento for zero.

### <a name="remarks"></a>Comentários

A função falhará quando a subcadeia de caracteres ultrapassar o final da cadeia de caracteres, ou quando o comprimento for menor que zero. O exemplo a seguir falha com o erro “Os parâmetros de índice e de tamanho devem se referir a uma localização na cadeia de caracteres. O parâmetro de índice: '0', o parâmetro de comprimento: '11', o comprimento do parâmetro de cadeia de caracteres: '10'”.

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/substring.json) a seguir extrai uma subcadeia de caracteres de um parâmetro.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| substringOutput | Cadeia de caracteres | dois |

## <a name="take"></a>take

`take(originalValue, numberToTake)`

Retorna uma cadeia de caracteres com o número especificado de caracteres desde o início da cadeia de caracteres ou uma matriz com o número especificado de elementos desde o início da matriz.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| originalValue |Sim |matriz ou cadeia de caracteres |A matriz ou cadeia de caracteres da qual extrair os elementos. |
| numberToTake |Sim |INT |O número de elementos ou caracteres a ser extraído. Se esse valor for 0 ou menos, uma matriz ou cadeia de caracteres vazia será retornada. Se for maior do que o comprimento da matriz ou cadeia de caracteres determinada, todos os elementos na matriz ou na cadeia de caracteres serão retornados. |

### <a name="return-value"></a>Valor retornado

Uma matriz ou cadeia de caracteres.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/take.json) a seguir extrai o número especificado de elementos da matriz e de caracteres de uma cadeia de caracteres.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| arrayOutput | Array | ["one", "two"] |
| stringOutput | Cadeia de caracteres | on |

## <a name="tolower"></a>toLower

`toLower(stringToChange)`

Converte a cadeia de caracteres especificada em letras minúsculas.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToChange |Sim |cadeia de caracteres |O valor a ser convertido em letras minúsculas. |

### <a name="return-value"></a>Valor retornado

A cadeia de caracteres convertida em minúsculas.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) a seguir converte um valor de parâmetro em letras minúsculas e maiúsculas.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| toLowerOutput | Cadeia de caracteres | um dois três |
| toUpperOutput | Cadeia de caracteres | UM DOIS TRÊS |

## <a name="toupper"></a>toUpper

`toUpper(stringToChange)`

Converte a cadeia de caracteres especificada em maiúsculas.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToChange |Sim |cadeia de caracteres |O valor a ser convertido em letras maiúsculas. |

### <a name="return-value"></a>Valor retornado

A cadeia de caracteres convertida em maiúsculas.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) a seguir converte um valor de parâmetro em letras minúsculas e maiúsculas.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| toLowerOutput | Cadeia de caracteres | um dois três |
| toUpperOutput | Cadeia de caracteres | UM DOIS TRÊS |

## <a name="trim"></a>cortar

`trim (stringToTrim)`

Remove todos os caracteres de espaço em branco à esquerda e à direita da cadeia de caracteres especificada.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToTrim |Sim |cadeia de caracteres |O valor de corte. |

### <a name="return-value"></a>Valor retornado

A cadeia de caracteres sem caracteres de espaço em branco à esquerda e à direita.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/trim.json) a seguir remove os caracteres de espaço em branco do parâmetro.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| return | Cadeia de caracteres | um dois três |

## <a name="uniquestring"></a>uniqueString

`uniqueString (baseString, ...)`

Cria uma cadeia de caracteres de hash determinístico com base nos valores fornecidos como parâmetros. 

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| baseString |Sim |cadeia de caracteres |O valor usado na função de hash para criar uma cadeia de caracteres exclusiva. |
| parâmetros extras conforme necessário |Não |cadeia de caracteres |Você pode adicionar quantas cadeias de caracteres forem necessárias para criar o valor que especifica o nível de exclusividade. |

### <a name="remarks"></a>Comentários

Essa função é útil quando você precisa criar um nome exclusivo para um recurso. Você fornece valores de parâmetros que limitam o escopo de exclusividade para o resultado. Você pode especificar se o nome é exclusivo para a assinatura, grupo de recursos ou implantação. 

O valor retornado não é uma cadeia de caracteres aleatória, mas sim o resultado de uma função de hash. O valor retornado tem 13 caracteres. Não é globalmente exclusivo. Você talvez queira combinar o valor com um prefixo de sua convenção de nomenclatura para criar um nome significativo. O exemplo a seguir mostra o formato do valor retornado. O valor real poderá variar de acordo com os parâmetros fornecidos.

    tcvhiyu5h2o5o

Os exemplos a seguir mostram como usar uniqueString para criar um valor exclusivo para níveis usados com mais frequência.

Escopo exclusivo para a assinatura

```json
"[uniqueString(subscription().subscriptionId)]"
```

Escopo exclusivo para o grupo de recursos

```json
"[uniqueString(resourceGroup().id)]"
```

Escopo exclusivo para a implantação de um grupo de recursos

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

O exemplo a seguir mostra como criar um nome exclusivo para uma conta de armazenamento com base em seu grupo de recursos. Dentro do grupo de recursos, o nome não é exclusivo se construído da mesma maneira.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

Se você precisar criar um novo nome exclusivo cada vez que implantar um modelo e não pretender atualizar o recurso, poderá usar a função [UtcNow](template-functions-date.md#utcnow) com uniquestring. Você pode usar essa abordagem em um ambiente de teste. Para obter um exemplo, consulte [UtcNow](template-functions-date.md#utcnow).

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres que contém 13 caracteres.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uniquestring.json) abaixo retorna os resultados da cadeia exclusiva:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

## <a name="uri"></a>uri

`uri (baseUri, relativeUri)`

Cria um URI absoluto, combinando o baseUri e a cadeia de caracteres relativeUri.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| baseUri |Sim |cadeia de caracteres |Cadeia de caracteres do URI de base. Tome cuidado para observar o comportamento em relação à manipulação da barra à direita ('/'), conforme descrito a seguir nesta tabela.  |
| relativeUri |Sim |cadeia de caracteres |Cadeia de caracteres de uri relativo para adicionar a cadeia de caracteres do uri de base. |

* Se **BaseUri** terminar em uma barra à direita, o resultado será simplesmente **BaseUri** seguido por **relativeUri**.

* Se **BaseUri** não terminar em uma barra à direita, uma das duas coisas ocorrerá.  

   * Se **BaseUri** não tiver nenhuma barra (além da "//" próxima à frente), o resultado será simplesmente **BaseUri** seguido por **relativeUri**.

   * Se **BaseUri** tiver algumas barras, mas não terminar com uma barra, tudo da última barra em diante será removido do **BaseUri** e o resultado será **BaseUri** seguido por **relativeUri**.
     
Estes são alguns exemplos:

```
uri('http://contoso.org/firstpath', 'myscript.sh') -> http://contoso.org/myscript.sh
uri('http://contoso.org/firstpath/', 'myscript.sh') -> http://contoso.org/firstpath/myscript.sh
uri('http://contoso.org/firstpath/azuredeploy.json', 'myscript.sh') -> http://contoso.org/firstpath/myscript.sh
uri('http://contoso.org/firstpath/azuredeploy.json/', 'myscript.sh') -> http://contoso.org/firstpath/azuredeploy.json/myscript.sh
```
Para obter detalhes completos, os parâmetros **BaseUri** e **relativeUri** são resolvidos conforme especificado na [RFC 3986, seção 5](https://tools.ietf.org/html/rfc3986#section-5).

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres que representa o URI absoluto dos valores base e relativos.

### <a name="examples"></a>Exemplos

O exemplo a seguir mostra como criar um link para um modelo aninhado com base no valor do modelo pai.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) a seguir mostra como usar uri, uriComponent e uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| uriOutput | Cadeia de caracteres | `http://contoso.com/resources/nested/azuredeploy.json` |
| componentOutput | Cadeia de caracteres | `http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json` |
| toStringOutput | Cadeia de caracteres | `http://contoso.com/resources/nested/azuredeploy.json` |

## <a name="uricomponent"></a>uriComponent

`uricomponent(stringToEncode)`

Codifica um URI.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| stringToEncode |Sim |cadeia de caracteres |O valor a ser codificado. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres do valor codificado em URI.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) a seguir mostra como usar uri, uriComponent e uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| uriOutput | Cadeia de caracteres | `http://contoso.com/resources/nested/azuredeploy.json` |
| componentOutput | Cadeia de caracteres | `http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json` |
| toStringOutput | Cadeia de caracteres | `http://contoso.com/resources/nested/azuredeploy.json` |

## <a name="uricomponenttostring"></a>uriComponentToString

`uriComponentToString(uriEncodedString)`

Retorna uma cadeia de caracteres de um valor codificado em URI.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Sim |cadeia de caracteres |O valor codificado em URI a ser convertido em uma cadeia de caracteres. |

### <a name="return-value"></a>Valor retornado

Uma cadeia de caracteres decodificada de valores codificados em URI.

### <a name="examples"></a>Exemplos

O [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) a seguir mostra como usar uri, uriComponent e uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

A saída do exemplo anterior com os valores padrão é:

| Nome | Type | Valor |
| ---- | ---- | ----- |
| uriOutput | Cadeia de caracteres | `http://contoso.com/resources/nested/azuredeploy.json` |
| componentOutput | Cadeia de caracteres | `http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json` |
| toStringOutput | Cadeia de caracteres | `http://contoso.com/resources/nested/azuredeploy.json` |

## <a name="next-steps"></a>Próximas etapas
* Para obter uma descrição das seções em um modelo de Azure Resource Manager, consulte [criando modelos de Azure Resource Manager](template-syntax.md).
* Para mesclar vários modelos, consulte [usando modelos vinculados com Azure Resource Manager](linked-templates.md).
* Para iterar um número especificado de vezes ao criar um tipo de recurso, consulte [criar várias instâncias de recursos no Azure Resource Manager](copy-resources.md).
* Para ver como implantar o modelo que você criou, consulte [Implantar um aplicativo com o Modelo do Azure Resource Manager](deploy-powershell.md).

