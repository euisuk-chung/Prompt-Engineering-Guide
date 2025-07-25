# OLMo

이 가이드에서는 프롬프트와 사용 예시를 포함한 Open Language Mode(OLMo)의 개요를 제공합니다. 이 가이드는 또한 OLMo와 관련된 팁, 응용 프로그램, 한계, 논문, 추가 읽기 자료를 포함합니다.

## OLMo 소개(Introduction to OLMo)

Allen Institute of AI는 OLMo라는 새로운 오픈 언어 모델과 프레임워크를 [출시](https://blog.allenai.org/olmo-open-language-model-87ccfc95f580)했습니다. 이 노력은 언어 모델 연구를 집단적으로 가속화하기 위해 데이터, 훈련 코드, 모델, 평가 코드에 대한 완전한 접근을 제공하기 위한 것입니다.

그들의 첫 번째 출시에는 7B 매개변수 규모의 4개 변형과 1B 규모의 1개 모델이 포함되며, 모두 최소 2T 토큰으로 훈련되었습니다. 이는 곧 출시될 65B OLMo 모델을 포함한 많은 출시 중 첫 번째입니다.

출시에는 다음이 포함됩니다:

- 데이터를 생성하는 [코드](https://github.com/allenai/dolma)를 포함한 전체 훈련 데이터
- 전체 모델 가중치, [훈련 코드](https://github.com/allenai/OLMo), 로그, 메트릭, 추론 코드
- 모델당 여러 체크포인트
- [평가 코드](https://github.com/allenai/OLMo-Eval)
- 미세조정 코드

모든 코드, 가중치, 중간 체크포인트는 [Apache 2.0 라이선스](https://github.com/allenai/OLMo#Apache-2.0-1-ov-file) 하에 출시됩니다.

## OLMo-7B

OLMo-7B와 OLMo-1B 모델 모두 디코더 전용 트랜스포머 아키텍처를 채택합니다. PaLM과 Llama와 같은 다른 모델의 개선사항을 따릅니다:

- 편향 없음
- 비모수적 레이어 정규화
- SwiGLU 활성화 함수
- 회전 위치 임베딩(RoPE)
- 50,280개의 어휘

## Dolma 데이터셋(Dolma Dataset)

이 출시에는 [Dolma](https://github.com/allenai/dolma)라는 사전 훈련 데이터셋의 출시도 포함됩니다. 이는 7개의 서로 다른 데이터 소스에서 획득한 50억 개 문서에 걸쳐 3조 토큰의 다양한 다중 소스 코퍼스입니다. Dolma의 생성에는 언어 필터링, 품질 필터링, 콘텐츠 필터링, 중복 제거, 다중 소스 혼합, 토큰화와 같은 단계가 포함됩니다.

훈련 데이터셋에는 Dolma에서 2T 토큰 샘플이 포함됩니다. 토큰은 각 문서 끝에 특별한 `EOS` 토큰을 추가한 후 함께 연결됩니다. 훈련 인스턴스는 2048 토큰의 연속적인 청크 그룹을 포함하며, 이는 또한 섞입니다.

모델을 훈련하기 위한 더 자세한 훈련 세부사항과 하드웨어 사양은 논문에서 찾을 수 있습니다.

## 결과(Results)

모델은 [Catwalk](https://github.com/allenai/catwalk)를 사용하여 다운스트림 작업에서 평가됩니다. OLMo 모델은 Falcon과 Llama 2와 같은 다른 여러 공개 모델과 비교됩니다. 구체적으로, 모델은 모델의 상식 추론 능력을 측정하는 것을 목표로 하는 작업 세트에서 평가됩니다. 다운스트림 평가 스위트에는 `piqa`와 `hellaswag`와 같은 데이터셋이 포함됩니다. 저자들은 순위 분류(즉, 완성은 가능성에 따라 순위가 매겨짐)를 사용하여 제로샷 평가를 수행하고 정확도가 보고됩니다. OLMo-7B는 2개의 엔드 작업에서 모든 다른 모델을 능가하고 8/9 엔드 작업에서 상위 3위를 유지합니다. 아래 차트에서 결과 요약을 참조하세요.

## OLMo를 위한 프롬프팅 가이드(Prompting Guide for OLMo)

곧 나올 예정입니다...

---

그림 출처: [OLMo: Accelerating the Science of Language Models](https://allenai.org/olmo/olmo-paper.pdf)

## 참고문헌(References)

- [OLMo: Open Language Model](https://blog.allenai.org/olmo-open-language-model-87ccfc95f580)
- [OLMo: Accelerating the Science of Language Models](https://allenai.org/olmo/olmo-paper.pdf)