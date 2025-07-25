# Gemini 시작하기(Getting Started with Gemini)

이 가이드에서는 Gemini 모델들의 개요와 효과적으로 프롬프트하고 사용하는 방법을 제공합니다. 이 가이드에는 Gemini 모델과 관련된 기능, 팁, 애플리케이션, 제한사항, 논문 및 추가 읽기 자료가 포함되어 있습니다.

## Gemini 소개(Introduction to Gemini)

Gemini는 Google Deepmind의 가장 최신이고 가장 강력한 AI 모델입니다. 처음부터 다중모달(multimodal) 기능으로 구축되었으며 텍스트, 이미지, 비디오, 오디오, 코드에 걸쳐 인상적인 교차모달 추론(crossmodal reasoning)을 보여줍니다.

Gemini는 세 가지 크기로 제공됩니다:

- **Ultra** - 모델 시리즈에서 가장 강력하며 고도로 복잡한 작업에 적합
- **Pro** - 광범위한 작업에서 확장하기에 최고의 모델로 간주됨
- **Nano** - 온디바이스 메모리 제약 작업 및 사용 사례를 위한 효율적인 모델; 1.8B(Nano-1) 및 3.25B(Nano-2) 파라미터 모델을 포함하며 대형 Gemini 모델에서 증류되어 4비트로 양자화됨

수반되는 [기술 보고서](https://storage.googleapis.com/deepmind-media/gemini/gemini_1_report.pdf)에 따르면, Gemini는 언어, 코딩, 추론, 다중모달 추론과 같은 작업을 다루는 32개 벤치마크 중 30개에서 최첨단 기술을 발전시켰습니다.

이는 [MMLU](https://paperswithcode.com/dataset/mmlu)(인기 있는 시험 벤치마크)에서 인간 전문가 수준의 성능을 달성한 첫 번째 모델이며, 20개의 다중모달 벤치마크에서 최첨단 기술을 주장합니다. Gemini Ultra는 MMLU에서 90.0%, 대학 수준의 주제 지식과 추론이 필요한 [MMMU 벤치마크](https://mmmu-benchmark.github.io/)에서 62.4%를 달성합니다.

Gemini 모델들은 32k 컨텍스트 길이를 지원하도록 훈련되었으며 효율적인 어텐션 메커니즘(예: [multi-query attention](https://arxiv.org/abs/1911.02150))을 가진 Transformer 디코더 위에 구축되었습니다. 오디오 및 시각적 입력과 인터리브된 텍스트 입력을 지원하며 텍스트와 이미지 출력을 생성할 수 있습니다.

모델들은 웹 문서, 책, 코드 데이터를 포함한 다중모달 및 다국어 데이터로 훈련되었으며 이미지, 오디오, 비디오 데이터를 포함합니다. 모델들은 모든 모달리티에 걸쳐 공동으로 훈련되며 강력한 교차모달 추론 기능과 각 도메인에서도 강력한 기능을 보여줍니다.

## Gemini 실험 결과(Gemini Experimental Results)

Gemini Ultra는 [chain-of-thought (CoT) 프롬프팅](https://www.promptingguide.ai/techniques/cot)과 [self-consistency](https://www.promptingguide.ai/techniques/consistency)와 같은 접근법과 결합할 때 가장 높은 정확도를 달성하며, 이는 모델 불확실성을 처리하는 데 도움이 됩니다.

기술 보고서에 보고된 바와 같이, Gemini Ultra는 MMLU에서의 성능을 탐욕 샘플링으로 84.0%에서 불확실성 라우팅 체인-오브-사우트 접근법(CoT 및 다수결 투표 포함)으로 32개 샘플을 사용하여 90.0%로 개선했으며, 32개의 체인-오브-사우트 샘플만 사용하면 85.0%로 약간 개선됩니다. 마찬가지로, CoT와 self-consistency는 GSM8K 초등학교 수학 벤치마크에서 94.4% 정확도를 달성합니다. 또한 Gemini Ultra는 [HumanEval](https://paperswithcode.com/dataset/humaneval) 코드 완성 문제의 74.4%를 올바르게 구현합니다. 아래는 Gemini의 결과와 모델들이 다른 주목할 만한 모델들과 어떻게 비교되는지를 요약한 표입니다.

Gemini Nano 모델들도 사실성(즉, 검색 관련 작업), 추론, STEM, 코딩, 다중모달 및 다국어 작업에서 강력한 성능을 보여줍니다.

표준 다국어 기능 외에도, Gemini는 [MGSM](https://paperswithcode.com/dataset/mgsm)과 [XLSum](https://paperswithcode.com/dataset/xl-sum)과 같은 다국어 수학 및 요약 벤치마크에서 각각 뛰어난 성능을 보여줍니다.

Gemini 모델들은 32K 시퀀스 길이로 훈련되었으며 컨텍스트 길이에 걸쳐 쿼리할 때 98% 정확도로 올바른 값을 검색하는 것으로 밝혀졌습니다. 이는 문서 검색 및 비디오 이해와 같은 새로운 사용 사례를 지원하는 중요한 기능입니다.

지시 튜닝된 Gemini 모델들은 지시 따르기, 창의적 글쓰기, 안전성과 같은 중요한 기능에서 인간 평가자들에 의해 일관되게 선호됩니다.

## Gemini 다중모달 추론 기능(Gemini Multimodal Reasoning Capabilities)

Gemini는 원래 다중모달로 훈련되었으며 언어 모델의 추론 기능과 함께 모달리티 간 기능을 결합하는 능력을 보여줍니다. 기능에는 테이블, 차트, 그림에서 정보 추출이 포함되지만 이에 국한되지 않습니다. 다른 흥미로운 기능에는 입력에서 세밀한 세부사항 식별, 공간과 시간에 걸친 컨텍스트 집계, 서로 다른 모달리티 간 정보 결합이 포함됩니다.

Gemini는 고수준 객체 인식, 세밀한 전사, 차트 이해, 다중모달 추론과 같은 이미지 이해 작업에서 기존 접근법들을 일관되게 능가합니다. 일부 이미지 이해 및 생성 기능은 다양한 글로벌 언어 세트에 걸쳐 전이됩니다(예: 힌디어와 루마니아어와 같은 언어를 사용하여 이미지 설명 생성).

### 텍스트 요약(Text Summarization)

Gemini는 다중모달 시스템으로 훈련되었지만 GPT-3.5, Claude, Llama와 같은 현대적인 대규모 언어 모델에 존재하는 많은 기능을 갖추고 있습니다. 아래는 Gemini Pro를 사용한 간단한 텍스트 요약 작업의 예시입니다. 이 예시에는 온도 값 0으로 [Google AI Studio](https://ai.google.dev)를 사용합니다.

프롬프트:
```
Your task is to summarize an abstract into one sentence. 

Avoid technical jargon and explain it in the simplest of words.

Abstract: Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body's immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.
```

Gemini Pro 출력:

```
Antibiotics are medicines used to kill or stop the growth of bacteria causing infections, but they don't work against viruses.
```

다음은 Google AI Studio 내에서 작업과 모델 응답(강조됨)이 어떻게 보이는지의 스크린샷입니다.

### 정보 추출(Information Extraction)

다음은 텍스트 조각을 분석하고 원하는 정보를 추출하는 작업의 또 다른 예시입니다. 이것은 zero-shot 프롬프팅을 사용하므로 결과가 완벽하지는 않지만 모델이 상대적으로 잘 수행하고 있음을 염두에 두세요.

프롬프트:
```
Your task is to extract model names from machine learning paper abstracts. Your response is an array of the model names in the format [\"model_name\"]. If you don't find model names in the abstract or you are not sure, return [\"NA\"]

Abstract: Large Language Models (LLMs), such as ChatGPT and GPT-4, have revolutionized natural language processing research and demonstrated potential in Artificial General Intelligence (AGI). However, the expensive training and deployment of LLMs present challenges to transparent and open academic research. To address these issues, this project open-sources the Chinese LLaMA and Alpaca…
```

Gemini Pro 출력:

```
[\"LLMs\", \"ChatGPT\", \"GPT-4\", \"Chinese LLaMA\", \"Alpaca\"]
```

### 시각적 질문 답변(Visual Question Answering)

시각적 질문 답변은 입력으로 전달된 이미지에 대해 모델에게 질문하는 것을 포함합니다. Gemini 모델들은 차트, 자연 이미지, 밈 및 기타 많은 유형의 이미지에 대한 이미지 이해를 위한 다양한 다중모달 추론 기능을 보여줍니다. 아래 예시에서는 모델(Gemini Pro Vision, Google AI Studio를 통해 접근)에 텍스트 지시와 이 프롬프트 엔지니어링 가이드의 스냅샷을 나타내는 이미지를 제공합니다.

모델은 "The title of the website is "Prompt Engineering Guide"."라고 응답하며, 이는 주어진 질문에 기반하여 올바른 답변으로 보입니다.

다른 입력 질문으로 또 다른 예시가 있습니다. Google AI Studio를 사용하면 위의 `{{}} Test input` 옵션을 클릭하여 다른 입력으로 테스트할 수 있습니다. 그런 다음 아래 표에 테스트하는 프롬프트를 추가할 수 있습니다.

자신만의 이미지를 업로드하고 질문하여 실험해보세요. Gemini Ultra가 이러한 유형의 작업에서 훨씬 더 잘할 수 있다고 보고됩니다. 이는 모델이 사용 가능해질 때 더 실험해볼 것입니다.

### 검증 및 수정(Verifying and Correcting)

Gemini 모델들은 인상적인 교차모달 추론 기능을 보여줍니다. 예를 들어, 아래 그림은 교사가 그린 물리 문제의 해결책(왼쪽)을 보여줍니다. Gemini는 그런 다음 문제에 대해 추론하고 학생이 해결책에서 잘못한 부분이 있다면 설명하도록 프롬프트됩니다. 모델은 또한 문제를 해결하고 수학 부분에 LaTeX를 사용하도록 지시받습니다. 응답(오른쪽)은 문제와 해결책을 세부사항과 함께 설명하는 모델이 제공한 해결책입니다.

### 그림 재배치(Rearranging Figures)

아래는 기술 보고서에서 나온 또 다른 흥미로운 예시로, Gemini의 다중모달 추론 기능을 보여주며 서브플롯을 재배치하기 위한 matplotlib 코드를 생성합니다. 다중모달 프롬프트는 왼쪽 상단에, 생성된 코드는 오른쪽에, 렌더링된 코드는 왼쪽 하단에 표시됩니다. 모델은 인식, 코드 생성, 서브플롯 위치에 대한 추상적 추론, 원하는 위치에 서브플롯을 재배치하기 위한 지시 따르기와 같은 여러 기능을 활용하여 작업을 해결합니다.

### 비디오 이해(Video Understanding)

Gemini Ultra는 다양한 few-shot 비디오 캡셔닝 작업과 zero-shot 비디오 질문 답변에서 최첨단 결과를 달성합니다. 아래 예시는 모델에 비디오와 텍스트 지시가 입력으로 제공됨을 보여줍니다. 비디오를 분석하고 상황에 대해 추론하여 적절한 답변 또는 이 경우에는 사람이 기술을 개선할 수 있는 방법에 대한 권장사항을 제공할 수 있습니다.

### 이미지 이해(Image Understanding)

Gemini Ultra는 또한 few-shot 프롬프트를 받아 이미지를 생성할 수 있습니다. 예를 들어, 아래 예시에서 보여주는 것처럼 사용자가 두 가지 색상과 이미지 제안에 대한 정보를 제공하는 인터리브된 이미지와 텍스트의 한 예시로 프롬프트할 수 있습니다. 모델은 그런 다음 프롬프트의 최종 지시를 받아 보는 색상과 함께 몇 가지 아이디어로 응답합니다.

### 모달리티 결합(Modality Combination)

Gemini 모델들은 또한 오디오와 이미지의 시퀀스를 원래대로 처리하는 능력을 보여줍니다. 예시에서 모델이 오디오와 이미지의 시퀀스로 프롬프트될 수 있음을 관찰할 수 있습니다. 모델은 그런 다음 각 상호작용의 컨텍스트를 고려한 텍스트 응답을 보낼 수 있습니다.

### Gemini 범용 코딩 에이전트(Gemini Generalist Coding Agent)

Gemini는 또한 [AlphaCode 2](https://storage.googleapis.com/deepmind-media/AlphaCode2/AlphaCode2_Tech_Report.pdf)라는 범용 에이전트를 구축하는 데 사용되며, 이는 추론 기능을 검색 및 도구 사용과 결합하여 경쟁 프로그래밍 문제를 해결합니다. AlphaCode 2는 Codeforces 경쟁 프로그래밍 플랫폼에서 참가자 상위 15% 내에 랭크됩니다.

## Gemini와 Few-Shot 프롬프팅(Few-Shot Prompting with Gemini)
Few-shot 프롬프팅은 모델에게 원하는 출력의 종류를 나타내는 데 유용한 프롬프팅 접근법입니다. 이는 출력을 특정 형식(예: JSON 객체)이나 스타일로 원할 때와 같은 다양한 시나리오에 유용합니다. Google AI Studio는 인터페이스에서도 이를 활성화합니다. 아래는 Gemini 모델과 함께 few-shot 프롬프팅을 사용하는 방법의 예시입니다.

Gemini를 사용하여 간단한 감정 분류기를 구축하는 데 관심이 있습니다. 첫 번째 단계는 "Create new" 또는 "+"를 클릭하여 "Structured prompt"를 만드는 것입니다. Few-shot 프롬프트는 지시사항(작업 설명)과 제공한 예시를 결합할 것입니다. 아래 그림은 모델에 전달하는 지시사항(상단)과 예시를 보여줍니다. INPUT 텍스트와 OUTPUT 텍스트를 더 설명적인 표시자로 설정할 수 있습니다. 아래 예시는 입력과 출력 표시자로 각각 "Text:"와 "Emotion:"을 사용합니다.

전체 결합된 프롬프트는 다음과 같습니다:

```
Your task is to classify a piece of text, delimited by triple backticks, into the following emotion labels: ["anger", "fear", "joy", "love", "sadness", "surprise"]. Just output the label as a lowercase string.
Text: I feel very angry today
Emotion: anger
Text: Feeling thrilled by the good news today.
Emotion: joy
Text: I am actually feeling good today.
Emotion:
```

그런 다음 "Test your prompt" 섹션 아래에 입력을 추가하여 프롬프트를 테스트할 수 있습니다. "I am actually feeling good today." 예시를 입력으로 사용하고 "Run"을 클릭한 후 모델이 올바르게 "joy" 라벨을 출력합니다. 아래 그림의 예시를 참조하세요:

## 라이브러리 사용법(Library Usage)

아래는 Gemini API를 사용하여 Gemini Pro 모델을 프롬프트하는 방법을 보여주는 간단한 예시입니다. `google-generativeai` 라이브러리를 설치하고 Google AI Studio에서 API 키를 얻어야 합니다. 아래 예시는 위 섹션에서 사용한 것과 같은 정보 추출 작업을 실행하는 코드입니다.

```python
"""
At the command line, only need to run once to install the package via pip:

$ pip install google-generativeai
"""

import google.generativeai as genai

genai.configure(api_key="YOUR_API_KEY")

# Set up the model
generation_config = {
  "temperature": 0,
  "top_p": 1,
  "top_k": 1,
  "max_output_tokens": 2048,
}

safety_settings = [
  {
    "category": "HARM_CATEGORY_HARASSMENT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_HATE_SPEECH",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_SEXUALLY_EXPLICIT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_DANGEROUS_CONTENT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  }
]

model = genai.GenerativeModel(model_name="gemini-pro",
                              generation_config=generation_config,
                              safety_settings=safety_settings)

prompt_parts = [
  "Your task is to extract model names from machine learning paper abstracts. Your response is an array of the model names in the format [\\\"model_name\\\"]. If you don't find model names in the abstract or you are not sure, return [\\\"NA\\\"]\n\nAbstract: Large Language Models (LLMs), such as ChatGPT and GPT-4, have revolutionized natural language processing research and demonstrated potential in Artificial General Intelligence (AGI). However, the expensive training and deployment of LLMs present challenges to transparent and open academic research. To address these issues, this project open-sources the Chinese LLaMA and Alpaca…",
]

response = model.generate_content(prompt_parts)
print(response.text)
```

출력은 이전과 동일합니다:
```
[\"LLMs\", \"ChatGPT\", \"GPT-4\", \"Chinese LLaMA\", \"Alpaca\"]
```

## 참고문헌(References)

- [Introducing Gemini: our largest and most capable AI model](https://blog.google/technology/ai/google-gemini-ai/#sundar-note)
- [How it's Made: Interacting with Gemini through multimodal prompting](https://developers.googleblog.com/2023/12/how-its-made-gemini-multimodal-prompting.html)
- [Welcome to the Gemini era](https://deepmind.google/technologies/gemini/#introduction)
- [Prompt design strategies](https://ai.google.dev/docs/prompt_best_practices)
- [Gemini: A Family of Highly Capable Multimodal Models - Technical Report](https://storage.googleapis.com/deepmind-media/gemini/gemini_1_report.pdf)
- [Fast Transformer Decoding: One Write-Head is All You Need](https://arxiv.org/abs/1911.02150)
- [Google AI Studio quickstart](https://ai.google.dev/tutorials/ai-studio_quickstart)
- [Multimodal Prompts](https://ai.google.dev/docs/multimodal_concepts)
- [Gemini vs GPT-4V: A Preliminary Comparison and Combination of Vision-Language Models Through Qualitative Cases](https://arxiv.org/abs/2312.15011v1)
- [A Challenger to GPT-4V? Early Explorations of Gemini in Visual Expertise](https://arxiv.org/abs/2312.12436v2)