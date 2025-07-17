# 생성 지식 프롬프트(Generated Knowledge Prompting)

이미지 출처: [Liu 외, 2022](https://arxiv.org/pdf/2110.08387.pdf)

LLM은 지속적으로 발전하고 있으며, 지식이나 정보를 통합해 더 정확한 예측을 하도록 하는 기법이 널리 활용되고 있습니다.

이와 유사한 아이디어로, 모델이 예측을 내리기 전에 지식을 직접 생성할 수 있을까요? [Liu 외, 2022](https://arxiv.org/pdf/2110.08387.pdf) 논문에서는 프롬프트의 일부로 활용할 지식을 생성하는 방식을 시도합니다. 특히, 이러한 방식이 상식 추론과 같은 과제에 얼마나 도움이 되는지 살펴봅니다.

간단한 프롬프트 예시를 살펴보겠습니다:

*프롬프트:*
```
Part of golf is trying to get a higher point total than others. Yes or No?
```

*출력:*
```
Yes.
```

이런 실수는 LLM이 세상에 대한 더 많은 지식이 필요한 과제에서 한계를 드러냅니다. 지식 생성을 통해 어떻게 개선할 수 있을까요?

먼저, 몇 가지 "지식"을 생성합니다:

*프롬프트:*
```
Input: Greece is larger than mexico.
Knowledge: Greece is approximately 131,957 sq km, while Mexico is approximately 1,964,375 sq km, making Mexico 1,389% larger than Greece.

Input: Glasses always fog up.
Knowledge: Condensation occurs on eyeglass lenses when water vapor from your sweat, breath, and ambient humidity lands on a cold surface, cools, and then changes into tiny drops of liquid, forming a film that you see as fog. Your lenses will be relatively cool compared to your breath, especially when the outside air is cold.

Input: A fish is capable of thinking.
Knowledge: Fish are more intelligent than they appear. In many areas, such as memory, their cognitive powers match or exceed those of ’higher’ vertebrates including non-human primates. Fish’s long-term memories help them keep track of complex social relationships.

Input: A common effect of smoking lots of cigarettes in one’s lifetime is a higher than normal chance of getting lung cancer.
Knowledge: Those who consistently averaged less than one cigarette per day over their lifetime had nine times the risk of dying from lung cancer than never smokers. Among people who smoked between one and 10 cigarettes per day, the risk of dying from lung cancer was nearly 12 times higher than that of never smokers.

Input: A rock is the same size as a pebble.
Knowledge: A pebble is a clast of rock with a particle size of 4 to 64 millimetres based on the Udden-Wentworth scale of sedimentology. Pebbles are generally considered larger than granules (2 to 4 millimetres diameter) and smaller than cobbles (64 to 256 millimetres diameter).

Input: Part of golf is trying to get a higher point total than others.
Knowledge:
```

*지식 1:*
```
The objective of golf is to play a set of holes in the least number of strokes. A round of golf typically consists of 18 holes. Each hole is played once in the round on a standard golf course. Each stroke is counted as one point, and the total number of strokes is used to determine the winner of the game.
```

*지식 2:*
```
Golf is a precision club-and-ball sport in which competing players (or golfers) use many types of clubs to hit balls into a series of holes on a course using the fewest number of strokes. The goal is to complete the course with the lowest score, which is calculated by adding up the total number of strokes taken on each hole. The player with the lowest score wins the game.
```

논문([Liu 외, 2022](https://arxiv.org/pdf/2110.08387.pdf))에서 제시된 프롬프트를 활용했습니다.

다음 단계는 지식을 통합해 예측을 얻는 것입니다. 답변 형식을 유도하기 위해 질문을 QA 포맷으로 재구성했습니다.

*프롬프트:*
```
Question: Part of golf is trying to get a higher point total than others. Yes or No?

Knowledge: The objective of golf is to play a set of holes in the least number of strokes. A round of golf typically consists of 18 holes. Each hole is played once in the round on a standard golf course. Each stroke is counted as one point, and the total number of strokes is used to determine the winner of the game.

Explain and Answer: 
```

*답변 1 (신뢰도 매우 높음):*
```
No, the objective of golf is not to get a higher point total than others. Rather, the objective is to play a set of holes in the least number of strokes. The total number of strokes is used to determine the winner of the game, not the total number of points.
```

```
Question: Part of golf is trying to get a higher point total than others. Yes or No?

Knowledge: Golf is a precision club-and-ball sport in which competing players (or golfers) use many types of clubs to hit balls into a series of holes on a course using the fewest number of strokes. The goal is to complete the course with the lowest score, which is calculated by adding up the total number of strokes taken on each hole. The player with the lowest score wins the game.

Explain and Answer:
```

*답변 2 (신뢰도는 다소 낮음):*
```
Yes, part of golf is trying to get a higher point total than others. Each player tries to complete the course with the lowest score, which is calculated by adding up the total number of strokes taken on each hole. The player with the lowest score wins the game.
```

이 예시에서는 흥미로운 현상이 나타납니다. 첫 번째 답변에서는 모델이 매우 확신을 보였지만, 두 번째 답변에서는 그렇지 않았습니다. 시연을 위해 과정을 단순화했지만, 실제로는 최종 답에 도달하기까지 고려해야 할 세부 사항이 더 있습니다. 자세한 내용은 논문을 참고하세요.
