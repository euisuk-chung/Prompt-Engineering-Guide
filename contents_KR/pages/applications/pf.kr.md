# 프롬프트 함수(Prompt Function)

## 소개(Introduction)
GPT의 대화 인터페이스를 프로그래밍 언어의 셸(shell)에 비유하면, 캡슐화된 프롬프트는 하나의 함수(function)를 형성한다고 볼 수 있습니다. 이 함수는 고유한 이름을 가지며, 해당 이름으로 입력 텍스트를 호출하면 내부 규칙에 따라 결과를 반환합니다. 즉, GPT와 쉽게 상호작용할 수 있도록 이름이 지정된 재사용 가능한 프롬프트를 만드는 것입니다. 이는 마치 GPT가 특정 작업을 대신 수행할 수 있도록 도와주는 유용한 도구(tool)와 같으며, 우리는 입력만 제공하면 원하는 출력을 받을 수 있습니다.

프롬프트를 함수로 캡슐화하면 여러 개의 함수를 조합하여 워크플로우(workflow)를 구성할 수 있습니다. 각 함수는 특정 단계나 작업을 나타내며, 이를 특정 순서로 결합하면 복잡한 프로세스를 자동화하거나 문제를 더욱 효율적으로 해결할 수 있습니다. 이러한 접근 방식은 GPT와의 상호작용을 보다 구조적이고 체계적으로 만들어, 다양한 작업을 강력하게 수행할 수 있도록 합니다.

함수를 사용하기 전에, 먼저 GPT에게 해당 함수에 대해 알려야 합니다. 다음은 함수를 정의하는 프롬프트 예시입니다.

*프롬프트 예시:*
> 이 프롬프트를 **메타 프롬프트(meta prompt)**라고 부르겠습니다.  
이 프롬프트는 GPT-3.5에서 테스트되었으며, GPT-4에서 더 뛰어난 성능을 보입니다.

```
Hello, ChatGPT! I hope you are doing well. I am reaching out to you for assistance with a specific function. I understand that you have the capability to process information and perform various tasks based on the instructions provided. In order to help you understand my request more easily, I will be using a template to describe the function, input, and instructions on what to do with the input. Please find the details below:

function_name: [Function Name]
input: [Input]
rule: [Instructions on how to process the input]

I kindly request you to provide the output for this function, based on the details I have provided. Your assistance is greatly appreciated. Thank you!
I will replace the text inside the brackets with the relevant information for the function I want you to perform. This detailed introduction should help you understand my request more efficiently and provide the desired output. The format is function_name(input) If you understand, just answer one word with ok.
```

## 예시(Examples)

### 영어 학습 도우미(English study assistant)
예를 들어, GPT를 영어 학습에 활용하고자 한다면 일련의 함수를 만들어 과정을 단순화할 수 있습니다.

이 예시는 GPT-3.5에서 테스트되었으며, GPT-4에서 더 뛰어난 성능을 보입니다.

#### 함수 설명(Function description)

위에서 정의한 **메타 프롬프트(meta prompt)**를 GPT에 입력해야 합니다.

그런 다음 `trans_word`라는 함수를 만듭니다.  
이 함수는 GPT에게 중국어를 영어로 번역하도록 요청합니다.

*프롬프트 예시:*
```
function_name: [trans_word]
input: ["text"]
rule: [I want you to act as an English translator, spelling corrector and improver. I will provide you with input forms including "text" in any language and you will detect the language, translate it and answer in the corrected of my text, in English.]
```

텍스트를 확장하는 함수를 작성합니다.

*프롬프트 예시:*
```
function_name: [expand_word]
input: ["text"]
rule: [Please serve as a Chatterbox, spelling corrector, and language enhancer. I will provide you with input forms including "text" in any language, and output the original language.I want you to Keep the meaning same, but make them more literary.]
```

텍스트를 교정하는 함수를 작성합니다.

*프롬프트 예시:*
```
function_name: [fix_english]
input: ["text"]
rule: [Please serve as an English master, spelling corrector, and language enhancer. I will provide you with input forms including "text", I want you to improve the text's vocabulary and sentences with more natural and elegent. Keep the meaning same.]
```
마지막으로, 각 함수를 독립적으로 실행하거나 연쇄적으로 연결할 수 있습니다.

*프롬프트 예시:*
```
trans_word('婆罗摩火山处于享有“千岛之国”美称的印度尼西亚. 多岛之国印尼有4500座之多的火山, 世界著名的十大活火山有三座在这里.')
fix_english('Finally, you can run the function independently or chain them together.')
fix_english(expand_word(trans_word('婆罗摩火山处于享有“千岛之国”美称的印度尼西亚. 多岛之国印尼有4500座之多的火山, 世界著名的十大活火山有三座在这里.')))
```
이와 같이 함수의 이름, 입력, 처리 규칙을 명확히 표현함으로써 각 단계의 기능과 목적을 체계적으로 이해할 수 있습니다.

_팁:_
ChatGPT가 불필요한 정보를 출력하지 않게 하려면, 함수 규칙 정의 후 다음 문장을 추가할 수 있습니다.
```
DO NOT SAY THINGS ELSE OK, UNLESS YOU DONT UNDERSTAND THE FUNCTION
```

### 다중 파라미터 함수(Multiple params function)
다섯 개의 입력 파라미터를 받아 비밀번호를 생성하는 함수를 만들어 보겠습니다. 이 함수는 생성된 비밀번호만 출력합니다.

*프롬프트 예시:*
```
function_name: [pg]
input: ["length", "capitalized", "lowercase", "numbers", "special"]
rule: [I want you to act as a password generator for individuals in need of a secure password. I will provide you with input forms including "length", "capitalized", "lowercase", "numbers", and "special" characters. Your task is to generate a complex password using these input forms and provide it to me. Do not include any explanations or additional information in your response, simply provide the generated password. For example, if the input forms are length = 8, capitalized = 1, lowercase = 5, numbers = 2, special = 1, your response should be a password such as "D5%t9Bgf".]
```
```
pg(length = 10, capitalized = 1, lowercase = 5, numbers = 2, special = 1)
pg(10,1,5,2,1)
```

### 생각(Thought)
이미 GPT를 프로그래밍하는 다양한 프로젝트가 존재합니다. 예를 들어:
- [GitHub Copilot](https://github.com/features/copilot)
- [Microsoft AI](https://www.microsoft.com/en-us/ai)
- [chatgpt-plugins](https://openai.com/blog/chatgpt-plugins)
- [LangChain](https://github.com/hwchase17/langchain)
- [marvin](https://github.com/PrefectHQ/marvin)

하지만 이러한 프로젝트들은 대부분 제품 고객이나 Python 등 프로그래밍 언어를 사용할 수 있는 사용자를 대상으로 설계되었습니다. 일반 사용자는 이 간단한 템플릿을 일상 업무에 활용하고, 여러 번 반복하여 개선할 수 있습니다. 노트 애플리케이션에 함수를 기록해두면, 나중에 라이브러리로 업데이트하는 것도 가능합니다.
또한, [ChatGPT-Next-Web](https://github.com/Yidadaa/ChatGPT-Next-Web), [chatbox](https://github.com/Bin-Huang/chatbox), [PromptAppGPT](https://github.com/mleoking/PromptAppGPT), [ChatGPT-Desktop](https://github.com/lencx/ChatGPT) 등 오픈소스 ChatGPT 도구도 활용할 수 있습니다. 현재 ChatGPT-Next-Web은 새 채팅을 시작하기 전에 몇 가지 샷을 추가할 수 있으며, PromptAppGPT는 프롬프트 템플릿 기반의 로우코드 웹 애플리케이션 개발을 지원하여 누구나 몇 줄의 프롬프트만으로 AutoGPT와 유사한 애플리케이션을 개발할 수 있습니다.
이러한 기능을 활용해 자신만의 함수를 추가하고 사용할 수 있습니다.
