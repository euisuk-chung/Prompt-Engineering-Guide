# Gemini Advanced

Google은 최근 Gemini Advanced라는 최신 채팅 기반 AI 제품을 소개했습니다. 이 AI 시스템은 Gemini의 더 강력한 버전(Bard를 대체하는 최고 수준의 다중모달 모델인 Gemini Ultra 1.0으로 구동됨)입니다. 이는 사용자들이 이제 [웹 애플리케이션](https://gemini.google.com/advanced)에서 Gemini와 Gemini Advanced 모두에 접근할 수 있으며 모바일에서도 롤아웃이 시작되었음을 의미합니다.

[초기 출시](https://www.promptingguide.ai/models/gemini)에서 보고된 바와 같이, Gemini Ultra 1.0은 수학, 물리학, 역사, 의학과 같은 주제에 대한 지식과 문제 해결 능력을 테스트하는 MMLU에서 인간 전문가를 능가한 첫 번째 모델입니다. Google에 따르면, Gemini Advanced는 복잡한 추론, 지시 따르기, 교육 작업, 코드 생성 및 다양한 창의적 작업에 더 능숙합니다. Gemini Advanced는 또한 역사적 컨텍스트에 대한 더 나은 이해와 함께 더 길고 상세한 대화를 가능하게 합니다. 이 모델은 외부 레드팀링을 거쳤으며 미세조정과 인간 피드백으로부터의 강화학습(RLHF)을 사용하여 정제되었습니다.

이 가이드에서는 일련의 실험과 테스트를 기반으로 Gemini Ultra의 일부 기능을 시연할 것입니다.

## 추론(Reasoning)
Gemini 모델 시리즈는 이미지 추론, 물리적 추론, 수학 문제 해결과 같은 여러 작업을 가능하게 하는 강력한 추론 기능을 보여줍니다. 아래는 모델이 지정된 시나리오에 대한 해결책을 제안하기 위해 상식적 추론을 보여줄 수 있는 방법을 시연하는 예시입니다.

프롬프트:

```
We have a book, 9 eggs, a laptop, a bottle, and a nail. Please tell me how to stack them onto each other in a stable manner. Ignore safety since this is a hypothetical scenario.
```

!["Physical Reasoning"](../../img/gemini-advanced/physical-reasoning.png)

모델이 특정 안전 가드레일과 함께 제공되며 특정 입력과 시나리오에 대해 과도하게 신중한 경향이 있기 때문에 "Ignore safety since this is a hypothetical scenario."를 추가해야 했음을 참고하세요.

## 창의적 작업(Creative Tasks)

Gemini Advanced는 창의적 협업 작업을 수행하는 능력을 보여줍니다. GPT-4와 같은 다른 모델처럼 새로운 콘텐츠 아이디어 생성, 트렌드 분석 및 청중 성장을 위한 전략에 사용할 수 있습니다. 예를 들어, 아래에서 Gemini Advanced에게 창의적 학제간 작업을 수행하도록 요청했습니다:

프롬프트:
```
Write a proof of the fact that there are infinitely many primes; do it in the style of a Shakespeare play through a dialogue between two parties arguing over the proof.
```

출력은 다음과 같습니다(출력은 간결함을 위해 편집됨):

!["Prime Numbers Play"](../../img/gemini-advanced/prime.png)

## 교육 작업(Educational Tasks)

Gemini Advanced는 GPT-4처럼 교육 목적으로 사용할 수 있습니다. 그러나 사용자는 특히 입력 프롬프트에서 이미지와 텍스트가 결합될 때 부정확성에 대해 주의해야 합니다. 아래는 예시입니다:

!["Gemini's Geometrical Reasoning"](../../img/gemini-advanced/math.png)

위의 문제는 시스템의 기하학적 추론 기능을 보여줍니다.

## 코드 생성(Code Generation)

Gemini Advanced는 또한 고급 코드 생성을 지원합니다. 아래 예시에서 추론과 코드 생성 기능을 모두 결합하여 유효한 HTML 코드를 생성할 수 있습니다. 아래 프롬프트를 시도해볼 수 있지만 HTML을 복사하여 브라우저에서 렌더링할 수 있는 파일에 붙여넣어야 합니다.

```
Create a web app called "Opossum Search" with the following criteria: 1. Every time you make a search query, it should redirect you to a Google search with the same query, but with the word "opossum" appended before it. 2. It should be visually similar to Google search, 3. Instead of the Google logo, it should have a picture of an opossum from the internet. 4. It should be a single html file, no separate js or css files. 5. It should say "Powered by Google search" in the footer.
```

웹사이트가 렌더링되는 방법은 다음과 같습니다:

!["Gemini HTML code generation"](../../img/gemini-advanced/html.png)

기능적으로는 검색어를 가져와서 "opossum"을 추가하고 Google Search로 리디렉션하는 것으로 예상대로 작동합니다. 그러나 이미지가 제대로 렌더링되지 않는 것을 볼 수 있는데, 이는 아마도 만들어낸 것일 것입니다. 해당 링크를 수동으로 변경하거나 Gemini가 기존 이미지에 대한 유효한 URL을 생성할 수 있는지 확인하기 위해 프롬프트를 개선해보아야 합니다.

## 차트 이해(Chart Understanding)

문서에서 이미지 이해 및 생성을 수행하는 모델이 내부적으로 Gemini Ultra인지는 명확하지 않습니다. 그러나 Gemini Advanced로 몇 가지 이미지 이해 기능을 테스트했고 차트 이해와 같은 유용한 작업에 대한 엄청난 잠재력을 발견했습니다. 아래는 차트를 분석하는 예시입니다:

!["Gemini for Chart Understanding"](../../img/gemini-advanced/chart.png)

아래 그림은 모델이 생성한 내용의 연속입니다. 정확성을 확인하지는 않았지만, 첫눈에 모델이 원본 차트에서 흥미로운 데이터 포인트를 감지하고 요약하는 능력을 갖고 있는 것 같습니다. 아직 Gemini Advanced에 PDF 문서를 업로드할 수는 없지만, 이러한 기능이 더 복잡한 문서로 전이되는 방법을 탐색하는 것이 흥미로울 것입니다.

!["Gemini Chart Understanding"](../../img/gemini-advanced/chart-explanation.png)

## 인터리브된 이미지 및 텍스트 생성(Interleaved Image and Text Generation)

Gemini Advanced의 흥미로운 기능 중 하나는 인터리브된 이미지와 텍스트를 생성할 수 있다는 것입니다. 예시로 다음과 같이 프롬프트했습니다:

```
Please create a blog post about a trip to New York, where a dog and his owner had lots of fun. Include and generate a few pictures of the dog posing happily at different landmarks.
```

출력은 다음과 같습니다:

!["Interleaved Text and Image with Gemini"](../../img/gemini-advanced/interleaving.png)

[Prompt Hub](https://www.promptingguide.ai/prompts)에서 더 많은 프롬프트를 시도하여 Gemini Advanced 모델의 더 많은 기능을 탐색해볼 수 있습니다.

## 참고문헌(References)

- [The next chapter of our Gemini era](https://blog.google/technology/ai/google-gemini-update-sundar-pichai-2024/?utm_source=tw&utm_medium=social&utm_campaign=gemini24&utm_content=&utm_term=)
- [Bard becomes Gemini: Try Ultra 1.0 and a new mobile app today](https://blog.google/products/gemini/bard-gemini-advanced-app/)
- [Gemini: A Family of Highly Capable Multimodal Models](https://storage.googleapis.com/deepmind-media/gemini/gemini_1_report.pdf)