---
author: Johnnytechn
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: include
ms.date: 10/22/2020
ms.author: v-johya
ms.openlocfilehash: 18a3ac15c49abf5f1f4491aa11c6ea41eb6c5eb3
ms.sourcegitcommit: 537d52cb783892b14eb9b33cf29874ffedebbfe3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92499151"
---
[!INCLUDE [Prerequisites](prerequisites-python.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-project-and-import-required-modules"></a>创建一个项目并导入必需的模块

使用最喜欢的 IDE 或编辑器创建新的 Python 项目。 然后，将此代码片段复制到项目的名为 `translate-text.py` 的文件中。 请确保 IDE 解释器引用了正确的 Python 版本，以避免无法识别库。

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> 如果尚未使用这些模块，则需在运行程序之前安装它们。 若要安装这些包，请运行 `pip install requests uuid`。

第一个注释告知 Python 解释器使用 UTF-8 编码。 然后导入必需的模块，以便从环境变量读取订阅密钥、构造 HTTP 请求、创建唯一标识符，以及处理“翻译”返回的 JSON 响应。

## <a name="set-the-subscription-key-endpoint-and-path"></a>设置订阅密钥、终结点和路径

此示例将尝试从以下环境变量中读取“翻译”订阅密钥和终结点：`TRANSLATOR_TEXT_KEY` 和 `TRANSLATOR_TEXT_ENDPOINT`。 如果不熟悉环境变量，则可将 `subscription_key` 和 `endpoint` 设置为字符串并注释掉条件语句。

将以下代码复制到项目中：

```python
key_var_name = 'TRANSLATOR_TEXT_SUBSCRIPTION_KEY'
if not key_var_name in os.environ:
    raise Exception('Please set/export the environment variable: {}'.format(key_var_name))
subscription_key = os.environ[key_var_name]

endpoint_var_name = 'TRANSLATOR_TEXT_ENDPOINT'
if not endpoint_var_name in os.environ:
    raise Exception('Please set/export the environment variable: {}'.format(endpoint_var_name))
endpoint = os.environ[endpoint_var_name]
```

“翻译”全局终结点设置为 `endpoint`。 `path` 设置 `translate` 路由并确定我们需使用 API 的版本 3。

`params` 用于设置输出语言。 在此示例中，我们将从英文翻译为意大利文和德文：`it` 和 `de`。

>[!NOTE]
> 有关终结点、路由和请求参数的详细信息，请参阅[翻译 3.0：翻译](/cognitive-services/translator/reference/v3-0-translate)。

```python
path = '/translate?api-version=3.0'
params = '&to=de&to=it'
constructed_url = endpoint + path + params
```

## <a name="add-headers"></a>添加标头

若要对请求进行身份验证，最容易的方法是将订阅密钥作为 `Ocp-Apim-Subscription-Key` 标头传入，这是我们在此示例中使用的方法。 替代方法是交换订阅密钥来获取访问令牌，将访问令牌作为 `Authorization` 标头传入，以便对请求进行验证。 有关详细信息，请参阅[身份验证](/cognitive-services/translator/reference/v3-0-reference#authentication)。

将以下代码片段复制到项目中。

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

如果使用的是认知服务多服务订阅，则还必须在请求参数中包括 `Ocp-Apim-Subscription-Region`。 [详细了解如何使用多服务订阅进行身份验证](/cognitive-services/translator/reference/v3-0-reference#authentication)。

## <a name="create-a-request-to-translate-text"></a>创建文本翻译请求

定义要翻译的字符串：

```python
body = [{
    'text': 'Hello World!'
}]
```

接下来，请使用 `requests` 模块创建一个 POST 请求。 它使用三个参数：串联的 URL、请求标头和请求正文：

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>输出响应

最后一步是输出结果。 以下代码片段通过将密钥排序、设置缩进以及声明项和密钥分隔符来美化结果。

```python
print(json.dumps(response, sort_keys=True, indent=4,
                 ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>将其放在一起

就是这样，你已构建了一个简单的程序。该程序可以调用“翻译”并返回 JSON 响应。 现在，可以运行该程序了：

```console
python translate-text.py
```

如果希望将你的代码与我们的进行比较，请查看 [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python) 上提供的完整示例。

## <a name="sample-response"></a>示例响应

```json
[
    {
        "detectedLanguage": {
            "language": "en",
            "score": 1.0
        },
        "translations": [
            {
                "text": "Hallo Welt!",
                "to": "de"
            },
            {
                "text": "Salve, mondo!",
                "to": "it"
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>清理资源

如果已将订阅密钥硬编码到程序中，请确保在完成本快速入门后删除该订阅密钥。

## <a name="next-steps"></a>后续步骤

查看 API 参考，了解使用“翻译”可以执行的所有操作。

> [!div class="nextstepaction"]
> [API 参考](/cognitive-services/translator/reference/v3-0-reference)

