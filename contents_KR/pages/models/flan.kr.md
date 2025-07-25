# 지시 미세조정 언어 모델의 확장(Scaling Instruction-Finetuned Language Models)

## 새로운 점은 무엇인가요?(What's new?)

<Screenshot src={FLAN1} alt="FLAN1" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

이 논문은 [지시 미세조정(instruction finetuning)](https://arxiv.org/pdf/2109.01652.pdf)의 확장 이점과 다양한 모델(PaLM, T5), 프롬프팅 설정(zero-shot, few-shot, CoT), 벤치마크(MMLU, TyDiQA)에서 성능이 어떻게 향상되는지 탐구합니다. 이는 다음 측면으로 탐구됩니다: 작업 수 확장(1.8K 작업), 모델 크기 확장, chain-of-thought 데이터 미세조정(9개 데이터셋 사용).

**미세조정 절차:**
- 1.8K 작업이 지시로 표현되어 모델 미세조정에 사용됨
- 예시가 있는 경우와 없는 경우, CoT가 있는 경우와 없는 경우 모두 사용

미세조정 작업과 보류 작업은 아래와 같습니다:

<Screenshot src={FLAN11} alt="FLAN11" />

## 기능 및 주요 결과(Capabilities & Key Results)

- 지시 미세조정은 작업 수와 모델 크기로 잘 확장됩니다. 이는 작업 수와 모델 크기를 더 확장할 필요가 있음을 시사합니다.
- 미세조정에 CoT 데이터셋을 추가하면 추론 작업에서 좋은 성능을 달성할 수 있습니다.
- Flan-PaLM은 다국어 능력이 향상되었습니다. one-shot TyDiQA에서 14.9% 향상, 대표되지 않은 언어의 산술 추론에서 8.1% 향상
- Plan-PaLM은 또한 개방형 생성 질문에서도 잘 수행하며, 이는 향상된 사용성에 대한 좋은 지표입니다.
- 책임 있는 AI(RAI) 벤치마크 전반에서 성능 향상
- Flan-T5 지시 튜닝 모델은 강력한 few-shot 기능을 보여주며 T5와 같은 공개 체크포인트를 능가합니다

**미세조정 작업 수와 모델 크기 확장 시 결과:** 모델 크기와 미세조정 작업 수를 모두 확장하면 성능이 계속 향상될 것으로 예상되지만, 작업 수 확장은 수익이 감소합니다.

<Screenshot src={FLAN2} alt="FLAN2" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

**비-CoT 및 CoT 데이터로 미세조정 시 결과:** 비-CoT 및 CoT 데이터에 대한 공동 미세조정은 하나 또는 다른 하나만 미세조정하는 것과 비교하여 두 평가 모두에서 성능을 향상시킵니다.

<Screenshot src={FLAN3} alt="FLAN3" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

또한, self-consistency와 CoT를 결합하면 여러 벤치마크에서 SoTA 결과를 달성합니다. CoT + self-consistency는 또한 수학 문제를 포함하는 벤치마크(예: MGSM, GSM8K)에서 결과를 크게 향상시킵니다.

<Screenshot src={FLAN4} alt="FLAN4" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

CoT 미세조정은 BIG-Bench 작업에서 "let's think step-by-step"이라는 구문으로 활성화되는 zero-shot 추론을 해제합니다. 일반적으로 zero-shot CoT Flan-PaLM은 미세조정 없이 zero-shot CoT PaLM을 능가합니다.

<Screenshot src={FLAN6} alt="FLAN6" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

아래는 보이지 않는 작업에서 PaLM과 Flan-PaLM의 zero-shot CoT에 대한 몇 가지 데모입니다.

<Screenshot src={FLAN5} alt="FLAN5" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

아래는 zero-shot 프롬프팅에 대한 더 많은 예시입니다. PaLM 모델이 반복과 지시에 응답하지 않는 것에 어려움을 겪는 반면 Flan-PaLM이 잘 수행할 수 있는 zero-shot 설정에서 어떻게 작동하는지 보여줍니다. Few-shot 예시는 이러한 오류를 완화할 수 있습니다.

<Screenshot src={FLAN7} alt="FLAN7" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

아래는 Flan-PALM 모델의 여러 가지 다른 유형의 도전적인 개방형 질문에 대한 더 많은 zero-shot 기능을 보여주는 몇 가지 예시입니다:

<Screenshot src={FLAN8} alt="FLAN8" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

<Screenshot src={FLAN9} alt="FLAN9" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

<Screenshot src={FLAN10} alt="FLAN10" />
이미지 출처: [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)

[Hugging Face Hub에서 Flan-T5 모델을 시도해볼 수 있습니다](https://huggingface.co/google/flan-t5-xxl).
