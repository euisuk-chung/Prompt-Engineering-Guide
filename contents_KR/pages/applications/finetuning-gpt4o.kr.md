# GPT-4o 모델 파인튜닝(Fine-Tuning with GPT-4o Models)

OpenAI는 최근 [GPT-4o 및 GPT-4o mini 모델의 파인튜닝 기능](https://openai.com/index/gpt-4o-fine-tuning/)을 공식 출시했습니다. 이 기능을 통해 개발자는 GPT-4o 모델을 특정 용도에 맞게 맞춤화하여 성능을 높이고, 출력 결과를 세밀하게 제어할 수 있습니다.

## 파인튜닝 세부 정보 및 비용(Fine-Tuning Details and Costs)

개발자는 [파인튜닝 대시보드](https://platform.openai.com/finetune)를 통해 `GPT-4o-2024-08-06` 체크포인트에 접근해 파인튜닝을 진행할 수 있습니다. 이를 통해 응답 구조, 톤, 복잡한 도메인별 지침 준수 등 다양한 맞춤화가 가능합니다.

GPT-4o 파인튜닝 비용은 학습 시 백만 토큰당 25달러, 추론 시 입력 토큰 백만 개당 3.75달러, 출력 토큰 백만 개당 15달러입니다. 이 기능은 유료 요금제 개발자에게만 제공됩니다.

## 실험용 무료 학습 토큰(Free Training Tokens for Experimentation)

OpenAI는 새로운 기능 탐색을 장려하기 위해 9월 23일까지 한시적으로 무료 학습 토큰을 제공합니다. 개발자는 GPT-4o 기준 하루 100만 개, GPT-4o mini 기준 하루 200만 개의 무료 학습 토큰을 사용할 수 있습니다. 이를 활용해 파인튜닝 모델의 다양한 응용 가능성을 실험해볼 수 있습니다.

## 활용 사례: 감정 분류(Use Case: Emotion Classification)

https://youtu.be/UJ7ry7Qp2Js?si=ZU3K0ZVNfQjnlZgo

위 가이드에서는 감정 분류(emotion classification) 모델을 파인튜닝하는 실제 예시를 소개합니다. [JSONL 형식 데이터셋](https://github.com/dair-ai/datasets/tree/main/openai)에 감정 레이블이 부여된 텍스트 샘플을 활용해, GPT-4o mini를 감정별로 텍스트를 분류하도록 파인튜닝할 수 있습니다.

이 데모는 특정 과제에 맞춘 파인튜닝이 모델 성능을 크게 향상시킬 수 있음을 보여줍니다. 표준 모델 대비 정확도가 크게 개선됩니다.

## 파인튜닝 모델 접근 및 평가(Accessing and Evaluating Fine-Tuned Models)

파인튜닝이 완료되면, 개발자는 OpenAI Playground에서 맞춤형 모델을 직접 테스트할 수 있습니다. Playground에서는 다양한 입력으로 상호작용하며 모델 성능을 확인할 수 있습니다. 더 체계적인 평가는 OpenAI API를 통해 실제 애플리케이션에 통합해 진행할 수 있습니다.

OpenAI의 GPT-4o 파인튜닝 기능은 LLM을 특화된 과제에 활용하려는 개발자에게 새로운 가능성을 열어줍니다. 