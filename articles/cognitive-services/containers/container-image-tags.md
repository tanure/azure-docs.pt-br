---
title: Marcas de imagem de contêiner dos Serviços Cognitivos
titleSuffix: Azure Cognitive Services
description: Uma listagem abrangente de todas as marcas de imagem de contêiner de serviço cognitiva.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 11/17/2020
ms.author: aahi
ms.openlocfilehash: 09a83c28d07540b8ecd813e7ab2f10ceee891d7a
ms.sourcegitcommit: 6a770fc07237f02bea8cc463f3d8cc5c246d7c65
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95792982"
---
# <a name="azure-cognitive-services-container-image-tags-and-release-notes"></a>Marcas de imagem e notas de versão do contêiner de serviços cognitivas do Azure

Os serviços cognitivas do Azure oferecem muitas imagens de contêiner. Os registros de contêiner e os repositórios correspondentes variam entre as imagens de contêiner. Cada nome de imagem de contêiner oferece várias marcas. Uma marca de imagem de contêiner é um mecanismo de controle de versão da imagem de contêiner. Este artigo destina-se a ser usado como uma referência abrangente para listar todas as imagens de contêiner de serviços cognitivas e suas marcas disponíveis.

> [!TIP]
> Ao usar [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) o, preste muita atenção à capitalização do registro de contêiner, do repositório, do nome da imagem de contêiner e da marca correspondente, pois eles diferenciam **maiúsculas de minúsculas**.

## <a name="anomaly-detector"></a>Detector de Anomalias

A imagem de contêiner do [detector de anomalias][ad-containers] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/decision` repositório e é nomeado `anomaly-detector` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector` .

Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/decision/anomaly-detector/tags/list).

# <a name="latest-version"></a>[Última versão](#tab/current)

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013560003-amd64-preview` |      |

# <a name="previous-versions"></a>[Versões anteriores](#tab/previous)

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `1.1.012300001-amd64-preview` |       |

---

## <a name="read-ocr-optical-character-recognition"></a>Ler OCR (reconhecimento óptico de caracteres)

A imagem de contêiner de OCR de [Pesquisa Visual computacional][cv-containers] leitura pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services` repositório e é nomeado `read` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/vision/read` .

Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/vision/read/tags/list).

# <a name="latest-version"></a>[Última versão](#tab/current)

Notas de versão para `3.2-preview.1` :

* Novo contêiner v 3.2

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `latest`                      |       |
| `3.2-preview.1` |  |

# <a name="previous-versions"></a>[Versões anteriores](#tab/previous)

Notas de versão para `v2.0.013250001-amd64-preview` :

* Diminuir ainda mais o uso de memória para o contêiner.
* O cache externo é necessário para a configuração de vários pods. Por exemplo, configure Redis para Caching.
* Correção de resultados ausentes quando o cache Redis está configurado e `ResultExpirationPeriod` definido como 0.
* Remova a limitação do tamanho do corpo da solicitação de 26MB. O contêiner agora pode aceitar >arquivos 26MB.
* Adicione carimbo de data/hora e versão de compilação ao log do console.

Notas de versão do `1.1.013050001-amd64-preview`

* Adicionada `ReadEngineConfig:ResultExpirationPeriod` configuração de inicialização de contêiner para especificar quando o sistema deve limpar os resultados de reconhecimento. 
    * A configuração é em horas, e o valor padrão é de 48 horas.
    * A configuração pode reduzir o uso de memória para o armazenamento de resultados, especialmente quando o armazenamento na memória do contêiner é usado.
    * Exemplo 1. ReadEngineConfig: ResultExpirationPeriod = 1, o sistema limpará o resultado de reconhecimento 1h após o processo.
    * Se essa configuração for definida como 0, o sistema limpará o resultado do reconhecimento depois que o resultado for recuperado.

* Foi corrigido um erro de servidor interno 500 quando um formato de imagem inválido é passado para o sistema. Agora, ele retornará um erro 400:

    ```json
    {
        "error": {
        "code": "InvalidImageSize",
        "message": "Image must be between 1024 and 209715200 bytes."
        }
    }
    ```

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `2.0.013250001-amd64-preview` |       |
| `1.1.013050001-amd64-preview` |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

---


## <a name="form-recognizer"></a>Reconhecimento de Formulários

A imagem de contêiner do [reconhecedor de formulário][fr-containers] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/custom-form` repositório e é nomeado `labeltool` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/custom-form/labeltool` .

Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/custom-form/labeltool/tags/list).

# <a name="latest-version"></a>[Última versão](#tab/current)

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |


# <a name="previous-versions"></a>[Versões anteriores](#tab/previous)

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

---

## <a name="language-understanding-luis"></a>Reconhecimento Vocal (LUIS)

A imagem de contêiner [Luis][lu-containers] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/language` repositório e é nomeado `luis` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/language/luis` .

Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/language/luis/tags/list).

# <a name="latest-version"></a>[Última versão](#tab/current)

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012280003-amd64-preview` |       |


# <a name="previous-version"></a>[Versão anterior](#tab/previous)

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `1.1.012130003-amd64-preview` |       |

---

## <a name="custom-speech-to-text"></a>Fala Personalizada para texto

A imagem de contêiner de [fala personalizada para texto][sp-cstt] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/speechservices/` repositório e é nomeado `custom-speech-to-text` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-speech-to-text` . Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-speech-to-text/tags/list).


# <a name="latest-version"></a>[Última versão](#tab/current)

Nota de versão para `2.7.0-amd64` :

**Recursos**
* A pontuação é definida como habilitada por padrão.

Observe que, devido às listas de frases incluídas, o tamanho dessa imagem de contêiner aumentou.

| Marcas de imagem                    | Observações | Digest                                                                  |
|-------------------------------|:------|:------------------------------------------------------------------------|
| `latest`                      |       | `sha256:d1573c2543cb7afedb0122da0995f345767b02f9c5f181950acf1509ca65726` |
| `2.7.0-amd64`                 |       | `sha256:d1573c2543cb7afedb0122da0995f345767b02f9c5f181950acf1509ca65726` |


# <a name="previous-version"></a>[Versão anterior](#tab/previous)
Nota de versão para `2.6.0-amd64` :

**Recursos**
* Suporte para a expressão v2 
* As listas de frases têm suporte nas seguintes localidades:
    * EN-au
    * EN-AC
    * en-GB
    * EN-in
    * pt-br
    * zh-cn
* Suporte para download de modelo de base personalizado. 
    * Use `BaseModelLocale=<locale>` com o `docker run` comando
* Migrado completamente para o .NET 3,1

**Correções**
* Corrigido um problema em que a pontuação de confiança era sempre 1 no modo Diarization
* Migrado para a API do textanalytics 3,0

Observe que, devido às listas de frases incluídas, o tamanho dessa imagem de contêiner aumentou.

Nota de versão para `2.5.0-amd64` :

**Recursos**
* Suporte a pronúncia personalizada em modelos personalizados
* Suporte para Azure e nuvem do Azure no governo dos EUA

**Correções**
* Corrigir execução como um problema de usuário não raiz no modo Diarization

| Marcas de imagem                    | Observações               |
|-------------------------------|:--------------------|
| `2.6.0-amd64`                 |                     |
| `2.5.0-amd64`                 |   primeira versão GA    |

---

## <a name="custom-text-to-speech"></a>Conversão de texto em fala personalizada

A imagem de contêiner de [texto em fala personalizado][sp-ctts] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/speechservices/` repositório e é nomeado `custom-text-to-speech` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/speechservices/custom-text-to-speech` . Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/custom-text-to-speech/tags/list).


# <a name="latest-version"></a>[Última versão](#tab/current)

Nota de versão para `1.9.0-amd64` :

Versão mensal regular

| Marcas de imagem                    | Observações | Digest                                                                  |
|-------------------------------|:------|:------------------------------------------------------------------------|
| `latest`                      |       | `sha256:e0397cf12d1367b13dd258f782bb513c93afcd5ee4b897794fe533205336355` |
| `1.9.0-amd64`                 |       | `sha256:e0397cf12d1367b13dd258f782bb513c93afcd5ee4b897794fe533205336355` |


# <a name="previous-version"></a>[Versão anterior](#tab/previous)
Nota de versão para `1.8.0-amd64` :

**Recursos**
* Migrado completamente para o .NET 3,1

Nota de versão para `1.7.0-amd64` :

**Recurso**
* Migrado parcialmente para o .NET 3,1

| Marcas de imagem                    | Observações               |
|-------------------------------|:--------------------|
| `1.8.0-amd64`                 |                     |
| `1.7.0-amd64`                 |   primeira versão GA    |

---

## <a name="speech-to-text"></a>Conversão de fala em texto

A imagem de contêiner de [fala em texto][sp-stt] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/speechservices/` repositório e é nomeado `speech-to-text` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/speechservices/speech-to-text` . Você pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/speech-to-text/tags/list).

Desde a 2.5.0 de fala em texto, as imagens têm suporte na região da *Virgínia do governo dos EUA* . Use o ponto de extremidade de cobrança do *Virgínia do governo dos EUA* e as chaves de API ao usar essa região.

# <a name="latest-version"></a>[Última versão](#tab/current)

Nota de versão para `2.7.0-amd64-<locale>` :

**Recursos**
* Suporte para as seguintes novas localidades:
    * ar-BH, ar-IQ, ar-Jo, ar-lb, ar-OM, ar-Sy
    * bg-bg
    * el-gr
    * EN-HK, en-IE, en-pH, en-SG, en-za
    * es-ar, es-Bo, es-CL, es-co, es-CR, es-cu, es-do, es-EC, es-gt, es-PA, es-PE, es-PR, es-py, es-VA, es-US, es-uy, es-ve
    * et-ee
    * GA-IE
    * hr-hr
    * hu-hu
    * lt-lt
    * lv-lv
    * MT-MT
    * ro-ro
    * sk-sk
    * SL-SL
* A pontuação é habilitada por padrão.

Observe que, devido às listas de frases incluídas, o tamanho dessa imagem de contêiner aumentou. 

| Marcas de imagem                    | Observações                                                                                                |
|-------------------------------|:-----------------------------------------------------------------------------------------------------|
| `latest`                      | Imagem de contêiner com a `en-US` localidade.                                                             |
| `2.7.0-amd64-<locale>`        | Substitua `<locale>` por uma das localidades disponíveis, listadas abaixo. Por exemplo, `2.7.0-amd64-en-us`. |

Esse contêiner tem as seguintes localidades disponíveis.

| Localidade para v 2.7.0           | Observações                                    | Digest                                                                  |
|-----------------------------|:-----------------------------------------|:------------------------------------------------------------------------|
| `ar-ae`                     | Imagem de contêiner com a `ar-AE` localidade. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-bh`                     | Imagem de contêiner com a `ar-BH` localidade. | `sha256:5a2c140661f50d0c95587121ec1ab8895289f4dda5b3ad14074413e869e6bd4` |
| `ar-eg`                     | Imagem de contêiner com a `ar-EG` localidade. | `sha256:783bb8321fcfb7890b0c99935099f7e84c85a698c2fe0031c661e265358d79c` |
| `ar-iq`                     | Imagem de contêiner com a `ar-IQ` localidade. | `sha256:abd0101f73c1cf71f30da7b11b93d2a7ac8877dbfcfc2d34553d20705aca7a2` |
| `ar-jo`                     | Imagem de contêiner com a `ar-JO` localidade. | `sha256:d4c7fd2a1637e163aa106c23b6a759e8c78366c60ece83b3aabfe93ebabae07` |
| `ar-kw`                     | Imagem de contêiner com a `ar-KW` localidade. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-lb`                     | Imagem de contêiner com a `ar-LB` localidade. | `sha256:20e5c9105e86625c72de54290a6eb07630d35c3760f729c4b855e3661583dfe` |
| `ar-om`                     | Imagem de contêiner com a `ar-OM` localidade. | `sha256:97f1b44f2cbb837a2ef86441a0a52a07f706240edb6ef6618ee4db8cbbe1c19` |
| `ar-qa`                     | Imagem de contêiner com a `ar-QA` localidade. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sa`                     | Imagem de contêiner com a `ar-SA` localidade. | `sha256:c8e99e71e6740cf671f3bf79de8b7dd890122cb674eedd2440e71e7cbc4c66b` |
| `ar-sy`                     | Imagem de contêiner com a `ar-SY` localidade. | `sha256:51980a2e2c3dd3548deedcedaf5fc688db602a5eced1a4b7df7d10750393623` |
| `bg-bg`                     | Imagem de contêiner com a `bg-BG` localidade. | `sha256:1c1acf0fbb353ebb04692f37eb4d4cdf0b4e309720dd7e709001dada0d1ea81` |
| `ca-es`                     | Imagem de contêiner com a `ca-ES` localidade. | `sha256:c60baa0007f61c7652b97b49645215de63411125d627c974c09222e316df204` |
| `cs-cz`                     | Imagem de contêiner com a `cs-CZ` localidade. | `sha256:3fa09fc3a6bde6b77df2444aae8fc78b5f25fb9010171d1682db116ea5801f5` |
| `da-dk`                     | Imagem de contêiner com a `da-DK` localidade. | `sha256:4b26dbba50c2771943880b68e0e4ea0713d0e3bb8bad884454849bccc9e94a3` |
| `de-de`                     | Imagem de contêiner com a `de-DE` localidade. | `sha256:5109ed80b1fecf4db0328adcd50528d0aa9e726b5fc84587c40aaea4e91256d` |
| `el-gr`                     | Imagem de contêiner com a `el-GR` localidade. | `sha256:fc8b466c588bf097efac2b79454d5ac0df5c6990398f07ede9be7e1d536e4bd` |
| `en-au`                     | Imagem de contêiner com a `en-AU` localidade. | `sha256:3461892a27fc3eb3f9610b2def00bc15f380c6b9797c90ceca19e6abb55f6a6` |
| `en-ca`                     | Imagem de contêiner com a `en-CA` localidade. | `sha256:a0509be39785f1e869bd96ab10e7c07d3f4e61c9aa17ff5900076e7bd64ba11` |
| `en-gb`                     | Imagem de contêiner com a `en-GB` localidade. | `sha256:1b976fc7ac109e61dcf74af3652c12535e3db92931d2d0bb2ea59bd46f9efed` |
| `en-hk`                     | Imagem de contêiner com a `en-HK` localidade. | `sha256:0b1e1df101f978869c98f6e50632712016b8311fc89b334e7f44e968d64bf2f` |
| `en-ie`                     | Imagem de contêiner com a `en-IE` localidade. | `sha256:c5ba0d3c7219ce39f0b918a51a7cae8a65c277f564279cad920e068725aa39f` |
| `en-in`                     | Imagem de contêiner com a `en-IN` localidade. | `sha256:e907f07be498f024103f6fe6abffa23e242bf3585724741b29a2f3f41d0899c` |
| `en-nz`                     | Imagem de contêiner com a `en-NZ` localidade. | `sha256:66845f6ce20ae71d609867c6eb4772366ce042499e4bcdce4c1b579daf7fad7` |
| `en-ph`                     | Imagem de contêiner com a `en-PH` localidade. | `sha256:e7874653bf66b1a1ab344b3391eb8767be34260b7f11b62fd057cbe17b805b2` |
| `en-sg`                     | Imagem de contêiner com a `en-SG` localidade. | `sha256:827cdb158280e6f4037f4815410c7aa78abf9c6467876c1504aecfef787bdd7` |
| `en-us`                     | Imagem de contêiner com a `en-US` localidade. | `sha256:248d17340055e3e137219ddc234c605e6a53ceead136ea55c9697c352da6a8d` |
| `en-za`                     | Imagem de contêiner com a `en-ZA` localidade. | `sha256:a8abc99f498db7088bb25acec47da81e90b6a5eaa1c6f78e0f9a314d839d0ae` |
| `es-ar`                     | Imagem de contêiner com a `es-AR` localidade. | `sha256:edf78429630851b6eb01f54f8a8a1aeeda9971c6a834403a204662eda22b3b9` |
| `es-bo`                     | Imagem de contêiner com a `es-BO` localidade. | `sha256:5832b44f1da2f6b9a097c99babfbc370d8d0eabe1ff8daabec2c3f482dc9d63` |
| `es-cl`                     | Imagem de contêiner com a `es-CL` localidade. | `sha256:409a712b96235e154472134f96ff9272265f1e5b555e00ad03c2260b0781009` |
| `es-co`                     | Imagem de contêiner com a `es-CO` localidade. | `sha256:99792bc083dc16e0edf15491e6a840d786c9140b747551563a8d98f66f0b415` |
| `es-cr`                     | Imagem de contêiner com a `es-CR` localidade. | `sha256:21fe14a538e5b8b2d288b00b8f5a02d87469e285f32e725155042079f336ac9` |
| `es-cu`                     | Imagem de contêiner com a `es-CU` localidade. | `sha256:05d40eae01cec4c42c4febd379cd61373eb43d0aacfd47b988bb95e6a6ad216` |
| `es-do`                     | Imagem de contêiner com a `es-DO` localidade. | `sha256:73dd0e0d4f39a259563ee7cc18c2e72c9ab20c52905fe343e0413ca7c4b3f0d` |
| `es-ec`                     | Imagem de contêiner com a `es-EC` localidade. | `sha256:c3e69139ef365fe9332b5b68b43458242c7dad9d9f2b557431272306e81cb9e` |
| `es-es`                     | Imagem de contêiner com a `es-ES` localidade. | `sha256:bd83fcfc116ba645a0e12a7a93b6ada74a8f701172f826a91c5f223a1dbaa61` |
| `es-gt`                     | Imagem de contêiner com a `es-GT` localidade. | `sha256:5bb9b18b91b74e123e3720893d88bfcb0a87dac31a1f7171d23c7cb1fa09fee` |
| `es-hn`                     | Imagem de contêiner com a `es-HN` localidade. | `sha256:941d108a4b76eb554e8f13cf5090665a702de3ebf35b75e4350f0916dfccd72` |
| `es-mx`                     | Imagem de contêiner com a `es-MX` localidade. | `sha256:cebea03732781b4425500d162ae6580bbd7ce9b5f4ede988c4570fe311d8567` |
| `es-ni`                     | Imagem de contêiner com a `es-NI` localidade. | `sha256:8ba165f94ad840936ebd0af17a0a63aa08a6292e7ad9029f5b93eef41165eb9` |
| `es-pa`                     | Imagem de contêiner com a `es-PA` localidade. | `sha256:c61b7f1b6801a03c3eab0dd1aede87017a86bc7368ded2f8bad8d9e5f60d0d3` |
| `es-pe`                     | Imagem de contêiner com a `es-PE` localidade. | `sha256:447a3ab3f302aba24d201d9f5b2877ffcd64dfd5e9d6b88d9924847160b2de2` |
| `es-pr`                     | Imagem de contêiner com a `es-PR` localidade. | `sha256:a53b3295c986e91ee8cf93ebe1057b997c76ef7f99913508b859311a194fdd4` |
| `es-py`                     | Imagem de contêiner com a `es-PY` localidade. | `sha256:85b3f75e75e63e29521daf772ee68a59ac2428579512501aa81dc51a2315652` |
| `es-sv`                     | Imagem de contêiner com a `es-SV` localidade. | `sha256:db5ece7ba536e38d5de59cd37807630ab76589dcf1c97e253f98d7f44d9424e` |
| `es-us`                     | Imagem de contêiner com a `es-US` localidade. | `sha256:99f2743725bb71e25543484f49bcfde14584ccbbaaa912678938d69d965075a` |
| `es-uy`                     | Imagem de contêiner com a `es-UY` localidade. | `sha256:a3e11c16a97a1ae76408d812b2fee1e4b3ba07160bbcb62a22814523568ee5d` |
| `es-ve`                     | Imagem de contêiner com a `es-VE` localidade. | `sha256:8cb431aafd84263ead8de946377c1d3f2ddfa7e172b8a4c5aa7ba477c5b41f0` |
| `et-ee`                     | Imagem de contêiner com a `et-EE` localidade. | `sha256:943e7cf894e9d75341a58993104824c1c8cd8da1322cc5a732e9d53882c6523` |
| `fi-fi`                     | Imagem de contêiner com a `fi-FI` localidade. | `sha256:35658e9dce796cb96a1371f250398e86351ea1b5ada080da7ce8471b30c7cae` |
| `fr-ca`                     | Imagem de contêiner com a `fr-CA` localidade. | `sha256:62256cad671e8baa03fdd4c5f4eca7d5c5effedd64cafd9020ba72c9c4210e0` |
| `fr-fr`                     | Imagem de contêiner com a `fr-FR` localidade. | `sha256:b385993232d9daa327d1a7b067268927b17f36eed3e8d423748794544c62746` |
| `ga-ie`                     | Imagem de contêiner com a `ga-IE` localidade. | `sha256:ab9abdb993b0f7487edda8200f1393ac44ba4888c0f444a02afb6c85ca3e393` |
| `gu-in`                     | Imagem de contêiner com a `gu-IN` localidade. | `sha256:328e69488f2948722d7ccc97e266071f61a8c9f65cd671688490955806526de` |
| `hi-in`                     | Imagem de contêiner com a `hi-IN` localidade. | `sha256:b9b0bfec80aa53d06ea2cbd9097f753ec5caaf00ac2f00321ae7ad916fd7fa6` |
| `hr-hr`                     | Imagem de contêiner com a `hr-HR` localidade. | `sha256:ab849cd2eeea682f8958bba8986fe90f0f7bb3b447512a10cf464e8e1ce4ea5` |
| `hu-hu`                     | Imagem de contêiner com a `hu-HU` localidade. | `sha256:30f239b155d91523442cf74a1f2732304fa2b50ae7b786833bb6a020b982621` |
| `it-it`                     | Imagem de contêiner com a `it-IT` localidade. | `sha256:288f95413870eb9d33bf1dabfa6fbd6b55b0faa52e4d5face3171d1dd4ddbdd` |
| `ja-jp`                     | Imagem de contêiner com a `ja-JP` localidade. | `sha256:e3ab37a80c215dec565eca212f57eb81887fc2894452868dff92e3bd42c4bb9` |
| `ko-kr`                     | Imagem de contêiner com a `ko-KR` localidade. | `sha256:c1208b8459333b606af516cd7806e9d4d5e002247bb1225e1f246563b356890` |
| `lt-lt`                     | Imagem de contêiner com a `lt-LT` localidade. | `sha256:8dec331161d3c29fc65ba6651fcc6cfe69fa314519f408b5f9f8eb27da09830` |
| `lv-lv`                     | Imagem de contêiner com a `lv-LV` localidade. | `sha256:7cf31282910b339666bb2b0a555caa7fc6ae414eea4423a41f35c3527f83235` |
| `mr-in`                     | Imagem de contêiner com a `mr-IN` localidade. | `sha256:9cb012bd58ef7723d4905d6fa3c1fde96e33c354b3d96d4e3ff69cf6e1bfe3a` |
| `mt-mt`                     | Imagem de contêiner com a `mt-MT` localidade. | `sha256:a0094c032ea555b168ec5751ab3257337d902d526e9ae335671fb751a352378` |
| `nb-no`                     | Imagem de contêiner com a `nb-NO` localidade. | `sha256:6bbc326e20a6a785b1ca33143b42a060858efb67b863a267d6efb7aebb48f87` |
| `nl-nl`                     | Imagem de contêiner com a `nl-NL` localidade. | `sha256:94b4ddf4cc80fa666e422f8416aea3f98ebe4842dfe9b1f4bfea7c47eb61127` |
| `pl-pl`                     | Imagem de contêiner com a `pl-PL` localidade. | `sha256:58e5f78bf772c3c8cbd5f0c5d6e67f5348e04e3f893d84738a2a3e964bab256` |
| `pt-br`                     | Imagem de contêiner com a `pt-BR` localidade. | `sha256:f500ef956bd28807f40df1f9f0520e437c5084f61a3be6d1379e746887d5b7c` |
| `pt-pt`                     | Imagem de contêiner com a `pt-PT` localidade. | `sha256:c841d2dbe5f40adf6039242c106985febb1a44212feb55d9769fe31134ec116` |
| `ro-ro`                     | Imagem de contêiner com a `ro-RO` localidade. | `sha256:93271c39c0a134e987a069c2a65289acff9869ae0d90fdcb39928c9ef0fd86b` |
| `ru-ru`                     | Imagem de contêiner com a `ru-RU` localidade. | `sha256:8d6b3c600e56cc96813b8c14b7916c5539a20ba561dc1c6d5bbef6285d6eef6` |
| `sk-sk`                     | Imagem de contêiner com a `sk-SK` localidade. | `sha256:6d604092cc6c964663a1c97d91c8f1c8cf4b46d07427d03f7041c0cc55eb521` |
| `sl-si`                     | Imagem de contêiner com a `sl-SI` localidade. | `sha256:f237ed58fedefcc749e74be1258cc70e5a690ee6c5a6b6388bd24075faa61da` |
| `sv-se`                     | Imagem de contêiner com a `sv-SE` localidade. | `sha256:da4233e6658b00eefdadb9d4acd889c6550a5e2a4a7af7a9f915c878abd4c9c` |
| `ta-in`                     | Imagem de contêiner com a `ta-IN` localidade. | `sha256:22b77606d25e9c2f52bf3cad6218782b4719f6a9dcfadc770468d266758a56c` |
| `te-in`                     | Imagem de contêiner com a `te-IN` localidade. | `sha256:7f4d11372862ca1d65fc9b868e2d775701b8e6eabd786c90c4e9ab82ba86e88` |
| `th-th`                     | Imagem de contêiner com a `th-TH` localidade. | `sha256:69033bcd7c0f59d31bafec6c2b7a9ff343928cdd58c16105415c291d555d37b` |
| `tr-tr`                     | Imagem de contêiner com a `tr-TR` localidade. | `sha256:4b7d339846a0d371dfe25aa2e626f131003c01329c9a1da468eb3703ef176ea` |
| `zh-cn`                     | Imagem de contêiner com a `zh-CN` localidade. | `sha256:a428459830fb766083212f71c5638a65ce30d8dd84f6c624ae22768e8a76976` |
| `zh-hk`                     | Imagem de contêiner com a `zh-HK` localidade. | `sha256:7a2903462b67336a6ce4c8e2faac42052f0a4392d1d5eb3839758cc8d0429f1` |
| `zh-tw`                     | Imagem de contêiner com a `zh-TW` localidade. | `sha256:30fd2b3660e047d24a46fbba14ba282f15bc0339ec93f49afd0d02ff4069146` |


# <a name="previous-version"></a>[Versão anterior](#tab/previous)

Nota de versão para `2.6.0-amd64-<locale>` :

**Recursos**
* Atualizado para os modelos mais recentes e migrado totalmente para o .NET 3,1
* Suporte para a expressão v2
* As listas de frases têm suporte nas seguintes localidades:
    * EN-au
    * EN-AC
    * en-GB
    * EN-in
    * pt-br
    * zh-cn
* Suporte para nova localidade `cs-CZ` 
    * Não há suporte para capitalização e pontuação no momento.

**Correções**
* Corrige um problema em que as pontuações de confiança eram sempre 1 no modo Diarization
* Migrado use a API do textanalytics 3,0

Observe que, devido às listas de frases incluídas, o tamanho dessa imagem de contêiner aumentou. 

Nota de versão para `2.5.0-amd64-<locale>` :

**Recursos**
* Suporte para a nuvem do governo dos EUA do Azure

**Correções**
* Corrige um problema com a execução como um usuário não raiz no modo Diarization

| Marcas de imagem                  | Observações                                    |
|-----------------------------|:-----------------------------------------|
| `2.6.0-amd64-<locale>`      | Substitua `<locale>` por uma das localidades disponíveis, listadas abaixo. Por exemplo, `2.6.0-amd64-en-us`. |
| `2.5.0-amd64-<locale>`      | Substitua `<locale>` por uma das localidades disponíveis, listadas abaixo. Por exemplo, `2.5.0-amd64-en-us`. |


Esse contêiner tem as seguintes localidades disponíveis.

| Localidade para v 2.6.0           | Observações                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Imagem de contêiner com a `ar-AE` localidade. |
| `ar-eg`                     | Imagem de contêiner com a `ar-EG` localidade. |
| `ar-kw`                     | Imagem de contêiner com a `ar-KW` localidade. |
| `ar-qa`                     | Imagem de contêiner com a `ar-QA` localidade. |
| `ar-sa`                     | Imagem de contêiner com a `ar-SA` localidade. |
| `ca-es`                     | Imagem de contêiner com a `ca-ES` localidade. |
| `cs-cz`                     | Imagem de contêiner com a `cs-CZ` localidade. |
| `da-dk`                     | Imagem de contêiner com a `da-DK` localidade. |
| `de-de`                     | Imagem de contêiner com a `de-DE` localidade. |
| `en-au`                     | Imagem de contêiner com a `en-AU` localidade. |
| `en-ca`                     | Imagem de contêiner com a `en-CA` localidade. |
| `en-gb`                     | Imagem de contêiner com a `en-GB` localidade. |
| `en-in`                     | Imagem de contêiner com a `en-IN` localidade. |
| `en-nz`                     | Imagem de contêiner com a `en-NZ` localidade. |
| `en-us`                     | Imagem de contêiner com a `en-US` localidade. |
| `es-es`                     | Imagem de contêiner com a `es-ES` localidade. |
| `es-mx`                     | Imagem de contêiner com a `es-MX` localidade. |
| `fi-fi`                     | Imagem de contêiner com a `fi-FI` localidade. |
| `fr-ca`                     | Imagem de contêiner com a `fr-CA` localidade. |
| `fr-fr`                     | Imagem de contêiner com a `fr-FR` localidade. |
| `gu-in`                     | Imagem de contêiner com a `gu-IN` localidade. |
| `hi-in`                     | Imagem de contêiner com a `hi-IN` localidade. |
| `it-it`                     | Imagem de contêiner com a `it-IT` localidade. |
| `ja-jp`                     | Imagem de contêiner com a `ja-JP` localidade. |
| `ko-kr`                     | Imagem de contêiner com a `ko-KR` localidade. |
| `mr-in`                     | Imagem de contêiner com a `mr-IN` localidade. |
| `nb-no`                     | Imagem de contêiner com a `nb-NO` localidade. |
| `nl-nl`                     | Imagem de contêiner com a `nl-NL` localidade. |
| `pl-pl`                     | Imagem de contêiner com a `pl-PL` localidade. |
| `pt-br`                     | Imagem de contêiner com a `pt-BR` localidade. |
| `pt-pt`                     | Imagem de contêiner com a `pt-PT` localidade. |
| `ru-ru`                     | Imagem de contêiner com a `ru-RU` localidade. |
| `sv-se`                     | Imagem de contêiner com a `sv-SE` localidade. |
| `ta-in`                     | Imagem de contêiner com a `ta-IN` localidade. |
| `te-in`                     | Imagem de contêiner com a `te-IN` localidade. |
| `th-th`                     | Imagem de contêiner com a `th-TH` localidade. |
| `tr-tr`                     | Imagem de contêiner com a `tr-TR` localidade. |
| `zh-cn`                     | Imagem de contêiner com a `zh-CN` localidade. |
| `zh-hk`                     | Imagem de contêiner com a `zh-HK` localidade. |
| `zh-tw`                     | Imagem de contêiner com a `zh-TW` localidade. |

| Localidade para v 2.5.0           | Observações                                    |
|-----------------------------|:-----------------------------------------|
| `ar-ae`                     | Imagem de contêiner com a `ar-AE` localidade. |
| `ar-eg`                     | Imagem de contêiner com a `ar-EG` localidade. |
| `ar-kw`                     | Imagem de contêiner com a `ar-KW` localidade. |
| `ar-qa`                     | Imagem de contêiner com a `ar-QA` localidade. |
| `ar-sa`                     | Imagem de contêiner com a `ar-SA` localidade. |
| `ca-es`                     | Imagem de contêiner com a `ca-ES` localidade. |
| `da-dk`                     | Imagem de contêiner com a `da-DK` localidade. |
| `de-de`                     | Imagem de contêiner com a `de-DE` localidade. |
| `en-au`                     | Imagem de contêiner com a `en-AU` localidade. |
| `en-ca`                     | Imagem de contêiner com a `en-CA` localidade. |
| `en-gb`                     | Imagem de contêiner com a `en-GB` localidade. |
| `en-in`                     | Imagem de contêiner com a `en-IN` localidade. |
| `en-nz`                     | Imagem de contêiner com a `en-NZ` localidade. |
| `en-us`                     | Imagem de contêiner com a `en-US` localidade. |
| `es-es`                     | Imagem de contêiner com a `es-ES` localidade. |
| `es-mx`                     | Imagem de contêiner com a `es-MX` localidade. |
| `fi-fi`                     | Imagem de contêiner com a `fi-FI` localidade. |
| `fr-ca`                     | Imagem de contêiner com a `fr-CA` localidade. |
| `fr-fr`                     | Imagem de contêiner com a `fr-FR` localidade. |
| `gu-in`                     | Imagem de contêiner com a `gu-IN` localidade. |
| `hi-in`                     | Imagem de contêiner com a `hi-IN` localidade. |
| `it-it`                     | Imagem de contêiner com a `it-IT` localidade. |
| `ja-jp`                     | Imagem de contêiner com a `ja-JP` localidade. |
| `ko-kr`                     | Imagem de contêiner com a `ko-KR` localidade. |
| `mr-in`                     | Imagem de contêiner com a `mr-IN` localidade. |
| `nb-no`                     | Imagem de contêiner com a `nb-NO` localidade. |
| `nl-nl`                     | Imagem de contêiner com a `nl-NL` localidade. |
| `pl-pl`                     | Imagem de contêiner com a `pl-PL` localidade. |
| `pt-br`                     | Imagem de contêiner com a `pt-BR` localidade. |
| `pt-pt`                     | Imagem de contêiner com a `pt-PT` localidade. |
| `ru-ru`                     | Imagem de contêiner com a `ru-RU` localidade. |
| `sv-se`                     | Imagem de contêiner com a `sv-SE` localidade. |
| `ta-in`                     | Imagem de contêiner com a `ta-IN` localidade. |
| `te-in`                     | Imagem de contêiner com a `te-IN` localidade. |
| `th-th`                     | Imagem de contêiner com a `th-TH` localidade. |
| `tr-tr`                     | Imagem de contêiner com a `tr-TR` localidade. |
| `zh-cn`                     | Imagem de contêiner com a `zh-CN` localidade. |
| `zh-hk`                     | Imagem de contêiner com a `zh-HK` localidade. |
| `zh-tw`                     | Imagem de contêiner com a `zh-TW` localidade. |

---

## <a name="text-to-speech"></a>Conversão de texto em fala

A imagem de contêiner de [conversão de texto em fala][sp-tts] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/speechservices/` repositório e é nomeado `text-to-speech` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/speechservices/text-to-speech` .

Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/text-to-speech/tags/list).


# <a name="latest-version"></a>[Última versão](#tab/current)

Nota de versão para `1.9.0-amd64-<locale-and-voice>` :

* Versão mensal regular

| Marcas de imagem                                  | Observações                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `latest`                                    | Imagem de contêiner com a `en-US` localidade e a `en-US-AriaRUS` voz.                                            | 
| `1.9.0-amd64-<locale-and-voice>`            | Substitua `<locale>` por uma das localidades disponíveis, listadas abaixo. Por exemplo, `1.9.0-amd64-en-us-ariarus`.  |


| Localidades para v 1.9.0                          | Observações                                                                      | Digest                         |
|---------------------------------------------|:---------------------------------------------------------------------------|:-------------------------------|
| `ar-eg-hoda`                                | Imagem de contêiner com a `ar-EG` localidade e a `ar-EG-Hoda` voz.            | `sha256:2b19cfd2212d6517b286aa18617d2f9d1dd1520078b559cbbf9240599270d10` | 
| `ar-sa-naayf`                               | Imagem de contêiner com a `ar-SA` localidade e a `ar-SA-Naayf` voz.           | `sha256:6063aae5fb15c62b234cf945220916516a06ca81354c5311dee02af4d8cb0d3` |
| `bg-bg-ivan`                                | Imagem de contêiner com a `bg-BG` localidade e a `bg-BG-Ivan` voz.            | `sha256:c6786916464755e64ffa64e69e8f3e7ef16115bac00bb6ea1e45368c42c58d1` |
| `ca-es-herenarus`                           | Imagem de contêiner com a `ca-ES` localidade e a `ca-ES-HerenaRUS` voz.       | `sha256:2a8a1accbf99e2746c9345b77e2f261e0111227312c402cc2e1cd8760cdc82a` |
| `cs-cz-jakub`                               | Imagem de contêiner com a `cs-CZ` localidade e a `cs-CZ-Jakub` voz.           | `sha256:3e464356bb08c9c966af2b28a88ccafd591aecd2e37a0fedb356bd443720e8d` |
| `da-dk-hellerus`                            | Imagem de contêiner com a `da-DK` localidade e a `da-DK-HelleRUS` voz.        | `sha256:b85c43080804103673ff99dddea644a516c4103e8b1f11fa3dd34857492cd40` |
| `de-at-michael`                             | Imagem de contêiner com a `de-AT` localidade e a `de-AT-Michael` voz.         | `sha256:87b57ee61f964e4d72e75d860c499fa3b3d8dbda6a96c97d696beb20aa8b2a9` |
| `de-ch-karsten`                             | Imagem de contêiner com a `de-CH` localidade e a `de-CH-Karsten` voz.         | `sha256:ab1385b9746f4f054204302b9d564a433ae03748021b8ed71b4a3a224af1e9b` |
| `de-de-heddarus`                            | Imagem de contêiner com a `de-DE` localidade e a `de-DE-Hedda` voz.           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-hedda`                               | Imagem de contêiner com a `de-DE` localidade e a `de-DE-Hedda` voz.           | `sha256:82185a710c87f9dde678d88036867559ab3bf5f08f234d60d1548d3e106db57` |
| `de-de-stefan-apollo`                       | Imagem de contêiner com a `de-DE` localidade e a `de-DE-Stefan-Apollo` voz.   | `sha256:56a1c63e7e6a0f5623ddc1f6a44ac6e51471d073e02e14e8c8b1e577930d816` |
| `el-gr-stefanos`                            | Imagem de contêiner com a `el-GR` localidade e a `el-GR-Stefanos` voz.        | `sha256:ccbbb09f29ff8f276e246037183c7a3e9a3eb5bf33a942b22205cce3c6857f2` |
| `en-au-catherine`                           | Imagem de contêiner com a `en-AU` localidade e a `en-AU-Catherine` voz.       | `sha256:0c7374890f963e1ae9507e89dc9965a94723bd57802826c0677cd5262189783` |
| `en-au-hayleyrus`                           | Imagem de contêiner com a `en-AU` localidade e a `en-AU-HayleyRUS` voz.       | `sha256:7430bf8eace8294ca085f36ea56399261b2b4f69027e86649e8f3868fc3d811` |
| `en-ca-heatherrus`                          | Imagem de contêiner com a `en-CA` localidade e a `en-CA-HeatherRUS` voz.      | `sha256:0166ce1de3d669ea4ad80738c63369b7032125a54ecabade07241d740a94cfe` |
| `en-ca-linda`                               | Imagem de contêiner com a `en-CA` localidade e a `en-CA-Linda` voz.           | `sha256:50bed6a7bde9b793d307bcc3ace4c0f28d4a33c7a4dad9b3a394dc39a3e1c28` |
| `en-gb-george-apollo`                       | Imagem de contêiner com a `en-GB` localidade e a `en-GB-George-Apollo` voz.   | `sha256:50b800c0018a39609ddb1cee1b10062bf38a907644c393d20786db7c3ade748` |
| `en-gb-hazelrus`                            | Imagem de contêiner com a `en-GB` localidade e a `en-GB-HazelRUS` voz.        | `sha256:2aa79394dfeac8cec0cc1704a5199949cfccf347fe61161d02c7000c4ffcfa6` |
| `en-gb-susan-apollo`                        | Imagem de contêiner com a `en-GB` localidade e a `en-GB-Susan-Apollo` voz.    | `sha256:7a3174b3aae5f10241e731d392b56f124808cdd506f881ced919ced73d836c0` |
| `en-ie-sean`                                | Imagem de contêiner com a `en-IE` localidade e a `en-IE-Sean` voz.            | `sha256:2457202fadb2354fc8d3666432096bd87c07760a4e3f4dbcc49853fff658577` |
| `en-in-heera-apollo`                        | Imagem de contêiner com a `en-IN` localidade e a `en-IN-Heera-Apollo` voz.    | `sha256:e4068cd7ca4272ea94819e2ba8743d2a76c8710b162db5e9ecbde6c92c12877` |
| `en-in-priyarus`                            | Imagem de contêiner com a `en-IN` localidade e a `en-IN-PriyaRUS` voz.        | `sha256:9d63a0ed53ac06178ab84588551421c0e1d04b8bad3321410ebb99c3ca2a9e8` |
| `en-in-ravi-apollo`                         | Imagem de contêiner com a `en-IN` localidade e a `en-IN-Ravi-Apollo` voz.     | `sha256:67049c9ce591336655943f5030afcfdaa150a8aace7b372425a69cc33a6b7b9` |
| `en-us-aria24krus`                          | Imagem de contêiner com a `en-US` localidade e a `en-US-Aria24kRUS` voz.      | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-ariarus`                             | Imagem de contêiner com a `en-US` localidade e a `en-US-AriaRUS` voz.         | `sha256:a95acf6874bf3df7ae8e96be779f80cb5405d21250227b0c4b3ddbcb3014082` |
| `en-us-benjaminrus`                         | Imagem de contêiner com a `en-US` localidade e a `en-US-BenjaminRUS` voz.     | `sha256:93cd49adaaa2a1bdfb06ab655be164ae66f206cb7c03a2cbd59e5fba70610ab` |
| `en-us-guy24krus`                           | Imagem de contêiner com a `en-US` localidade e a `en-US-Guy24kRUS` voz.       | `sha256:7b788bfcaae4c63c274ca15924bfd861cfcafd5fec13f685d80babc25b2949d` |
| `en-us-zirarus`                             | Imagem de contêiner com a `en-US` localidade e a `en-US-ZiraRUS` voz.         | `sha256:bfc87a77df5695ad43481348500fba8f6a7b495708fba200706049469b5ba97` |
| `es-es-helenarus`                           | Imagem de contêiner com a `es-ES` localidade e a `es-ES-HelenaRUS` voz.       | `sha256:0b6c17aca75efb64aa9bfc0d83303038fe58d4b2fb1fc94c9380a4335b80796` |
| `es-es-laura-apollo`                        | Imagem de contêiner com a `es-ES` localidade e a `es-ES-Laura-Apollo` voz.    | `sha256:d6fcffc944c37a2dd0de29c39b82f3f8cce3a95ad925d2814ed7538335d5d4f` |
| `es-es-pablo-apollo`                        | Imagem de contêiner com a `es-ES` localidade e a `es-ES-Pablo-Apollo` voz.    | `sha256:a460bc53d9083d3c3770129995cf96cc1069ae4e8101f1739d304fe210f0af0` |
| `es-mx-hildarus`                            | Imagem de contêiner com a `es-MX` localidade e a `es-MX-HildaRUS` voz.        | `sha256:5b7578fc5b00158dfa674d95a3f1d57f22eb285e8333b4006d1fe1808bda7ba` |
| `es-mx-raul-apollo`                         | Imagem de contêiner com a `es-MX` localidade e a `es-MX-Raul-Apollo` voz.     | `sha256:03922fb017783c86d788c72e01c7ede440f8f3c913c86cab19bad4dfc2e4a2b` |
| `fi-fi-heidirus`                            | Imagem de contêiner com a `fi-FI` localidade e a `fi-FI-HeidiRUS` voz.        | `sha256:146c1f98d6fa061016eba41db6e7b654eef222d37f35406d4b43477bb2ff897` |
| `fr-ca-caroline`                            | Imagem de contêiner com a `fr-CA` localidade e a `fr-CA-Caroline` voz.        | `sha256:1ee2e53f12ad1c72665d2aef64e9d4a7f9ea05670cad84dcae5e75409494f32` |
| `fr-ca-harmonierus`                         | Imagem de contêiner com a `fr-CA` localidade e a `fr-CA-HarmonieRUS` voz.     | `sha256:a21d25d3ac699af4e9ba9194aadd9b45f35fd9205224f3429a4c7da41fc38fe` |
| `fr-ch-guillaume`                           | Imagem de contêiner com a `fr-CH` localidade e a `fr-CH-Guillaume` voz.       | `sha256:216125a9bd89a95d3c4dc2d7e031398659427b3aa7d4663d23a65737972e42b` |
| `fr-fr-hortenserus`                         | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-HortenseRUS` voz.     | `sha256:795a698120eecbd80c48e738f73300739c1698ca859130ddb4236317bcdf70f` |
| `fr-fr-julie-apollo`                        | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-Julie-Apollo` voz.    | `sha256:f6eb70d523c435c2e3a713b32a8af4a781df7ec043caad2fc7f458ee341eb2f` |
| `fr-fr-paul-apollo`                         | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-Paul-Apollo` voz.     | `sha256:28864c662a20f459b3051b1da2967a605e06267e6408285f7c2552748cf4eed` |
| `he-il-asaf`                                | Imagem de contêiner com a `he-IL` localidade e a `he-IL-Asaf` voz.            | `sha256:eaa834bac6b69abef096b36a8baead741db78fe438af3d30f60abde3631d639` |
| `hi-in-hemant`                              | Imagem de contêiner com a `hi-IN` localidade e a `hi-IN-Hemant` voz.          | `sha256:cfea0fa7cce9cc512f2fbb8b76f1c00fe5c32fad853c90b15934cf4ee6262fa` |
| `hi-in-kalpana-apollo`                      | Imagem de contêiner com a `hi-IN` localidade e a `hi-IN-Kalpana-Apollo` voz.  | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hi-in-kalpana`                             | Imagem de contêiner com a `hi-IN` localidade e a `hi-IN-Kalpana` voz.         | `sha256:afbd6cc0413f3a3c9f6df044b6df6d9dac9e8e888c2cb619fefbdc3e105c644` |
| `hr-hr-matej`                               | Imagem de contêiner com a `hr-HR` localidade e a `hr-HR-Matej` voz.           | `sha256:86683597c62752b4d769b69e5294979fafd4c277aaef1536e1cb19f9f06c0bf` |
| `hu-hu-szabolcs`                            | Imagem de contêiner com a `hu-HU` localidade e a `hu-HU-Szabolcs` voz.        | `sha256:aa64eed28ca2ad060e2e02188e0401bf34e4caf7e2182b70a30ce33b3c11c9c` |
| `id-id-andika`                              | Imagem de contêiner com a `id-ID` localidade e a `id-ID-Andika` voz.          | `sha256:0e1394d231a57a1df8163ccb634dc2ef2f8103b10608a40ab3efc5c0fbe9ded` |
| `it-it-cosimo-apollo`                       | Imagem de contêiner com a `it-IT` localidade e a `it-IT-Cosimo-Apollo` voz.   | `sha256:eef97f2817fc24405823a5fe4e825244db32279b44c0e6631e8ad9a5c1acf40` |
| `it-it-luciarus`                            | Imagem de contêiner com a `it-IT` localidade e a `it-IT-LuciaRUS` voz.        | `sha256:ebc331b0685f482d2f55619fa81fd451fd7c8f107f9cd7ad159bc6213ae4e33` |
| `ja-jp-ayumi-apollo`                        | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-Ayumi-Apollo` voz.    | `sha256:e9cb7dfd2eec154c8f3d530c16b66e8558c5955a2edaede69740067f00e43cf` |
| `ja-jp-harukarus`                           | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-HarukaRUS` voz.       | `sha256:93ce2ef6177c0d8ac70b61df8b11fcbcdfd3c0be0cc51cd8644f26679a741c2` |
| `ja-jp-ichiro-apollo`                       | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-Ichiro-Apollo` voz.   | `sha256:6a18bae69ac63b42ba992b8b74d8d31d91ca984d61b5f62f38be988cf38645e` |
| `ko-kr-heamirus`                            | Imagem de contêiner com a `ko-KR` localidade e a `ko-KR-HeamiRUS` voz.        | `sha256:7a48252d4ada2af43f9266a70113426d330bac192348cbdc929022295a0e727` |
| `ms-my-rizwan`                              | Imagem de contêiner com a `ms-MY` localidade e a `ms-MY-Rizwan` voz.          | `sha256:90e2ecac14f8e960934fd013d208fc2a0afe1bfff037d5648d422bda8d8a76e` |
| `nb-no-huldarus`                            | Imagem de contêiner com a `nb-NO` localidade e a `nb-NO-HuldaRUS` voz.        | `sha256:217b61bd6244b5effda8f12a2c563ce1b4572e9c5b8a08df143665f9ff754e4` |
| `nl-nl-hannarus`                            | Imagem de contêiner com a `nl-NL` localidade e a `nl-NL-HannaRUS` voz.        | `sha256:fbff48dfc9dfadadf377867b28f6e3a3bd605e59da20f77a531efcc7d85d16e` |
| `pl-pl-paulinarus`                          | Imagem de contêiner com a `pl-PL` localidade e a `pl-PL-PaulinaRUS` voz.      | `sha256:856a033a09925773fa4b4531e199ab7c03c537f366acecbda60f8d21735725e` |
| `pt-br-daniel-apollo`                       | Imagem de contêiner com a `pt-BR` localidade e a `pt-BR-Daniel-Apollo` voz.   | `sha256:2d1ec975f1aee56a6fc6039d154fb3f2fbeb4636f7078c5dfe99aeddb6a3634` |
| `pt-br-heloisarus`                          | Imagem de contêiner com a `pt-BR` localidade e a `pt-BR-HeloisaRUS` voz.      | `sha256:b7d629f37ab3305274764264dc08fab5236e60ef18d40e987618115db67ce44` |
| `pt-pt-heliarus`                            | Imagem de contêiner com a `pt-PT` localidade e a `pt-PT-HeliaRUS` voz.        | `sha256:8b380ae7e4aac9d4ada4d15fa9e667387bc9ca038796d9b6999953bfbc97259` |
| `ro-ro-andrei`                              | Imagem de contêiner com a `ro-RO` localidade e a `ro-RO-Andrei` voz.          | `sha256:b00ca7f1411169a5baf7263a8d7e5eed1a72084d9489eaf458429dfc338564a` |
| `ru-ru-ekaterinarus`                        | Imagem de contêiner com a `ru-RU` localidade e a `ru-RU-EkaterinaRUS` voz.    | `sha256:31c588c31e3ac67305af66091e7756dfc4ca454317d0228116ea0b2fedf5d71` |
| `ru-ru-irina-apollo`                        | Imagem de contêiner com a `ru-RU` localidade e a `ru-RU-Irina-Apollo` voz.    | `sha256:e76437f8da7c279b38d2643defc997a13b4a364e9a212895cdb33a9a3f6457f` |
| `ru-ru-pavel-apollo`                        | Imagem de contêiner com a `ru-RU` localidade e a `ru-RU-Pavel-Apollo` voz.    | `sha256:461c1efa6cce0b10a87f338bc637aca76aef8458061a688870fb3343d682da0` |
| `sk-sk-filip`                               | Imagem de contêiner com a `sk-SK` localidade e a `sk-SK-Filip` voz.           | `sha256:7fb0cfab4c0fe2913eb20f28a25c6663015d62f82e7e7864d9f7fac2d27697b` |
| `sl-si-lado`                                | Imagem de contêiner com a `sl-SI` localidade e a `sl-SI-Lado` voz.            | `sha256:5336173d410e10ffeb5dc211a583887e33754319c757914955057d398dfbb0a` |
| `sv-se-hedvigrus`                           | Imagem de contêiner com a `sv-SE` localidade e a `sv-SE-HedvigRUS` voz.       | `sha256:5dc8cdcc3054386bf69596707d9d261d4db5bfd09f1882ceb4e29238a34b24e` |
| `ta-in-valluvar`                            | Imagem de contêiner com a `ta-IN` localidade e a `ta-IN-Valluvar` voz.        | `sha256:74ea485f23e4c1fe0029e06894860aa0188c36c0e14ea3584a06d4216ccef56` |
| `te-in-chitra`                              | Imagem de contêiner com a `te-IN` localidade e a `te-IN-Chitra` voz.          | `sha256:ff2977a98ef691da543db08be9cfe04d7fc3bf8f78b29310c163e47303b2ddd` |
| `th-th-pattara`                             | Imagem de contêiner com a `th-TH` localidade e a `th-TH-Pattara` voz.         | `sha256:ba7e2c0e5e75d9f2b52aa50c97728616c43e81f48c15e24665e4c2ea5770a8f` |
| `tr-tr-sedarus`                             | Imagem de contêiner com a `tr-TR` localidade e a `tr-TR-SedaRUS` voz.         | `sha256:375a8ceae89ea1f0dda551feff30ae3679231189b527992edbc49988d042d66` |
| `vi-vn-an`                                  | Imagem de contêiner com a `vi-VN` localidade e a `vi-VN-An` voz.              | `sha256:b6f82148295b38b4039c45c48695ec50b4e97cd02b18d49c39bf9fca3bec958` |
| `zh-cn-huihuirus`                           | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-HuihuiRUS` voz.       | `sha256:3e773931f3adaac92cba43773a241692a2b471ebe73ec51c475df8ff63b7ee1` |
| `zh-cn-kangkang-apollo`                     | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-Kangkang-Apollo` voz. | `sha256:05fc0d5075a1094caf70d98b4a9469952be52cb6eb4d9f7b9ff4ae961100c7b` |
| `zh-cn-yaoyao-apollo`                       | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-Yaoyao-Apollo` voz.   | `sha256:d7613bcefc48e85b9d6f07c8cd223c16d4958bcf7f24087575250e97c593ac1` |
| `zh-hk-danny-apollo`                        | Imagem de contêiner com a `zh-HK` localidade e a `zh-HK-Danny-Apollo` voz.    | `sha256:efe22bc123dac9312dcaeb859a377d81f61fbb25ef46e4678d36ec6bebc5d32` |
| `zh-hk-tracy-apollo`                        | Imagem de contêiner com a `zh-HK` localidade e a `zh-HK-Tracy-Apollo` voz.    | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-hk-tracyrus`                            | Imagem de contêiner com a `zh-HK` localidade e a `zh-HK-TracyRUS` voz.        | `sha256:802c60bc65012c03ffe96268dca79b8c6dcd0c5cc6180ec271c50ef5c9ba132` |
| `zh-tw-hanhanrus`                           | Imagem de contêiner com a `zh-TW` localidade e a `zh-TW-HanHanRUS` voz.       | `sha256:95d58922463d577d4c4722ab722a5768af35fb62236d47f6709717dea758909` |
| `zh-tw-yating-apollo`                       | Imagem de contêiner com a `zh-TW` localidade e a `zh-TW-Yating-Apollo` voz.   | `sha256:33eec6e3aaaedafaf3969746eeaf97a1760e763505decfe2abaa03f5054bfd2` |
| `zh-tw-zhiwei-apollo`                       | Imagem de contêiner com a `zh-TW` localidade e a `zh-TW-Zhiwei-Apollo` voz.   | `sha256:456db2898b2e5a9c30b7071ce6ea3f141438cbf1aa4899c7ffccfc2f0dde5bd` |


# <a name="previous-version"></a>[Versão anterior](#tab/previous)

Nota de versão para `1.8.0-amd64-<locale-and-voice>` :

**Recurso**

* Migrado completamente para o .NET 3,1

Nota de versão para `1.7.0-amd64-<locale-and-voice>` :

**Recurso**

* Componentes atualizados para o .NET 3,1

| Marcas de imagem                                  | Observações                                                                                                         |
|---------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `1.8.0-amd64-<locale-and-voice>`            | Substitua `<locale>` por uma das localidades disponíveis, listadas abaixo. Por exemplo, `1.8.0-amd64-en-us-ariarus`.  |
| `1.7.0-amd64-<locale-and-voice>`            | primeira versão GA. Substitua `<locale>` por uma das localidades disponíveis, listadas abaixo. Por exemplo, `1.7.0-amd64-en-us-ariarus`.  |


| Localidades para v 1.8.0                          | Observações                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Imagem de contêiner com a `ar-EG` localidade e a `ar-EG-Hoda` voz.            |
| `ar-sa-naayf`                               | Imagem de contêiner com a `ar-SA` localidade e a `ar-SA-Naayf` voz.           |
| `bg-bg-ivan`                                | Imagem de contêiner com a `bg-BG` localidade e a `bg-BG-Ivan` voz.            |
| `ca-es-herenarus`                           | Imagem de contêiner com a `ca-ES` localidade e a `ca-ES-HerenaRUS` voz.       |
| `cs-cz-jakub`                               | Imagem de contêiner com a `cs-CZ` localidade e a `cs-CZ-Jakub` voz.           |
| `da-dk-hellerus`                            | Imagem de contêiner com a `da-DK` localidade e a `da-DK-HelleRUS` voz.        |
| `de-at-michael`                             | Imagem de contêiner com a `de-AT` localidade e a `de-AT-Michael` voz.         |
| `de-ch-karsten`                             | Imagem de contêiner com a `de-CH` localidade e a `de-CH-Karsten` voz.         |
| `de-de-hedda`                               | Imagem de contêiner com a `de-DE` localidade e a `de-DE-Hedda` voz.           |
| `de-de-heddarus`                            | Imagem de contêiner com a `de-DE` localidade e a `de-DE-Hedda` voz.           |
| `de-de-stefan-apollo`                       | Imagem de contêiner com a `de-DE` localidade e a `de-DE-Stefan-Apollo` voz.   |
| `el-gr-stefanos`                            | Imagem de contêiner com a `el-GR` localidade e a `el-GR-Stefanos` voz.        |
| `en-au-catherine`                           | Imagem de contêiner com a `en-AU` localidade e a `en-AU-Catherine` voz.       |
| `en-au-hayleyrus`                           | Imagem de contêiner com a `en-AU` localidade e a `en-AU-HayleyRUS` voz.       |
| `en-ca-heatherrus`                          | Imagem de contêiner com a `en-CA` localidade e a `en-CA-HeatherRUS` voz.      |
| `en-ca-linda`                               | Imagem de contêiner com a `en-CA` localidade e a `en-CA-Linda` voz.           |
| `en-gb-george-apollo`                       | Imagem de contêiner com a `en-GB` localidade e a `en-GB-George-Apollo` voz.   |
| `en-gb-hazelrus`                            | Imagem de contêiner com a `en-GB` localidade e a `en-GB-HazelRUS` voz.        |
| `en-gb-susan-apollo`                        | Imagem de contêiner com a `en-GB` localidade e a `en-GB-Susan-Apollo` voz.    |
| `en-ie-sean`                                | Imagem de contêiner com a `en-IE` localidade e a `en-IE-Sean` voz.            |
| `en-in-heera-apollo`                        | Imagem de contêiner com a `en-IN` localidade e a `en-IN-Heera-Apollo` voz.    |
| `en-in-priyarus`                            | Imagem de contêiner com a `en-IN` localidade e a `en-IN-PriyaRUS` voz.        |
| `en-in-ravi-apollo`                         | Imagem de contêiner com a `en-IN` localidade e a `en-IN-Ravi-Apollo` voz.     |
| `en-us-benjaminrus`                         | Imagem de contêiner com a `en-US` localidade e a `en-US-BenjaminRUS` voz.     |
| `en-us-guy24krus`                           | Imagem de contêiner com a `en-US` localidade e a `en-US-Guy24kRUS` voz.       |
| `en-us-aria24krus`                          | Imagem de contêiner com a `en-US` localidade e a `en-US-Aria24kRUS` voz.      |
| `en-us-ariarus`                             | Imagem de contêiner com a `en-US` localidade e a `en-US-AriaRUS` voz.         |
| `en-us-zirarus`                             | Imagem de contêiner com a `en-US` localidade e a `en-US-ZiraRUS` voz.         |
| `es-es-helenarus`                           | Imagem de contêiner com a `es-ES` localidade e a `es-ES-HelenaRUS` voz.       |
| `es-es-laura-apollo`                        | Imagem de contêiner com a `es-ES` localidade e a `es-ES-Laura-Apollo` voz.    |
| `es-es-pablo-apollo`                        | Imagem de contêiner com a `es-ES` localidade e a `es-ES-Pablo-Apollo` voz.    |
| `es-mx-hildarus`                            | Imagem de contêiner com a `es-MX` localidade e a `es-MX-HildaRUS` voz.        |
| `es-mx-raul-apollo`                         | Imagem de contêiner com a `es-MX` localidade e a `es-MX-Raul-Apollo` voz.     |
| `fi-fi-heidirus`                            | Imagem de contêiner com a `fi-FI` localidade e a `fi-FI-HeidiRUS` voz.        |
| `fr-ca-caroline`                            | Imagem de contêiner com a `fr-CA` localidade e a `fr-CA-Caroline` voz.        |
| `fr-ca-harmonierus`                         | Imagem de contêiner com a `fr-CA` localidade e a `fr-CA-HarmonieRUS` voz.     |
| `fr-ch-guillaume`                           | Imagem de contêiner com a `fr-CH` localidade e a `fr-CH-Guillaume` voz.       |
| `fr-fr-hortenserus`                         | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-HortenseRUS` voz.     |
| `fr-fr-julie-apollo`                        | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-Julie-Apollo` voz.    |
| `fr-fr-paul-apollo`                         | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-Paul-Apollo` voz.     |
| `he-il-asaf`                                | Imagem de contêiner com a `he-IL` localidade e a `he-IL-Asaf` voz.            |
| `hi-in-hemant`                              | Imagem de contêiner com a `hi-IN` localidade e a `hi-IN-Hemant` voz.          |
| `hi-in-kalpana-apollo`                      | Imagem de contêiner com a `hi-IN` localidade e a `hi-IN-Kalpana-Apollo` voz.  |
| `hi-in-kalpana`                             | Imagem de contêiner com a `hi-IN` localidade e a `hi-IN-Kalpana` voz.         |
| `hr-hr-matej`                               | Imagem de contêiner com a `hr-HR` localidade e a `hr-HR-Matej` voz.           |
| `hu-hu-szabolcs`                            | Imagem de contêiner com a `hu-HU` localidade e a `hu-HU-Szabolcs` voz.        |
| `id-id-andika`                              | Imagem de contêiner com a `id-ID` localidade e a `id-ID-Andika` voz.          |
| `it-it-cosimo-apollo`                       | Imagem de contêiner com a `it-IT` localidade e a `it-IT-Cosimo-Apollo` voz.   |
| `it-it-luciarus`                            | Imagem de contêiner com a `it-IT` localidade e a `it-IT-LuciaRUS` voz.        |
| `ja-jp-ayumi-apollo`                        | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-Ayumi-Apollo` voz.    |
| `ja-jp-harukarus`                           | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-HarukaRUS` voz.       |
| `ja-jp-ichiro-apollo`                       | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-Ichiro-Apollo` voz.   |
| `ko-kr-heamirus`                            | Imagem de contêiner com a `ko-KR` localidade e a `ko-KR-HeamiRUS` voz.        |
| `ms-my-rizwan`                              | Imagem de contêiner com a `ms-MY` localidade e a `ms-MY-Rizwan` voz.          |
| `nb-no-huldarus`                            | Imagem de contêiner com a `nb-NO` localidade e a `nb-NO-HuldaRUS` voz.        |
| `nl-nl-hannarus`                            | Imagem de contêiner com a `nl-NL` localidade e a `nl-NL-HannaRUS` voz.        |
| `pl-pl-paulinarus`                          | Imagem de contêiner com a `pl-PL` localidade e a `pl-PL-PaulinaRUS` voz.      |
| `pt-br-daniel-apollo`                       | Imagem de contêiner com a `pt-BR` localidade e a `pt-BR-Daniel-Apollo` voz.   |
| `pt-br-heloisarus`                          | Imagem de contêiner com a `pt-BR` localidade e a `pt-BR-HeloisaRUS` voz.      |
| `pt-pt-heliarus`                            | Imagem de contêiner com a `pt-PT` localidade e a `pt-PT-HeliaRUS` voz.        |
| `ro-ro-andrei`                              | Imagem de contêiner com a `ro-RO` localidade e a `ro-RO-Andrei` voz.          |
| `ru-ru-ekaterinarus`                        | Imagem de contêiner com a `ru-RU` localidade e a `ru-RU-EkaterinaRUS` voz.    |
| `ru-ru-irina-apollo`                        | Imagem de contêiner com a `ru-RU` localidade e a `ru-RU-Irina-Apollo` voz.    |
| `ru-ru-pavel-apollo`                        | Imagem de contêiner com a `ru-RU` localidade e a `ru-RU-Pavel-Apollo` voz.    |
| `sk-sk-filip`                               | Imagem de contêiner com a `sk-SK` localidade e a `sk-SK-Filip` voz.           |
| `sl-si-lado`                                | Imagem de contêiner com a `sl-SI` localidade e a `sl-SI-Lado` voz.            |
| `sv-se-hedvigrus`                           | Imagem de contêiner com a `sv-SE` localidade e a `sv-SE-HedvigRUS` voz.       |
| `ta-in-valluvar`                            | Imagem de contêiner com a `ta-IN` localidade e a `ta-IN-Valluvar` voz.        |
| `te-in-chitra`                              | Imagem de contêiner com a `te-IN` localidade e a `te-IN-Chitra` voz.          |
| `th-th-pattara`                             | Imagem de contêiner com a `th-TH` localidade e a `th-TH-Pattara` voz.         |
| `tr-tr-sedarus`                             | Imagem de contêiner com a `tr-TR` localidade e a `tr-TR-SedaRUS` voz.         |
| `vi-vn-an`                                  | Imagem de contêiner com a `vi-VN` localidade e a `vi-VN-An` voz.              |
| `zh-cn-huihuirus`                           | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-HuihuiRUS` voz.       |
| `zh-cn-kangkang-apollo`                     | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-Kangkang-Apollo` voz. |
| `zh-cn-yaoyao-apollo`                       | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-Yaoyao-Apollo` voz.   |
| `zh-hk-danny-apollo`                        | Imagem de contêiner com a `zh-HK` localidade e a `zh-HK-Danny-Apollo` voz.    |
| `zh-hk-tracy-apollo`                        | Imagem de contêiner com a `zh-HK` localidade e a `zh-HK-Tracy-Apollo` voz.    |
| `zh-hk-tracyrus`                            | Imagem de contêiner com a `zh-HK` localidade e a `zh-HK-TracyRUS` voz.        |
| `zh-tw-hanhanrus`                           | Imagem de contêiner com a `zh-TW` localidade e a `zh-TW-HanHanRUS` voz.       |
| `zh-tw-yating-apollo`                       | Imagem de contêiner com a `zh-TW` localidade e a `zh-TW-Yating-Apollo` voz.   |
| `zh-tw-zhiwei-apollo`                       | Imagem de contêiner com a `zh-TW` localidade e a `zh-TW-Zhiwei-Apollo` voz.   |

| Localidades para v 1.7.0                          | Observações                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `ar-eg-hoda`                                | Imagem de contêiner com a `ar-EG` localidade e a `ar-EG-Hoda` voz.            |
| `ar-sa-naayf`                               | Imagem de contêiner com a `ar-SA` localidade e a `ar-SA-Naayf` voz.           |
| `bg-bg-ivan`                                | Imagem de contêiner com a `bg-BG` localidade e a `bg-BG-Ivan` voz.            |
| `ca-es-herenarus`                           | Imagem de contêiner com a `ca-ES` localidade e a `ca-ES-HerenaRUS` voz.       |
| `cs-cz-jakub`                               | Imagem de contêiner com a `cs-CZ` localidade e a `cs-CZ-Jakub` voz.           |
| `da-dk-hellerus`                            | Imagem de contêiner com a `da-DK` localidade e a `da-DK-HelleRUS` voz.        |
| `de-at-michael`                             | Imagem de contêiner com a `de-AT` localidade e a `de-AT-Michael` voz.         |
| `de-ch-karsten`                             | Imagem de contêiner com a `de-CH` localidade e a `de-CH-Karsten` voz.         |
| `de-de-hedda`                               | Imagem de contêiner com a `de-DE` localidade e a `de-DE-Hedda` voz.           |
| `de-de-heddarus`                            | Imagem de contêiner com a `de-DE` localidade e a `de-DE-Hedda` voz.           |
| `de-de-stefan-apollo`                       | Imagem de contêiner com a `de-DE` localidade e a `de-DE-Stefan-Apollo` voz.   |
| `el-gr-stefanos`                            | Imagem de contêiner com a `el-GR` localidade e a `el-GR-Stefanos` voz.        |
| `en-au-catherine`                           | Imagem de contêiner com a `en-AU` localidade e a `en-AU-Catherine` voz.       |
| `en-au-hayleyrus`                           | Imagem de contêiner com a `en-AU` localidade e a `en-AU-HayleyRUS` voz.       |
| `en-ca-heatherrus`                          | Imagem de contêiner com a `en-CA` localidade e a `en-CA-HeatherRUS` voz.      |
| `en-ca-linda`                               | Imagem de contêiner com a `en-CA` localidade e a `en-CA-Linda` voz.           |
| `en-gb-george-apollo`                       | Imagem de contêiner com a `en-GB` localidade e a `en-GB-George-Apollo` voz.   |
| `en-gb-hazelrus`                            | Imagem de contêiner com a `en-GB` localidade e a `en-GB-HazelRUS` voz.        |
| `en-gb-susan-apollo`                        | Imagem de contêiner com a `en-GB` localidade e a `en-GB-Susan-Apollo` voz.    |
| `en-ie-sean`                                | Imagem de contêiner com a `en-IE` localidade e a `en-IE-Sean` voz.            |
| `en-in-heera-apollo`                        | Imagem de contêiner com a `en-IN` localidade e a `en-IN-Heera-Apollo` voz.    |
| `en-in-priyarus`                            | Imagem de contêiner com a `en-IN` localidade e a `en-IN-PriyaRUS` voz.        |
| `en-in-ravi-apollo`                         | Imagem de contêiner com a `en-IN` localidade e a `en-IN-Ravi-Apollo` voz.     |
| `en-us-benjaminrus`                         | Imagem de contêiner com a `en-US` localidade e a `en-US-BenjaminRUS` voz.     |
| `en-us-guy24krus`                           | Imagem de contêiner com a `en-US` localidade e a `en-US-Guy24kRUS` voz.       |
| `en-us-aria24krus`                          | Imagem de contêiner com a `en-US` localidade e a `en-US-Aria24kRUS` voz.      |
| `en-us-ariarus`                             | Imagem de contêiner com a `en-US` localidade e a `en-US-AriaRUS` voz.         |
| `en-us-zirarus`                             | Imagem de contêiner com a `en-US` localidade e a `en-US-ZiraRUS` voz.         |
| `es-es-helenarus`                           | Imagem de contêiner com a `es-ES` localidade e a `es-ES-HelenaRUS` voz.       |
| `es-es-laura-apollo`                        | Imagem de contêiner com a `es-ES` localidade e a `es-ES-Laura-Apollo` voz.    |
| `es-es-pablo-apollo`                        | Imagem de contêiner com a `es-ES` localidade e a `es-ES-Pablo-Apollo` voz.    |
| `es-mx-hildarus`                            | Imagem de contêiner com a `es-MX` localidade e a `es-MX-HildaRUS` voz.        |
| `es-mx-raul-apollo`                         | Imagem de contêiner com a `es-MX` localidade e a `es-MX-Raul-Apollo` voz.     |
| `fi-fi-heidirus`                            | Imagem de contêiner com a `fi-FI` localidade e a `fi-FI-HeidiRUS` voz.        |
| `fr-ca-caroline`                            | Imagem de contêiner com a `fr-CA` localidade e a `fr-CA-Caroline` voz.        |
| `fr-ca-harmonierus`                         | Imagem de contêiner com a `fr-CA` localidade e a `fr-CA-HarmonieRUS` voz.     |
| `fr-ch-guillaume`                           | Imagem de contêiner com a `fr-CH` localidade e a `fr-CH-Guillaume` voz.       |
| `fr-fr-hortenserus`                         | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-HortenseRUS` voz.     |
| `fr-fr-julie-apollo`                        | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-Julie-Apollo` voz.    |
| `fr-fr-paul-apollo`                         | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-Paul-Apollo` voz.     |
| `he-il-asaf`                                | Imagem de contêiner com a `he-IL` localidade e a `he-IL-Asaf` voz.            |
| `hi-in-hemant`                              | Imagem de contêiner com a `hi-IN` localidade e a `hi-IN-Hemant` voz.          |
| `hi-in-kalpana-apollo`                      | Imagem de contêiner com a `hi-IN` localidade e a `hi-IN-Kalpana-Apollo` voz.  |
| `hi-in-kalpana`                             | Imagem de contêiner com a `hi-IN` localidade e a `hi-IN-Kalpana` voz.         |
| `hr-hr-matej`                               | Imagem de contêiner com a `hr-HR` localidade e a `hr-HR-Matej` voz.           |
| `hu-hu-szabolcs`                            | Imagem de contêiner com a `hu-HU` localidade e a `hu-HU-Szabolcs` voz.        |
| `id-id-andika`                              | Imagem de contêiner com a `id-ID` localidade e a `id-ID-Andika` voz.          |
| `it-it-cosimo-apollo`                       | Imagem de contêiner com a `it-IT` localidade e a `it-IT-Cosimo-Apollo` voz.   |
| `it-it-luciarus`                            | Imagem de contêiner com a `it-IT` localidade e a `it-IT-LuciaRUS` voz.        |
| `ja-jp-ayumi-apollo`                        | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-Ayumi-Apollo` voz.    |
| `ja-jp-harukarus`                           | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-HarukaRUS` voz.       |
| `ja-jp-ichiro-apollo`                       | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-Ichiro-Apollo` voz.   |
| `ko-kr-heamirus`                            | Imagem de contêiner com a `ko-KR` localidade e a `ko-KR-HeamiRUS` voz.        |
| `ms-my-rizwan`                              | Imagem de contêiner com a `ms-MY` localidade e a `ms-MY-Rizwan` voz.          |
| `nb-no-huldarus`                            | Imagem de contêiner com a `nb-NO` localidade e a `nb-NO-HuldaRUS` voz.        |
| `nl-nl-hannarus`                            | Imagem de contêiner com a `nl-NL` localidade e a `nl-NL-HannaRUS` voz.        |
| `pl-pl-paulinarus`                          | Imagem de contêiner com a `pl-PL` localidade e a `pl-PL-PaulinaRUS` voz.      |
| `pt-br-daniel-apollo`                       | Imagem de contêiner com a `pt-BR` localidade e a `pt-BR-Daniel-Apollo` voz.   |
| `pt-br-heloisarus`                          | Imagem de contêiner com a `pt-BR` localidade e a `pt-BR-HeloisaRUS` voz.      |
| `pt-pt-heliarus`                            | Imagem de contêiner com a `pt-PT` localidade e a `pt-PT-HeliaRUS` voz.        |
| `ro-ro-andrei`                              | Imagem de contêiner com a `ro-RO` localidade e a `ro-RO-Andrei` voz.          |
| `ru-ru-ekaterinarus`                        | Imagem de contêiner com a `ru-RU` localidade e a `ru-RU-EkaterinaRUS` voz.    |
| `ru-ru-irina-apollo`                        | Imagem de contêiner com a `ru-RU` localidade e a `ru-RU-Irina-Apollo` voz.    |
| `ru-ru-pavel-apollo`                        | Imagem de contêiner com a `ru-RU` localidade e a `ru-RU-Pavel-Apollo` voz.    |
| `sk-sk-filip`                               | Imagem de contêiner com a `sk-SK` localidade e a `sk-SK-Filip` voz.           |
| `sl-si-lado`                                | Imagem de contêiner com a `sl-SI` localidade e a `sl-SI-Lado` voz.            |
| `sv-se-hedvigrus`                           | Imagem de contêiner com a `sv-SE` localidade e a `sv-SE-HedvigRUS` voz.       |
| `ta-in-valluvar`                            | Imagem de contêiner com a `ta-IN` localidade e a `ta-IN-Valluvar` voz.        |
| `te-in-chitra`                              | Imagem de contêiner com a `te-IN` localidade e a `te-IN-Chitra` voz.          |
| `th-th-pattara`                             | Imagem de contêiner com a `th-TH` localidade e a `th-TH-Pattara` voz.         |
| `tr-tr-sedarus`                             | Imagem de contêiner com a `tr-TR` localidade e a `tr-TR-SedaRUS` voz.         |
| `vi-vn-an`                                  | Imagem de contêiner com a `vi-VN` localidade e a `vi-VN-An` voz.              |
| `zh-cn-huihuirus`                           | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-HuihuiRUS` voz.       |
| `zh-cn-kangkang-apollo`                     | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-Kangkang-Apollo` voz. |
| `zh-cn-yaoyao-apollo`                       | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-Yaoyao-Apollo` voz.   |
| `zh-hk-danny-apollo`                        | Imagem de contêiner com a `zh-HK` localidade e a `zh-HK-Danny-Apollo` voz.    |
| `zh-hk-tracy-apollo`                        | Imagem de contêiner com a `zh-HK` localidade e a `zh-HK-Tracy-Apollo` voz.    |
| `zh-hk-tracyrus`                            | Imagem de contêiner com a `zh-HK` localidade e a `zh-HK-TracyRUS` voz.        |
| `zh-tw-hanhanrus`                           | Imagem de contêiner com a `zh-TW` localidade e a `zh-TW-HanHanRUS` voz.       |
| `zh-tw-yating-apollo`                       | Imagem de contêiner com a `zh-TW` localidade e a `zh-TW-Yating-Apollo` voz.   |
| `zh-tw-zhiwei-apollo`                       | Imagem de contêiner com a `zh-TW` localidade e a `zh-TW-Zhiwei-Apollo` voz.   |

---

## <a name="neural-text-to-speech"></a>Texto em fala neural

A imagem de contêiner de [texto em fala neural][sp-ntts] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/speechservices/` repositório e é nomeado `neural-text-to-speech` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/speechservices/neural-text-to-speech` .

Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/neural-text-to-speech/tags/list).


# <a name="latest-version"></a>[Última versão](#tab/current)

Notas de versão para `v1.3.0` :
* O contêiner de texto em fala neural agora está disponível para o público geral. 

| Marcas de imagem                                  | Observações                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Imagem de contêiner com a `en-US` localidade e a `en-US-AriaNeural` voz.      |
| `1.3.0-amd64-<locale-and-voice>`    | Substitua `<locale>` por uma das localidades disponíveis, listadas abaixo. Por exemplo, `1.3.0-amd64-en-us-arianeural`. |


| v 1.3.0 localidades e vozes           | Observações                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `de-de-katjaneural`                 | Imagem de contêiner com a `de-DE` localidade e a `de-DE-KatjaNeural` voz.     |
| `en-au-natashaneural`               | Imagem de contêiner com a `en-AU` localidade e a `en-AU-NatashaNeural` voz.   |
| `en-ca-claraneural`                 | Imagem de contêiner com a `en-CA` localidade e a `en-CA-ClaraNeural` voz.     |
| `en-gb-libbyneural`                 | Imagem de contêiner com a `en-GB` localidade e a `en-GB-LibbyNeural` voz.     |
| `en-gb-mianeural`                   | Imagem de contêiner com a `en-GB` localidade e a `en-GB-MiaNeural` voz.       |
| `en-us-arianeural`                  | Imagem de contêiner com a `en-US` localidade e a `en-US-AriaNeural` voz.      |
| `en-us-guyneural`                   | Imagem de contêiner com a `en-US` localidade e a `en-US-GuyNeural` voz.       |
| `es-es-elviraneural`                | Imagem de contêiner com a `es-ES` localidade e a `es-ES-ElviraNeural` voz.    |
| `es-mx-dalianeural`                 | Imagem de contêiner com a `es-MX` localidade e a `es-MX-DaliaNeural` voz.     |
| `fr-ca-sylvieneural`                | Imagem de contêiner com a `fr-CA` localidade e a `fr-CA-SylvieNeural` voz.    |
| `fr-fr-deniseneural`                | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-DeniseNeural` voz.    |
| `it-it-elsaneural`                  | Imagem de contêiner com a `it-IT` localidade e a `it-IT-ElsaNeural` voz.      |
| `ja-jp-nanamineural`                | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-NanamiNeural` voz.    |
| `ko-kr-sunhineural`                 | Imagem de contêiner com a `ko-KR` localidade e a `ko-KR-SunHiNeural` voz.     |
| `pt-br-franciscaneural`             | Imagem de contêiner com a `pt-BR` localidade e a `pt-BR-FranciscaNeural` voz. |
| `zh-cn-xiaoxiaoneural`              | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-XiaoxiaoNeural` voz.  |

# <a name="previous-version"></a>[Versão anterior](#tab/previous)

| Marcas de imagem                                  | Observações                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Imagem de contêiner com a `en-US` localidade e a `en-US-AriaNeural` voz.      |
| `1.2.0-amd64-<locale-and-voice>-preview`    | Substitua `<locale>` por uma das localidades disponíveis, listadas abaixo. Por exemplo, `1.2.0-amd64-en-us-arianeural-preview`. |


| 1.2.0 e as localidades da visualização v           | Observações                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Imagem de contêiner com a `en-US` localidade e a `en-US-AriaNeural` voz.      |
| `de-de-katjaneural-preview`                 | Imagem de contêiner com a `de-DE` localidade e a `de-DE-KatjaNeural` voz.     |
| `en-au-natashaneural-preview`               | Imagem de contêiner com a `en-AU` localidade e a `en-AU-NatashaNeural` voz.   |
| `en-ca-claraneural-preview`                 | Imagem de contêiner com a `en-CA` localidade e a `en-CA-ClaraNeural` voz.     |
| `en-gb-libbyneural-preview`                 | Imagem de contêiner com a `en-GB` localidade e a `en-GB-LibbyNeural` voz.     |
| `en-gb-mianeural-preview`                   | Imagem de contêiner com a `en-GB` localidade e a `en-GB-MiaNeural` voz.       |
| `en-us-arianeural-preview`                  | Imagem de contêiner com a `en-US` localidade e a `en-US-AriaNeural` voz.      |
| `en-us-guyneural-preview`                   | Imagem de contêiner com a `en-US` localidade e a `en-US-GuyNeural` voz.       |
| `es-es-elviraneural-preview`                | Imagem de contêiner com a `es-ES` localidade e a `es-ES-ElviraNeural` voz.    |
| `es-mx-dalianeural-preview`                 | Imagem de contêiner com a `es-MX` localidade e a `es-MX-DaliaNeural` voz.     |
| `fr-ca-sylvieneural-preview`                | Imagem de contêiner com a `fr-CA` localidade e a `fr-CA-SylvieNeural` voz.    |
| `fr-fr-deniseneural-preview`                | Imagem de contêiner com a `fr-FR` localidade e a `fr-FR-DeniseNeural` voz.    |
| `it-it-elsaneural-preview`                  | Imagem de contêiner com a `it-IT` localidade e a `it-IT-ElsaNeural` voz.      |
| `ja-jp-nanamineural-preview`                | Imagem de contêiner com a `ja-JP` localidade e a `ja-JP-NanamiNeural` voz.    |
| `ko-kr-sunhineural-preview`                 | Imagem de contêiner com a `ko-KR` localidade e a `ko-KR-SunHiNeural` voz.     |
| `pt-br-franciscaneural-preview`             | Imagem de contêiner com a `pt-BR` localidade e a `pt-BR-FranciscaNeural` voz. |
| `zh-cn-xiaoxiaoneural-preview`              | Imagem de contêiner com a `zh-CN` localidade e a `zh-CN-XiaoxiaoNeural` voz.  |

---

## <a name="speech-language-detection"></a>Detecção de idioma de fala

A imagem de contêiner de [detecção de idioma de fala][sp-lid] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/speechservices/` repositório e é nomeado `language-detection` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/speechservices/language-detection` .

Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/speechservices/language-detection/tags/list).

| Marcas de imagem                                  | Observações                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                       |      |
| `1.1.0-amd64-preview`                       |      |

## <a name="key-phrase-extraction"></a>Extração de Frases-Chave

a imagem de contêiner pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/textanalytics/` repositório e é nomeado `keyphrase` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase` .

Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/keyphrase/tags/list).

# <a name="latest-version"></a>[Última versão](#tab/current)


| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` |       |

# <a name="previous-versions"></a>[Versões anteriores](#tab/previous)

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `1.1.012840001-amd64` |       |
| `1.1.012830001-amd64`    |       |

---

## <a name="text-language-detection"></a>Detecção de idioma do texto

A imagem de contêiner [detecção de idioma][ta-la] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/textanalytics/` repositório e é nomeado `language` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/textanalytics/language`


Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/language/tags/list).

# <a name="latest-versions"></a>[Versões mais recentes](#tab/current)

| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.013570001-amd64` | |
   

# <a name="previous-versions"></a>[Versões anteriores](#tab/previous)


| Marcas de imagem                    | Observações |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.012840001-amd64` |   |
| `1.1.012830001-amd64` |   |

---

## <a name="sentiment-analysis"></a>Análise de sentimento

A imagem de contêiner [análise de sentimento][ta-se] pode ser encontrada na `mcr.microsoft.com` agregação do registro de contêiner. Ele reside no `azure-cognitive-services/textanalytics/` repositório e é nomeado `sentiment` . O nome da imagem de contêiner totalmente qualificado é, `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment`

Essa imagem de contêiner tem as seguintes marcas disponíveis. Você também pode encontrar uma lista completa de [marcas no MCR](https://mcr.microsoft.com/v2/azure-cognitive-services/textanalytics/sentiment/tags/list).

| Marcas de imagem | Observações                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Análise de Sentimento v3 (inglês)               |
| `3.0-es`   | Análise de Sentimento v3 (espanhol)               |
| `3.0-fr`   | Análise de Sentimento v3 (francês)                |
| `3.0-it`   | Análise de Sentimento v3 (italiano)               |
| `3.0-de`   | Análise de Sentimento v3 (alemão)                |
| `3.0-zh`   | Análise de Sentimento v3 (chinês simplificado)  |
| `3.0-zht`  | Análise de Sentimento v3 (chinês tradicional) |
| `3.0-ja`   | Análise de Sentimento v3 (japonês)              |
| `3.0-pt`   | Análise de Sentimento v3 (Português)            |
| `3.0-nl`   | Análise de Sentimento v3 (Holandês)                 |
| `2.1`    | Análise de Sentimento v2      |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[sp-ntts]: ../speech-service/speech-container-howto.md?tabs=ntts
[sp-lid]: ../speech-service/speech-container-howto.md?tabs=lid
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
