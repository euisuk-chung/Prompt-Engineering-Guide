# LLM의 신뢰성(Trustworthiness in LLMs)

신뢰할 수 있는 LLM(대형 언어 모델, Large Language Model)은 의료, 금융 등 고위험(high-stake) 도메인에서 응용 시스템을 구축하는 데 매우 중요합니다. ChatGPT와 같은 LLM은 인간 수준의 자연스러운 응답을 생성할 수 있지만, 진실성(truthfulness), 안전성(safety), 프라이버시(privacy) 등 다양한 차원에서 신뢰성을 보장하지는 않습니다.

[Sun 등(2024)](https://arxiv.org/abs/2401.05561)은 최근 LLM의 신뢰성에 대한 종합적 연구를 발표하며, 주요 도전과제, 벤치마크, 평가, 접근법 분석, 미래 방향성을 논의했습니다.

현재 LLM을 실제 서비스에 적용할 때 가장 큰 과제 중 하나가 바로 신뢰성입니다. 해당 서베이에서는 신뢰할 수 있는 LLM을 위한 8가지 원칙을 제시하며, 6개 차원(진실성, 안전성, 공정성, 견고성, 프라이버시, 기계 윤리)에 대한 벤치마크도 함께 제안합니다.

저자는 LLM의 신뢰성을 6가지 측면에서 평가할 수 있는 다음과 같은 벤치마크를 제시했습니다:

![](../../img/llms/trustllm.png)

아래는 신뢰할 수 있는 LLM의 8가지 차원에 대한 정의입니다.

![](../../img/llms/trust-dimensions.png)

## 주요 연구 결과

이 연구에서는 TrustLLM 벤치마크(30개 이상 데이터셋)로 16종의 주요 LLM을 평가했습니다. 주요 결과는 다음과 같습니다:

- 상용 LLM이 전반적으로 오픈소스 모델보다 신뢰성 측면에서 우수하지만, 일부 오픈소스 모델도 격차를 빠르게 좁히고 있음
- GPT-4, Llama 2 등은 고정관념(stereotype) 거부 및 적대적 공격(adversarial attack)에 대한 강인성이 뛰어남
- Llama 2 등 오픈소스 모델도 별도의 특수 검열 도구 없이 신뢰성에서 상용 모델과 유사한 성능을 보임. 논문에서는 Llama 2가 신뢰성에 과도하게 최적화되어, 일부 과제에서 유용성이 저하되거나 정상 프롬프트를 유해 입력으로 오인하는 경우가 있다고 지적함

## 주요 인사이트

논문에서 다룬 신뢰성 각 차원별 주요 인사이트는 다음과 같습니다:

- **진실성(Truthfulness)**: LLM은 학습 데이터의 노이즈, 허위 정보, 구식 정보 등으로 인해 진실성에서 종종 어려움을 겪음. 외부 지식 소스에 접근 가능한 LLM이 진실성에서 더 나은 성능을 보임
- **안전성(Safety)**: 오픈소스 LLM은 탈옥(jailbreak), 독성(toxicity), 오용(misuse) 등 안전성 측면에서 상용 모델에 비해 뒤처짐. 과도한 안전성 조치와 실용성 간 균형이 과제임
- **공정성(Fairness)**: 대부분의 LLM은 고정관념 인식에서 만족스러운 성능을 보이지 않음. GPT-4 등 최신 모델도 약 65% 정확도에 그침
- **견고성(Robustness)**: LLM의 견고성은 특히 개방형(open-ended), 분포 외(out-of-distribution) 과제에서 큰 변동성을 보임
- **프라이버시(Privacy)**: LLM은 프라이버시 규범을 인지하지만, 민감 정보 처리 능력은 모델별로 상이함. 예시로, 일부 모델은 Enron 이메일 데이터셋에서 정보 유출을 보임
- **기계 윤리(Machine Ethics)**: LLM은 기본적인 도덕 원칙을 이해하지만, 복잡한 윤리적 상황에서는 한계가 있음

## LLM 신뢰성 리더보드

저자들은 [여기](https://trustllmbenchmark.github.io/TrustLLM-Website/leaderboard.html)에서 신뢰성 리더보드도 공개했습니다. 예를 들어, 아래 표는 진실성 차원에서 다양한 모델의 성능을 보여줍니다. 웹사이트 설명에 따르면, "더 신뢰할 수 있는 LLM은 ↑가 붙은 지표는 높을수록, ↓가 붙은 지표는 낮을수록 좋다"고 명시되어 있습니다.

![](../../img/llms/truthfulness-leaderboard.png)

## 코드

LLM의 다양한 신뢰성 차원을 평가할 수 있는 전체 평가 키트는 아래 깃허브에서 확인할 수 있습니다.

코드: https://github.com/HowieHwong/TrustLLM

## 참고문헌

이미지 출처/논문: [TrustLLM: Trustworthiness in Large Language Models](https://arxiv.org/abs/2401.05561) (2024년 1월 10일)
