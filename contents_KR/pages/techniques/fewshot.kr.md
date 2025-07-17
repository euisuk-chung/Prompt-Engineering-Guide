# 퓨샷 프롬프트(Few-Shot Prompting)

대형 언어 모델(LLM)은 놀라운 제로샷(Zero-shot) 능력을 보여주지만, 제로샷 설정에서는 더 복잡한 과제에서 한계를 보입니다. 퓨샷 프롬프트(Few-shot Prompting)는 프롬프트 내에 데모(예시)를 제공하여 인컨텍스트 학습(In-context Learning)을 가능하게 하는 기법입니다. 데모는 이후 예시에서 모델이 더 나은 응답을 생성하도록 조건을 부여하는 역할을 합니다.

[Touvron 외, 2023](https://arxiv.org/pdf/2302.13971.pdf)에 따르면, 퓨샷 특성은 모델이 충분히 커졌을 때 처음 등장했다고 합니다[(Kaplan 외, 2020)](https://arxiv.org/abs/2001.08361).

[Brown 외, 2020](https://arxiv.org/abs/2005.14165)에서 제시된 예시로 퓨샷 프롬프트를 살펴보겠습니다. 이 예시에서 과제는 새로운 단어를 올바르게 문장에 사용하는 것입니다.

*프롬프트:*
```markdown
A "whatpu" is a small, furry animal native to Tanzania. An example of a sentence that uses the word whatpu is:
We were traveling in Africa and we saw these very cute whatpus.

To do a "farduddle" means to jump up and down really fast. An example of a sentence that uses the word farduddle is:
```

*출력:*
```
When we won the game, we all started to farduddle in celebration.
```

단 하나의 예시(1-shot)만 제공해도 모델이 과제를 수행하는 법을 학습한 것을 볼 수 있습니다. 더 어려운 과제에는 데모 수(3-shot, 5-shot, 10-shot 등)를 늘려 실험할 수 있습니다.

[Min 외, 2022](https://arxiv.org/abs/2202.12837) 연구에 따르면, 퓨샷에서 데모/예시를 다룰 때 다음과 같은 팁이 있습니다:

- "데모가 지정하는 레이블 공간과 입력 텍스트 분포가 모두 중요하다(개별 입력에 대한 레이블이 맞는지와 무관하게)"
- 사용하는 포맷도 성능에 중요한 역할을 하며, 무작위 레이블을 사용하더라도 레이블이 아예 없는 것보다 훨씬 낫다.
- 실제 레이블 분포에서 무작위로 레이블을 선택하는 것이(균등 분포보다) 더 도움이 된다.

몇 가지 예시를 살펴보겠습니다. 먼저 레이블을 무작위로 할당한 예시입니다:

*프롬프트:*
```
This is awesome! // Negative
This is bad! // Positive
Wow that movie was rad! // Positive
What a horrible show! //
```

*출력:*
```
Negative
```

레이블이 무작위임에도 불구하고 정답을 얻었습니다. 포맷을 유지한 것도 도움이 됩니다. 실제로 최신 GPT 모델은 무작위 포맷에도 점점 더 강인해지고 있습니다. 예시:

*프롬프트:*
```
Positive This is awesome! 
This is bad! Negative
Wow that movie was rad!
Positive
What a horrible show! --
```

*출력:*
```
Negative
```

위 예시에서는 포맷이 일관되지 않지만 모델은 여전히 정답을 예측합니다. 다양한 과제와 프롬프트 변형에 대해 이 현상이 유지되는지 더 면밀한 분석이 필요합니다.

### 퓨샷 프롬프트의 한계

표준 퓨샷 프롬프트는 많은 과제에서 잘 동작하지만, 복잡한 추론 과제에서는 완벽한 기법이 아닙니다. 그 이유를 예시로 살펴보겠습니다. 앞서 다룬 다음 과제를 기억하시나요?

```
The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 

A: 
```

이 예시를 다시 시도하면 모델은 다음과 같이 출력합니다:

```
Yes, the odd numbers in this group add up to 107, which is an even number.
```

정답이 아닙니다. 이는 시스템의 한계와 더 고급 프롬프트 엔지니어링의 필요성을 보여줍니다.

예시를 추가해 퓨샷 프롬프트가 결과를 개선하는지 확인해봅시다.

*프롬프트:*
```
The odd numbers in this group add up to an even number: 4, 8, 9, 15, 12, 2, 1.
A: The answer is False.

The odd numbers in this group add up to an even number: 17,  10, 19, 4, 8, 12, 24.
A: The answer is True.

The odd numbers in this group add up to an even number: 16,  11, 14, 4, 8, 13, 24.
A: The answer is True.

The odd numbers in this group add up to an even number: 17,  9, 10, 12, 13, 4, 2.
A: The answer is False.

The odd numbers in this group add up to an even number: 15, 32, 5, 13, 82, 7, 1. 
A: 
```

*출력:*
```
The answer is True.
```

효과가 없었습니다. 퓨샷 프롬프트만으로는 이런 유형의 추론 문제에서 신뢰할 만한 답을 얻기 어렵다는 점을 보여줍니다. 위 예시는 과제에 대한 기본 정보를 제공하지만, 실제로는 더 많은 추론 단계를 요구합니다. 문제를 단계별로 나누어 모델에 시연하는 것이 도움이 될 수 있습니다. 최근에는 [체인 오브 쏘트(Chain-of-Thought, CoT) 프롬프트](https://arxiv.org/abs/2201.11903)가 복잡한 산술, 상식, 기호 추론 과제에 널리 활용되고 있습니다.

결론적으로, 예시를 제공하는 것은 일부 과제 해결에 유용합니다. 제로샷, 퓨샷 프롬프트만으로 충분하지 않다면, 모델이 해당 과제에 대해 충분히 학습하지 못했을 수 있습니다. 이 경우 파인튜닝이나 더 고급 프롬프트 기법을 실험해보는 것이 권장됩니다. 다음 장에서는 최근 각광받는 프롬프트 기법인 체인 오브 쏘트 프롬프트를 다룹니다.