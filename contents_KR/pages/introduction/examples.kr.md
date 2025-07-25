# 프롬프트 예시(Examples of Prompts)

이전 섹션에서는 LLM(대형 언어 모델)에 프롬프트를 입력하는 기본 예시를 소개했습니다.

이 섹션에서는 다양한 작업을 달성하기 위해 프롬프트를 활용하는 더 많은 예시를 제공하고, 그 과정에서 주요 개념도 함께 소개합니다. 개념을 배우는 가장 좋은 방법 중 하나는 예시를 직접 따라가 보는 것입니다. 아래의 몇 가지 예시는 잘 설계된 프롬프트를 사용해 다양한 유형의 작업을 수행할 수 있음을 보여줍니다.

주요 주제:
- [텍스트 요약](#text-summarization)
- [정보 추출](#information-extraction)
- [질문 답변](#question-answering)
- [텍스트 분류](#text-classification)
- [대화](#conversation)
- [코드 생성](#code-generation)
- [추론](#reasoning)

---

<iframe width="100%"
  height="415px"
  src="https://www.youtube.com/embed/TBhRC4Dath4?si=6nwh0GuYAOv1H6yT" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  />

<Callout type="info" emoji="🎓">
프롬프팅 기법과 예시에 대해 더 배우고 싶다면 새로운 AI 과정을 확인하세요. [지금 참여하기!](https://dair-ai.thinkific.com/)
추가 20% 할인 코드: PROMPTING20
</Callout>

## 텍스트 요약(Text Summarization)
자연어 생성(NLG)에서 표준적인 작업 중 하나는 텍스트 요약입니다. 텍스트 요약은 다양한 방식과 도메인에서 활용될 수 있습니다. 실제로, 언어 모델의 가장 유망한 응용 중 하나는 기사나 개념을 빠르고 읽기 쉬운 요약으로 만들어주는 능력입니다. 프롬프트를 활용한 기본 요약 작업을 시도해봅시다.

예를 들어, 항생제(antibiotics)에 대해 배우고 싶다면 다음과 같이 프롬프트를 작성할 수 있습니다:

*프롬프트:*
```
Explain antibiotics

A:
```

*출력:*
```
Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.
```

여기서 "A:"는 질문-답변(QA) 프롬프트 형식에서 답변이 뒤따름을 명시적으로 알리는 역할을 합니다. 이 예시에서는 그 유용성이 명확하지 않을 수 있지만, 이후 예시에서 더 자세히 다룹니다. 만약 정보가 너무 많다고 느껴진다면, 한 문장으로 요약하도록 모델에 지시할 수도 있습니다.

*프롬프트:*
```
Antibiotics are a type of medication used to treat bacterial infections. They work by either killing the bacteria or preventing them from reproducing, allowing the body’s immune system to fight off the infection. Antibiotics are usually taken orally in the form of pills, capsules, or liquid solutions, or sometimes administered intravenously. They are not effective against viral infections, and using them inappropriately can lead to antibiotic resistance.

Explain the above in one sentence:
```

*출력:*
```
Antibiotics are medications used to treat bacterial infections by either killing the bacteria or stopping them from reproducing, but they are not effective against viruses and overuse can lead to antibiotic resistance.
```

정확성에 대한 평가는 이후 가이드에서 다루겠지만, 위 예시에서 모델은 단락을 한 문장으로 요약하려고 시도했습니다. 지시를 더 정교하게 할 수도 있지만, 이는 이후 장에서 다룹니다. 잠시 멈추고 직접 실험해보는 것도 좋습니다.

---
## 정보 추출(Information Extraction)
언어 모델은 자연어 생성 및 관련 작업을 위해 훈련되었지만, 분류(classification) 등 다양한 자연어 처리(NLP) 작업도 매우 잘 수행할 수 있습니다.

아래는 주어진 단락에서 정보를 추출하는 프롬프트 예시입니다.

*프롬프트:*
```
Author-contribution statements and acknowledgements in research papers should state clearly and specifically whether, and to what extent, the authors used AI technologies such as ChatGPT in the preparation of their manuscript and analysis. They should also indicate which LLMs were used. This will alert editors and reviewers to scrutinize manuscripts more carefully for potential biases, inaccuracies and improper source crediting. Likewise, scientific journals should be transparent about their use of LLMs, for example when selecting submitted manuscripts.

Mention the large language model based product mentioned in the paragraph above:
```

*출력:*
```
The large language model based product mentioned in the paragraph above is ChatGPT.
```

더 나은 결과를 얻을 수 있는 다양한 방법이 있지만, 이미 매우 유용한 결과입니다.

이제 모델에 단순히 무엇을 하라고 지시함으로써 다양한 작업을 수행할 수 있다는 것이 명확해졌을 것입니다. 이는 AI 제품 개발자들이 강력한 제품과 경험을 구축하는 데 이미 활용하고 있는 강력한 기능입니다.

단락 출처: [ChatGPT: five priorities for research](https://www.nature.com/articles/d41586-023-00288-7)

---
## 질문 답변(Question Answering)
모델이 구체적인 답변을 하도록 하려면 프롬프트의 형식을 개선하는 것이 좋습니다. 앞서 다룬 것처럼, 프롬프트는 지침, 컨텍스트, 입력, 출력 표시자를 조합하여 더 나은 결과를 얻을 수 있습니다. 이러한 요소들이 필수는 아니지만, 지침이 구체적일수록 더 좋은 결과를 얻을 수 있습니다. 아래는 더 구조화된 프롬프트 예시입니다.

*프롬프트:*
```
Answer the question based on the context below. Keep the answer short and concise. Respond "Unsure about answer" if not sure about the answer.

Context: Teplizumab traces its roots to a New Jersey drug company called Ortho Pharmaceutical. There, scientists generated an early version of the antibody, dubbed OKT3. Originally sourced from mice, the molecule was able to bind to the surface of T cells and limit their cell-killing potential. In 1986, it was approved to help prevent organ rejection after kidney transplants, making it the first therapeutic antibody allowed for human use.

Question: What was OKT3 originally sourced from?

Answer:
```

*출력:*
```
Mice.
```

컨텍스트 출처: [Nature](https://www.nature.com/articles/d41586-023-00400-x)

---

## 텍스트 분류(Text Classification)
지금까지는 간단한 지시만으로 작업을 수행했습니다. 프롬프트 엔지니어로서 더 나은 지시를 제공하는 능력을 키워야 합니다. 하지만 그것만으로는 충분하지 않은 어려운 사례도 있습니다. 이럴 때는 컨텍스트와 프롬프트에 사용할 수 있는 다양한 요소를 더 고민해야 합니다. 추가로 제공할 수 있는 요소로는 `입력 데이터`나 `예시`가 있습니다.

예시를 통해 텍스트 분류 작업을 시연해보겠습니다.

*프롬프트:*
```
Classify the text into neutral, negative or positive. 

Text: I think the food was okay. 
Sentiment:
```

*출력:*
```
Neutral
```

텍스트를 분류하라는 지시를 주었고, 모델은 'Neutral'이라고 응답했습니다. 맞는 답이지만, 만약 정확히 'neutral'이라는 소문자 레이블을 원한다면 어떻게 해야 할까요? 다양한 방법이 있지만, 여기서는 예시를 추가하여 원하는 동작을 명확히 지정할 수 있습니다. 다시 시도해봅시다:

*프롬프트:*
```
Classify the text into neutral, negative or positive. 

Text: I think the vacation is okay.
Sentiment: neutral 

Text: I think the food was okay. 
Sentiment:
```

*출력:*
```
neutral
```

이번에는 모델이 원하는 대로 'neutral'을 반환했습니다. 예시를 추가하니 출력이 더 구체적으로 맞춰졌음을 알 수 있습니다.

구체성이 왜 중요한지 강조하기 위해 아래 예시를 살펴보세요:

*프롬프트:*
```
Classify the text into nutral, negative or positive. 

Text: I think the vacation is okay.
Sentiment:
```

*출력:*
```
Neutral
```

여기서 문제는 무엇일까요? 힌트: 'nutral'이라는 잘못된 레이블은 모델이 완전히 무시하고, 대신 'Neutral'을 출력합니다. 모델이 해당 레이블에 편향되어 있기 때문입니다. 만약 정말로 'nutral'을 원한다면 어떻게 해야 할까요? 레이블에 대한 설명을 추가하거나 더 많은 예시를 추가해볼 수 있습니다. 자세한 아이디어는 다음 섹션에서 다룹니다.

---

## 대화(Conversation)
프롬프트 엔지니어링으로 달성할 수 있는 더 흥미로운 것 중 하나는 LLM 시스템의 행동, 의도, 정체성을 지시하는 것입니다. 이는 고객 서비스 챗봇과 같은 대화형 시스템을 구축할 때 특히 유용합니다.

예를 들어, 질문에 대해 더 기술적이고 과학적인 답변을 생성하는 대화형 시스템을 만들어봅시다. 지시를 통해 어떻게 행동해야 하는지 명확히 전달하는 점에 주목하세요. 이를 *역할 프롬프팅(role prompting)*이라고도 합니다.

*프롬프트:*
```
The following is a conversation with an AI research assistant. The assistant tone is technical and scientific.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of blackholes?
AI:
```

*출력:*
```
Sure! Black holes are regions of spacetime where the gravitational force is so strong that nothing, not even light, can escape from it. They are created when a very massive star dies and its core collapses in on itself, forming a singularity of infinite density. The intense gravity of the singularity pulls in all the matter and radiation around it, creating the black hole.
```

AI 리서치 어시스턴트가 너무 기술적으로 들리나요? 이번에는 더 쉽게 이해할 수 있도록 시스템에 지시해봅시다.

*프롬프트:*
```
The following is a conversation with an AI research assistant. The assistant answers should be easy to understand even by primary school students.

Human: Hello, who are you?
AI: Greeting! I am an AI research assistant. How can I help you today?
Human: Can you tell me about the creation of black holes?
AI: 
```

*출력:*
```
Sure! Black holes are created when a very massive star runs out of fuel and collapses in on itself. The gravity of the star is so strong that nothing, not even light, can escape its pull. This creates a point in space-time with an intense gravitational force called a black hole.
```

진전이 있었습니다. 더 많은 예시를 추가하면 결과가 더 좋아질 수 있습니다.

---

## 코드 생성(Code Generation)
LLM이 매우 효과적으로 활용되는 분야 중 하나는 코드 생성입니다. Copilot이 대표적인 예시입니다. 영리한 프롬프트로 수행할 수 있는 코드 생성 작업은 매우 다양합니다. 아래 몇 가지 예시를 살펴봅시다.

먼저, 사용자에게 이름을 묻고 인사하는 간단한 프로그램을 시도해봅시다.

*프롬프트:*
```
/*
Ask the user for their name and say "Hello"
*/
```

*출력:*
```
let name = prompt("What is your name?");
console.log(`Hello, ${name}!`);
```

사용할 언어를 명시하지 않아도 동작합니다.

조금 더 복잡한 예시로 넘어가봅시다. 아래 예시는 프롬프트를 조금 더 정교하게 설계하면 LLM이 얼마나 강력해질 수 있는지 보여줍니다.

*프롬프트:*
```
"""
Table departments, columns = [DepartmentId, DepartmentName]
Table students, columns = [DepartmentId, StudentId, StudentName]
Create a MySQL query for all students in the Computer Science Department
"""
```

*출력:*
```
SELECT StudentId, StudentName 
FROM students 
WHERE DepartmentId IN (SELECT DepartmentId FROM departments WHERE DepartmentName = 'Computer Science');
```

매우 인상적입니다. 이 경우, 데이터베이스 스키마에 대한 정보를 제공하고 유효한 MySQL 쿼리를 생성하도록 요청했습니다.

---

## 추론(Reasoning)
오늘날 LLM에게 가장 어려운 작업 중 하나는 일종의 추론(reasoning)을 요구하는 작업입니다. 추론은 LLM에서 복잡한 응용이 등장할 수 있기 때문에 매우 흥미로운 분야입니다.

수학적 능력이 필요한 작업에서 일부 개선이 이루어졌지만, 현재 LLM은 여전히 추론 작업에 어려움을 겪으므로 더 고급 프롬프트 엔지니어링 기법이 필요합니다. 다음 가이드에서 이러한 고급 기법을 다룰 예정입니다. 지금은 산술 능력을 보여주는 몇 가지 기본 예시만 살펴봅니다.

*프롬프트:*
```
What is 9,000 * 9,000?
```

*출력:*
```
81,000,000
```

조금 더 어려운 문제를 시도해봅시다.

*프롬프트:*
```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 

A: 
```

*출력:*
```
No, the odd numbers in this group add up to an odd number: 119.
```

틀렸네요! 프롬프트를 개선해봅시다.

*프롬프트:*
```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 

Solve by breaking the problem into steps. First, identify the odd numbers, add them, and indicate whether the result is odd or even. 
```

*출력:*
```
Odd numbers: 15, 5, 13, 7, 1
Sum: 41 
41 is an odd number.
```

훨씬 낫죠? 참고로, 이 작업을 여러 번 시도하면 모델이 가끔 실패하기도 합니다. 더 나은 지시와 예시를 제공하면 더 정확한 결과를 얻을 수 있습니다.

다음 섹션에서는 이러한 작업과 더 어려운 작업의 성능을 개선하기 위한 고급 프롬프트 엔지니어링 개념과 기법을 다룰 예정입니다.

## 노트북(Notebook)

위의 프롬프트를 Python으로 실습해보고 싶다면, OpenAI 모델을 활용해 프롬프트를 테스트할 수 있는 노트북을 준비했습니다.

- [프롬프트 엔지니어링 시작하기 노트북](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/notebooks/pe-lecture.ipynb)
