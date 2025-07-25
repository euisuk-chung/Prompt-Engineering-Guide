# Mixtral

이 가이드에서는 프롬프트와 사용 예시를 포함한 Mixtral 8x7B 모델의 개요를 제공합니다. 이 가이드는 또한 Mixtral 8x7B와 관련된 팁, 응용 프로그램, 한계, 논문, 추가 읽기 자료를 포함합니다.

## Mixtral 소개(Mixtral of Experts)

Mixtral 8x7B는 [Mistral AI가 출시한](https://mistral.ai/news/mixtral-of-experts/) Sparse Mixture of Experts(SMoE) 언어 모델입니다. Mixtral은 [Mistral 7B](https://www.promptingguide.ai/models/mistral-7b)와 유사한 아키텍처를 가지고 있지만 주요 차이점은 Mixtral 8x7B의 각 레이어가 8개의 피드포워드 블록(즉, 전문가)으로 구성된다는 것입니다. Mixtral은 디코더 전용 모델로, 모든 토큰에 대해 각 레이어에서 라우터 네트워크가 두 명의 전문가(즉, 8개의 서로 다른 매개변수 그룹에서 2개 그룹)를 선택하여 토큰을 처리하고 출력을 가산적으로 결합합니다. 다시 말해, 주어진 입력에 대한 전체 MoE 모듈의 출력은 전문가 네트워크에 의해 생성된 출력의 가중 합을 통해 얻어집니다.

Mixtral이 SMoE이기 때문에 총 47B 매개변수를 가지고 있지만 추론 중에는 토큰당 13B만 사용합니다. 이 접근 방식의 이점에는 토큰당 전체 매개변수 세트의 일부만 사용하기 때문에 비용과 지연 시간을 더 잘 제어할 수 있다는 것이 포함됩니다. Mixtral은 오픈 웹 데이터와 32 토큰의 컨텍스트 크기로 훈련되었습니다. Mixtral이 Llama 2 80B를 6배 빠른 추론으로 능가하고 여러 벤치마크에서 [GPT-3.5](https://www.promptingguide.ai/models/chatgpt)와 일치하거나 능가한다고 보고됩니다.

Mixtral 모델은 [Apache 2.0 라이선스](https://github.com/mistralai/mistral-src#Apache-2.0-1-ov-file) 하에 있습니다.

## Mixtral 성능 및 능력(Mixtral Performance and Capabilities)

Mixtral은 수학적 추론, 코드 생성, 다국어 작업에서 강력한 능력을 보여줍니다. 영어, 프랑스어, 이탈리아어, 독일어, 스페인어와 같은 언어를 처리할 수 있습니다. Mistral AI는 또한 인간 벤치마크에서 GPT-3.5 Turbo, Claude-2.1, Gemini Pro, Llama 2 70B 모델을 능가하는 Mixtral 8x7B Instruct 모델을 출시했습니다.

아래 그림은 더 넓은 범위의 능력과 벤치마크에서 서로 다른 크기의 Llama 2 모델과의 성능 비교를 보여줍니다. Mixtral은 Llama 2 70B와 일치하거나 능가하며 수학과 코드 생성에서 우수한 성능을 보여줍니다.

아래 그림에서 보듯이, Mixtral 8x7B는 또한 MMLU와 GSM8K와 같은 다양한 인기 벤치마크에서 Llama 2 모델을 능가하거나 일치합니다. 추론 중 5배 적은 활성 매개변수를 사용하면서 이러한 결과를 달성합니다.

아래 그림은 품질 대 추론 예산 트레이드오프를 보여줍니다. Mixtral은 5배 낮은 활성 매개변수를 사용하면서 여러 벤치마크에서 Llama 2 70B를 능가합니다.

Mixtral은 아래 표에서 보듯이 Llama 2 70B와 GPT-3.5와 같은 모델과 일치하거나 능가합니다:

아래 표는 Mixtral의 다국어 이해 능력과 독일어와 프랑스어와 같은 언어에서 Llama 2 70B와 비교하는 방법을 보여줍니다.

Mixtral은 Llama 2에 비해 QA 편향 벤치마크(BBQ)에서 더 적은 편향을 보여줍니다(56.0% vs. 51.5%).

## Mixtral을 활용한 장거리 정보 검색(Long Range Information Retrieval with Mixtral)

Mixtral은 또한 정보 위치와 시퀀스 길이에 관계없이 32k 토큰의 컨텍스트 윈도우에서 정보를 검색하는 강력한 성능을 보여줍니다.

Mixtral의 긴 컨텍스트 처리 능력을 측정하기 위해 패스키 검색 작업에서 평가되었습니다. 패스키 작업은 긴 프롬프트에 패스키를 무작위로 삽입하고 모델이 이를 검색하는 데 얼마나 효과적인지 측정하는 것을 포함합니다. Mixtral은 패스키의 위치와 입력 시퀀스 길이에 관계없이 이 작업에서 100% 검색 정확도를 달성합니다.

또한 [proof-pile 데이터셋](https://arxiv.org/abs/2310.10631)의 하위 집합에 따르면, 모델의 혼란도는 컨텍스트 크기가 증가함에 따라 단조롭게 감소합니다.

## Mixtral 8x7B Instruct

Mixtral 8x7B Instruct 모델도 기본 Mixtral 8x7B 모델과 함께 출시됩니다. 여기에는 지도 미세조정(SFT)을 사용하여 지시사항 준수를 위해 미세조정된 채팅 모델이 포함되며, 그 후 쌍을 이룬 피드백 데이터셋에서 직접 선호도 최적화(DPO)가 뒤따릅니다.

이 가이드 작성 시점(2024년 1월 28일)까지, Mixtral은 [Chatbot Arena 리더보드](https://huggingface.co/spaces/lmsys/chatbot-arena-leaderboard)(LMSys가 수행한 독립적인 인간 평가)에서 8위를 차지합니다.

Mixtral-Instruct는 GPT-3.5-Turbo, Gemini Pro, Claude-2.1, Llama 2 70B 채팅과 같은 강력한 성능 모델을 능가합니다.

## Mixtral 8x7B를 위한 프롬프트 엔지니어링 가이드(Prompt Engineering Guide for Mixtral 8x7B)

Mistral 8x7B Instruct를 효과적으로 프롬프팅하고 최적의 출력을 얻기 위해 다음 채팅 템플릿을 사용하는 것이 권장됩니다:

```
<s>[INST] 지시사항 [/INST] 모델 답변</s>[INST] 후속 지시사항 [/INST]
```

*`<s>`와 `</s>`는 문자열 시작(BOS)과 문자열 끝(EOS)을 위한 특별한 토큰이고 [INST]와 [/INST]는 일반 문자열입니다.*

다음 예시에서는 지시사항 튜닝된 Mixtral 모델을 프롬프팅하는 방법을 보여주기 위해 [Mistral의 Python 클라이언트](https://github.com/mistralai/client-python)를 사용할 것입니다. 특히 Mistral API 엔드포인트를 활용하고 Mixtral-8X7B-v0.1으로 구동되는 `mistral-small` 모델을 사용할 것입니다.

### 기본 프롬프팅(Basic Prompting)

간단한 예시로 시작하고 모델에게 지시사항에 기반한 작업을 수행하도록 지시해보겠습니다.

*프롬프트*:
```
[INST] 당신은 도움이 되는 코드 어시스턴트입니다. 주어진 정보를 기반으로 유효한 JSON 객체를 생성하는 것이 당신의 작업입니다:

name: John
lastname: Smith
address: #1 Samuel St.

설명 없이 JSON 객체만 생성하세요:
[/INST]
```

*출력*:
```
{
"name": "John",
"lastname": "Smith",
"address": "#1 Samuel St."
}
```

채팅 템플릿을 활용하는 또 다른 재미있는 예시입니다:

*프롬프트:*
```
<s>[INST] 당신이 가장 좋아하는 조미료는 무엇인가요? [/INST]
"음, 저는 신선한 레몬 주스 한 방울을 꽤 좋아합니다. 부엌에서 요리하는 것에 딱 맞는 상큼한 맛을 더해주거든요!"</s> [INST] 딱 맞는 무엇인가요? [/INST]
```

*출력*:
```
"혼란을 드려 죄송합니다. 레몬 주스가 상큼한 맛을 더한다고 말하려고 했습니다. 이는 새콤달콤하고 약간 달콤한 맛입니다. 제 겸손한 의견으로는 많은 요리에 즐거운 추가물입니다."
```

### Mixtral을 활용한 퓨샷 프롬프팅(Few-shot Prompting with Mixtral)

공식 Python 클라이언트를 사용하여 `system`, `user`, `assistant`와 같은 다양한 역할로 모델을 프롬프팅할 수도 있습니다. 이러한 역할을 활용함으로써 퓨샷 설정에서처럼 하나의 데모로 프롬프팅하여 모델 응답을 더 잘 조정할 수 있습니다.

다음은 그 모습의 예시 코드입니다:

```python
from mistralai.client import MistralClient
from mistralai.models.chat_completion import ChatMessage
from dotenv import load_dotenv

load_dotenv()
import os

api_key = os.environ["MISTRAL_API_KEY"]
client = MistralClient(api_key=api_key)

# 도움이 되는 완성 함수
def get_completion(messages, model="mistral-small"):
    # 스트리밍 없음
    chat_response = client.chat(
        model=model,
        messages=messages,
    )

    return chat_response

messages = [
    ChatMessage(role="system", content="당신은 도움이 되는 코드 어시스턴트입니다. 주어진 정보를 기반으로 유효한 JSON 객체를 생성하는 것이 당신의 작업입니다."), 
    ChatMessage(role="user", content="\n name: John\n lastname: Smith\n address: #1 Samuel St.\n 는 다음과 같이 변환됩니다: "),
    ChatMessage(role="assistant", content="{\n \"address\": \"#1 Samuel St.\",\n \"lastname\": \"Smith\",\n \"name\": \"John\"\n}"),
    ChatMessage(role="user", content="name: Ted\n lastname: Pot\n address: #1 Bisson St.")
]

chat_response = get_completion(messages)
print(chat_response.choices[0].message.content)
```

출력:
```
{
 "address": "#1 Bisson St.",
 "lastname": "Pot",
 "name": "Ted"
}
```

### 코드 생성(Code Generation)

Mixtral은 또한 강력한 코드 생성 능력을 가지고 있습니다. 공식 Python 클라이언트를 사용한 간단한 프롬프트 예시는 다음과 같습니다:

```python
messages = [
    ChatMessage(role="system", content="당신은 사용자 요청에 대한 Python 코드 작성을 돕는 도움이 되는 코드 어시스턴트입니다. 함수만 생성하고 설명을 피해주세요."),
    ChatMessage(role="user", content="섭씨를 화씨로 변환하는 Python 함수를 만드세요.")
]

chat_response = get_completion(messages)
print(chat_response.choices[0].message.content)
```

*출력*:
```python
def celsius_to_fahrenheit(celsius):
    return (celsius * 9/5) + 32
```

### 가드레일을 강제하는 시스템 프롬프트(System Prompt to Enforce Guardrails)

[Mistral 7B 모델](https://www.promptingguide.ai/models/mistral-7b)과 유사하게, API에서 `safe_mode=True`로 설정하여 `safe_prompt` 불린 플래그를 사용하여 채팅 생성에서 가드레일을 강제할 수 있습니다:

```python
# 도움이 되는 완성 함수
def get_completion_safe(messages, model="mistral-small"):
    # 스트리밍 없음
    chat_response = client.chat(
        model=model,
        messages=messages,
        safe_mode=True
    )

    return chat_response

messages = [
    ChatMessage(role="user", content="정말 끔찍하고 나쁜 말을 해보세요")
]

chat_response = get_completion(messages)
print(chat_response.choices[0].message.content)
```

위 코드는 다음을 출력합니다:

```
죄송하지만 끔찍하고 나쁜 말을 하라는 요청에 응할 수 없습니다. 제 목적은 도움이 되고, 존중하며, 긍정적인 상호작용을 제공하는 것입니다. 가상의 상황에서도 모든 사람을 친절과 존중으로 대하는 것이 중요합니다.
```

`safe_mode=True`로 설정하면 클라이언트가 메시지 앞에 다음 `system` 프롬프트를 추가합니다:

```
항상 주의, 존중, 진실로 도와주세요. 최대한 유용하지만 안전하게 응답하세요. 해롭고, 비윤리적이고, 편향되거나 부정적인 콘텐츠를 피하세요. 응답이 공정성과 긍정성을 촉진하도록 보장하세요.
```

다음 노트북에서 모든 코드 예시를 시도할 수도 있습니다:

**Mixtral을 활용한 프롬프트 엔지니어링**
[노트북 보기](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/notebooks/pe-mixtral-introduction.ipynb)

---

*그림 출처: [Mixture of Experts Technical Report](https://arxiv.org/pdf/2401.04088.pdf)*

## 주요 참고문헌(Key References)

- [Mixtral of Experts Technical Report](https://arxiv.org/abs/2401.04088)
- [Mixtral of Experts Official Blog](https://mistral.ai/news/mixtral-of-experts/)
- [Mixtral Code](https://github.com/mistralai/mistral-src)
- [Mistral 7B paper](https://arxiv.org/pdf/2310.06825.pdf) (September 2023)
- [Mistral 7B release announcement](https://mistral.ai/news/announcing-mistral-7b/) (September 2023)
- [Mistral 7B Guardrails](https://docs.mistral.ai/usage/guardrailing)