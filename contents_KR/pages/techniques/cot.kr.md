# 연쇄적 사고 프롬프트(Chain-of-Thought Prompting)

## 연쇄적 사고(Chain-of-Thought, CoT) 프롬프트

이미지 출처: [Wei 등(2022)](https://arxiv.org/abs/2201.11903)

[Wei 등(2022)](https://arxiv.org/abs/2201.11903)에서 소개된 연쇄적 사고(CoT) 프롬프트는 중간 추론 단계를 통해 복잡한 추론 능력을 활성화합니다. few-shot 프롬프트와 결합하면, 응답 전 추론이 필요한 복잡한 과제에서 더 나은 결과를 얻을 수 있습니다.

*프롬프트 예시:*
```
The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
A: Adding all the odd numbers (9, 15, 1) gives 25. The answer is False.

The odd numbers in this group add up to an even number: 17,  10, 19, 4, 8, 12, 24.
A: Adding all the odd numbers (17, 19) gives 36. The answer is True.

The odd numbers in this group add up to an even number: 16,  11, 14, 4, 8, 13, 24.
A: Adding all the odd numbers (11, 13) gives 24. The answer is True.

The odd numbers in this group add up to an even number: 17,  9, 10, 12, 13, 4, 2.
A: Adding all the odd numbers (17, 9, 13) gives 39. The answer is False.

The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 
A:
```

*출력 예시:*
```
Adding all the odd numbers (15, 5, 13, 7, 1) gives 41. The answer is False.
```

중간 추론 단계를 제공하면 완벽한 결과가 나오는 것을 확인할 수 있습니다. 실제로 예시를 더 적게(단 1개만) 제공해도 과제를 해결할 수 있습니다.

*프롬프트 예시:*
```
The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
A: Adding all the odd numbers (9, 15, 1) gives 25. The answer is False.

The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 
A:
```

*출력 예시:*
```
Adding all the odd numbers (15, 5, 13, 7, 1) gives 41. The answer is False.
```

저자들은 이러한 능력이 충분히 큰 언어 모델에서 나타나는 창발적(emergent) 특성임을 주장합니다.

## 제로샷 CoT 프롬프트(Zero-shot COT Prompting)

이미지 출처: [Kojima 등(2022)](https://arxiv.org/abs/2205.11916)

최근 제안된 [제로샷 CoT](https://arxiv.org/abs/2205.11916)(Kojima 등, 2022) 아이디어는, 원래 프롬프트에 "Let's think step by step"을 추가하는 방식입니다. 간단한 문제로 모델의 성능을 확인해봅니다.

*프롬프트 예시:*
```
I went to the market and bought 10 apples. I gave 2 apples to the neighbor and 2 to the repairman. I then went and bought 5 more apples and ate 1. How many apples did I remain with?
```

*출력 예시:*
```
11 apples
```

정답이 아닙니다! 이제 특수 프롬프트를 추가해봅니다.

*프롬프트 예시:*
```
I went to the market and bought 10 apples. I gave 2 apples to the neighbor and 2 to the repairman. I then went and bought 5 more apples and ate 1. How many apples did I remain with?

Let's think step by step.
```

*출력 예시:*
```
First, you started with 10 apples.
You gave away 2 apples to the neighbor and 2 to the repairman, so you had 6 apples left.
Then you bought 5 more apples, so now you had 11 apples.
Finally, you ate 1 apple, so you would remain with 10 apples.
```

이처럼 간단한 프롬프트가 효과적으로 작동함을 알 수 있습니다. 예시를 많이 넣기 어려운 상황에서 특히 유용합니다.

## 자동 연쇄적 사고(Auto-CoT)

연쇄적 사고 프롬프트를 시연(demonstration)과 함께 적용할 때, 효과적이고 다양한 예시를 수작업으로 설계해야 하므로 비효율적일 수 있습니다. [Zhang 등(2022)](https://arxiv.org/abs/2210.03493)은 "Let's think step by step" 프롬프트와 LLM을 활용해, 각 예시별로 추론 체인을 자동 생성하는 접근법을 제안했습니다. 자동 생성된 체인에도 오류가 있을 수 있으므로, 다양한 시연(demonstration)의 확보가 중요합니다. 이 논문에서는 Auto-CoT라는 방법을 제안하며, 다양한 질문을 샘플링하고, 각 질문에 대해 추론 체인을 생성해 시연을 구성합니다.

Auto-CoT는 두 단계로 구성됩니다:
- 1단계: **질문 클러스터링(question clustering)** — 데이터셋의 질문을 몇 개의 클러스터로 분할
- 2단계: **시연 샘플링(demonstration sampling)** — 각 클러스터에서 대표 질문을 선택하고, Zero-Shot-CoT와 간단한 휴리스틱(예: 질문 길이 60토큰, 추론 단계 5개 등)으로 추론 체인을 생성

이 과정을 통해 모델이 간단하고 정확한 시연을 활용하도록 유도합니다.

아래는 전체 과정의 시각화입니다:

이미지 출처: [Zhang 등(2022)](https://arxiv.org/abs/2210.03493)

Auto-CoT 코드는 [여기](https://github.com/amazon-science/auto-cot)에서 확인할 수 있습니다.