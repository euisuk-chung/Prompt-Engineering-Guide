# LLM을 활용한 함수 호출(Function Calling with LLMs)

## 함수 호출 시작하기(Getting Started with Function Calling)

함수 호출은 LLM을 외부 도구에 안정적으로 연결하여 효과적인 도구 사용과 외부 API와의 상호작용을 가능하게 하는 능력입니다.

GPT-4와 GPT-3.5와 같은 LLM은 함수가 호출되어야 할 때를 감지하고 함수를 호출하기 위한 인수를 포함한 JSON을 출력하도록 미세조정되었습니다. 함수 호출에 의해 호출되는 함수들은 AI 애플리케이션에서 도구로 작동하며, 단일 요청에서 둘 이상을 정의할 수 있습니다.

함수 호출은 자연어를 API 호출로 변환하여 LLM을 위한 컨텍스트를 검색하거나 외부 도구와 상호작용해야 하는 LLM 기반 챗봇이나 에이전트를 구축하는 데 중요한 능력입니다.

함수 호출을 통해 개발자는 다음을 생성할 수 있습니다:

- 외부 도구를 효율적으로 사용하여 질문에 답변할 수 있는 대화형 에이전트. 예를 들어, "벨리즈의 날씨는 어떤가요?"라는 쿼리는 `get_current_weather(location: string, unit: 'celsius' | 'fahrenheit')`와 같은 함수 호출로 변환됩니다.
- 데이터 추출 및 태깅을 위한 LLM 기반 솔루션(예: 위키피디아 기사에서 사람 이름 추출)
- 자연어를 API 호출이나 유효한 데이터베이스 쿼리로 변환하는 데 도움이 되는 애플리케이션
- 지식 베이스와 상호작용하는 대화형 지식 검색 엔진

이 가이드에서는 GPT-4와 오픈소스 모델이 다양한 사용 사례에 대해 함수 호출을 수행하도록 프롬프팅하는 방법을 보여줍니다.

## GPT-4를 활용한 함수 호출(Function Calling with GPT-4)

기본 예시로, 모델에게 주어진 위치의 날씨를 확인하도록 요청했다고 가정해보겠습니다.

LLM만으로는 이 요청에 응답할 수 없습니다. 왜냐하면 절단점이 있는 데이터셋으로 훈련되었기 때문입니다. 이를 해결하는 방법은 LLM을 외부 도구와 결합하는 것입니다. 모델의 함수 호출 기능을 활용하여 호출할 외부 함수와 그 인수를 결정한 다음 최종 응답을 반환하도록 할 수 있습니다. 아래는 OpenAI API를 사용하여 이를 달성할 수 있는 방법의 간단한 예시입니다.

사용자가 모델에게 다음 질문을 하고 있다고 가정해보겠습니다:

```
런던의 날씨는 어떤가요?
```

함수 호출을 사용하여 이 요청을 처리하기 위해 첫 번째 단계는 OpenAI API 요청의 일부로 전달할 날씨 함수 또는 함수 집합을 정의하는 것입니다:

```python
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "주어진 위치의 현재 날씨를 가져옵니다",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "도시와 주, 예: San Francisco, CA",
                    },
                    "unit": {
                        "type": "string", 
                        "enum": ["celsius", "fahrenheit"]},
                },
                "required": ["location"],
            },
        },   
    }
]
```

`get_current_weather` 함수는 주어진 위치의 현재 날씨를 반환합니다. 이 함수 정의를 요청의 일부로 전달할 때, 실제로 함수를 실행하지는 않고 함수를 호출하는 데 필요한 인수를 포함한 JSON 객체만 반환합니다. 이를 달성하는 방법의 코드 스니펫은 다음과 같습니다.

다음과 같이 완성 함수를 정의할 수 있습니다:

```python
def get_completion(messages, model="gpt-3.5-turbo-1106", temperature=0, max_tokens=300, tools=None):
    response = openai.chat.completions.create(
        model=model,
        messages=messages,
        temperature=temperature,
        max_tokens=max_tokens,
        tools=tools
    )
    return response.choices[0].message
```

사용자 질문을 다음과 같이 구성할 수 있습니다:

```python
messages = [
    {
        "role": "user",
        "content": "런던의 날씨는 어떤가요?"
    }
]
```

마지막으로, 위의 `get_completion`을 호출하고 `messages`와 `tools`를 모두 전달할 수 있습니다:

```python
response = get_completion(messages, tools=tools)
```

`response` 객체는 다음을 포함합니다:

```python
ChatCompletionMessage(content=None, role='assistant', function_call=None, tool_calls=[ChatCompletionMessageToolCall(id='...', function=Function(arguments='{"location":"London","unit":"celsius"}', name='get_current_weather'), type='function')])
```

특히, `arguments` 객체는 모델에 의해 추출되고 요청을 완료하는 데 필요한 중요한 인수를 포함합니다.

그런 다음 실제 날씨를 위해 외부 날씨 API를 호출하도록 선택할 수 있습니다. 날씨 정보를 사용할 수 있게 되면 원래 사용자 질문에 대한 최종 응답을 요약하기 위해 모델에게 다시 전달할 수 있습니다.

## 노트북(Notebooks)

🎓 **정보**: 새로운 AI 과정에서 함수 호출에 대해 자세히 알아보세요. [지금 참여하세요!](https://dair-ai.thinkific.com/)
PROMPTING20 코드를 사용하면 추가 20% 할인을 받을 수 있습니다.

다음은 OpenAI API를 사용한 함수 호출 방법을 보여주는 간단한 예시가 포함된 노트북입니다:

**OpenAI API를 활용한 함수 호출**
[노트북 보기](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/notebooks/pe-function-calling.ipynb)

## 오픈소스 LLM을 활용한 함수 호출(Function Calling with Open-Source LLMs)
오픈소스 LLM을 활용한 함수 호출에 대한 추가 노트가 곧 나올 예정입니다.

## 함수 호출 사용 사례(Function Calling Use Cases)

다음은 LLM의 함수 호출 기능으로부터 혜택을 받을 수 있는 사용 사례 목록입니다:

- **대화형 에이전트**: 함수 호출은 외부 API나 외부 지식 베이스를 호출하여 더 관련성 있고 유용한 응답을 제공하는 복잡한 질문에 답변하는 복잡한 대화형 에이전트나 챗봇을 만드는 데 사용할 수 있습니다.

- **자연어 이해**: 자연어를 구조화된 JSON 데이터로 변환하고, 텍스트에서 구조화된 데이터를 추출하며, 명명된 엔티티 인식, 감정 분석, 키워드 추출과 같은 작업을 수행할 수 있습니다.

- **수학 문제 해결**: 함수 호출은 여러 단계와 다양한 유형의 고급 계산이 필요한 복잡한 수학 문제를 해결하기 위한 사용자 정의 함수를 정의하는 데 사용할 수 있습니다.

- **API 통합**: LLM을 외부 API와 효과적으로 통합하여 입력을 기반으로 데이터를 가져오거나 작업을 수행하는 데 사용할 수 있습니다. 이는 QA 시스템이나 창의적 어시스턴트를 구축하는 데 도움이 될 수 있습니다. 일반적으로 함수 호출은 자연어를 유효한 API 호출로 변환할 수 있습니다.

- **정보 추출**: 함수 호출은 기사에서 관련 뉴스 스토리나 참조를 검색하는 것과 같이 주어진 입력에서 특정 정보를 추출하는 데 효과적으로 사용될 수 있습니다.

## 참고문헌(References)
- [Fireworks Raises the Quality Bar with Function Calling Model and API Release](https://blog.fireworks.ai/fireworks-raises-the-quality-bar-with-function-calling-model-and-api-release-e7f49d1e98e9)
- [Benchmarking Agent Tool Use and Function Calling](https://blog.langchain.dev/benchmarking-agent-tool-use/)
- [Function Calling](https://ai.google.dev/docs/function_calling)
- [Interacting with APIs](https://python.langchain.com/docs/use_cases/apis)
- [OpenAI's Function Calling](https://platform.openai.com/docs/guides/function-calling)
- [How to call functions with chat models](https://cookbook.openai.com/examples/how_to_call_functions_with_chat_models)
- [Pushing ChatGPT's Structured Data Support To Its Limits](https://minimaxir.com/2023/12/chatgpt-structured-data/)
- [Math Problem Solving with Function Calling](https://github.com/svpino/openai-function-calling/blob/main/sample.ipynb)
