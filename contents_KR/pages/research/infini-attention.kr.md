# 효율적인 무한 컨텍스트 트랜스포머(Efficient Infinite Context Transformers)

[Google의 최신 논문](https://arxiv.org/abs/2404.07143)은 압축 메모리(compressive memory)를 일반 점곱(dot-product) 어텐션 레이어에 통합하는 방식을 제안합니다.

이 연구의 목표는 트랜스포머(Transformer) LLM(대형 언어 모델, Large Language Model)이 제한된 메모리와 연산량으로도 무한히 긴 입력을 효과적으로 처리할 수 있도록 하는 것입니다.

저자들은 Infini-attention이라는 새로운 어텐션 기법을 제안합니다. 이 기법은 압축 메모리 모듈을 기존 어텐션 메커니즘에 결합합니다.

![](../../img/research/infini-attention.png)

Infini-attention은 마스킹된 로컬 어텐션(masked local attention)과 장기 선형 어텐션(long-term linear attention)을 하나의 트랜스포머 블록에 통합합니다. 이를 통해 Infini-Transformer 모델은 장기 및 단기 컨텍스트 의존성을 모두 효율적으로 처리할 수 있습니다.

이 접근법은 메모리 114배 압축률로 long-context 언어 모델링에서 기존 모델 대비 뛰어난 성능을 보입니다!

또한, 10억(1B) 파라미터 LLM이 자연스럽게 100만(1M) 토큰 시퀀스 길이까지 확장 가능함을 보였고, 80억(8B) 모델은 50만(500K) 길이의 책 요약 과제에서 새로운 SOTA(최첨단) 결과를 달성했습니다.

장기 컨텍스트 LLM의 중요성이 커지는 현 시점에서, 효과적인 메모리 시스템은 LLM의 강력한 추론, 계획, 지속적 적응, 그리고 이전에 볼 수 없었던 새로운 능력을 실현할 수 있는 열쇠가 될 것입니다. 