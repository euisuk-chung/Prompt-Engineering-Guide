# Gemma

Google DeepMind는 Gemini를 만드는 데 사용된 것과 동일한 연구 및 기술에서 영감을 받은 오픈 언어 모델 시리즈인 Gemma를 출시했습니다. Gemma 모델 출시에는 기본 및 지시 튜닝 체크포인트를 포함한 2B(2T 토큰으로 훈련됨) 및 7B(6T 토큰으로 훈련됨) 모델이 포함됩니다. 모델들은 8192 토큰의 컨텍스트 길이로 훈련되었으며 여러 벤치마크에서 Llama 2 7B 및 Mistral 7B 모델을 일반적으로 능가합니다.

Gemma 모델 아키텍처는 [multi-query attention](http://arxiv.org/abs/1911.02150)(2B 모델에서 사용), multi-head attention(7B 모델에서 사용), [RoPE embeddings](https://arxiv.org/abs/2104.09864), [GeGLU activations](https://arxiv.org/abs/2002.05202), [normalizer location](http://arxiv.org/abs/1910.07467)을 포함한 개선사항과 함께 트랜스포머 디코더를 기반으로 합니다.

[기술 보고서](https://storage.googleapis.com/deepmind-media/gemma/gemma-report.pdf)에 따르면, Gemma 2B와 7B는 주로 웹 문서, 수학, 코드로 구성된 2T 및 6T 토큰으로 훈련되었습니다. Gemini와 달리 이 모델들은 다국어 또는 다중모달 기능을 지원하도록 명시적으로 훈련되지 않았습니다. 어휘 크기는 256K 토큰이며 Gemini의 SentencePiece 토크나이저의 하위 집합을 사용하고, 분할된 숫자에서 공백을 보존하며, 알 수 없는 토큰에 대해 바이트 레벨 인코딩에 의존합니다.

지시 튜닝된 모델들은 텍스트 전용 합성 및 인간 생성 프롬프트 응답 쌍의 혼합물에 대한 지도 미세조정과 라벨링된 선호도 데이터로 훈련된 보상 모델과 고품질 프롬프트 세트를 기반으로 한 정책을 사용한 인간 피드백으로부터의 강화학습(RLHF)을 사용하여 튜닝됩니다. 사용된 모든 데이터셋은 영어 전용임을 참고하세요. 아래 표에서 보여주는 것처럼, 지시 튜닝된 모델들은 또한 대화에서 역할과 턴을 나타내기 위해 특정 형식 제어 토큰을 사용합니다.

!["Gemma Control Tokens"](../../img/gemma/control-tokens.png)

## 결과(Results)

아래 그림에서 보여주는 것처럼, Gemma 7B 모델은 수학, 과학, 코드 관련 작업에서 강력한 성능을 보여줍니다. 점수는 기능별로 그룹화된 학술 벤치마크 평가의 평균 점수에 해당합니다.

!["Gemma Capabilities"](../../img/gemma/capabilities.png)

Gemma 7B는 HumanEval, GSM8K, MATH, AGIEval에서 주목할 만한 성능과 추론, 대화, 수학, 코드에서 개선된 성능으로 다양한 학술 벤치마크에서 Llama 2 7B 및 Mistral 7B를 능가합니다.

!["Gemma Safety"](../../img/gemma/benchmarks.png)

Gemma 7B 지시 튜닝 모델들은 또한 인간에 의해 평가된 안전성 및 지시 따르기에서 Mistral-7B v0.2 Instruct 모델을 능가합니다.

!["Gemma Safety"](../../img/gemma/safety.png)

Gemma는 또한 여러 안전 학술 벤치마크에서 평가되며 Mistral과 비교됩니다. 기술 보고서는 또한 편향 제거 기술과 레드팀링의 사용을 언급하여 대규모 언어 모델(LLM)과 관련된 일반적인 위험을 잠재적으로 완화합니다. [모델 카드](https://ai.google.dev/gemma/docs/model_card)와 [Responsible Generative AI toolkit](https://ai.google.dev/responsible)에서 Gemma로 책임감 있게 개발하는 방법에 대한 더 많은 정보를 찾을 수 있습니다.

!["Gemma Safety"](../../img/gemma/safety-2.png)

## Gemma 7B 프롬프트 형식(Gemma 7B Prompt Format)

Gemma 기본 모델들은 특정 프롬프트 형식을 사용하지 않지만 zero-shot/few-shot 프롬프팅을 통해 작업을 수행하도록 프롬프트할 수 있습니다. Gemma Instruct 모델은 다음 형식을 사용합니다:

```
<start_of_turn>user
Generate a Python function that multiplies two numbers <end_of_turn>
<start_of_turn>model
```

다음은 Gemma에서 사용 가능한 관련 형식 제어 토큰을 보여주는 표입니다:

| 컨텍스트(Context)                    | 관련 토큰(Relevant Token) |
|-------------------------------------|-------------------------|
| 사용자 턴(User turn)                | `user`                 |
| 모델 턴(Model turn)                 | `model`                |
| 대화 턴 시작(Start of conversation turn) | `<start_of_turn>`      |
| 대화 턴 끝(End of conversation turn)   | `<end_of_turn>`        |

다음과 같이 다중 턴 사용자 프롬프트의 컨텍스트에서 특수 제어 토큰을 사용할 수도 있습니다:

```markdown
<start_of_turn>user
What is a good place for travel in the US?<end_of_turn>
<start_of_turn>model
California.<end_of_turn>
<start_of_turn>user
What can I do in California?<end_of_turn>
<start_of_turn>model
```

## Gemma 7B 프롬프팅 방법(How to Prompt Gemma 7B)
Gemma 7B를 효과적으로 프롬프팅하려면 프롬프트 템플릿을 올바르게 사용할 수 있어야 합니다. 다음 예시에서는 다양한 작업을 위한 Gemma 7B Instruct의 프롬프트 템플릿을 효과적으로 사용하는 방법을 시연하는 몇 가지 예시를 다룰 것입니다.

### Zero-shot 프롬프팅(Zero-shot Prompting)
다른 모델과 마찬가지로 다음과 같이 간단히 프롬프트하여 Gemma의 zero-shot 기능을 활용할 수 있습니다:

```markdown
<start_of_turn>user
Explain why the sky is blue<end_of_turn>
<start_of_turn>model
```

### 시스템 프롬프트가 있는 Zero-shot 프롬프팅(Zero-shot Prompting with System Prompt)
시스템 역할이나 시스템 프롬프트를 추가하면 LLM을 더 잘 조종하는 데 도움이 됩니다. Gemma에는 명시적인 시스템 역할이 없지만 다음과 같이 프롬프트의 일부로 추가 지시사항을 추가할 수 있습니다:

```markdown
<start_of_turn>user
Answer the following question in a concise and informative manner:

Explain why the sky is blue<end_of_turn>
<start_of_turn>model
```

위 예시에서 모델을 더 잘 조종하기 위해 추가 지시사항이나 시스템 프롬프트로 `"Answer the following question in a concise and informative manner:"`를 추가했습니다.

### 역할극(Role Playing)
Gemma의 역할극 기능을 테스트할 수도 있으며, 이는 모델 응답을 개인화할 뿐만 아니라 더 복잡한 도메인 특정 [LLM 에이전트](https://www.promptingguide.ai/research/llm-agents)를 구축하는 데 유용한 능력입니다.

프롬프트:
```
<start_of_turn>user
You are a helpful 2nd-grade teacher. Help a 2nd grader to answer questions in a short and clear manner.

Explain why the sky is blue<end_of_turn>
<start_of_turn>model
```

모델 응답:
```
Sure, here is an explanation for why the sky is blue:

The sky appears blue because of a phenomenon called Rayleigh Scattering.

Here's the science behind it:

Sunlight is made up of different colors of light, like red, green, blue, and violet.
When sunlight hits the Earth's atmosphere, the different colors of light scatter in different directions.
Blue light scatters more easily than other colors because it has shorter wavelengths.
The scattered blue light travels in all directions, including towards your eyes.
That's why we see the sky as blue.
It's important to note that this explanation is simplified and does not account for all scientific details. However, it gives you a good understanding of the basic science behind why the sky appears blue.
```

응답의 정확성이나 2학년 학생에게 적합한지 확인하지 않았음을 참고하세요. 그러나 위의 응답을 다음에 나오는 chain-of-thought 프롬프트에서 얻은 응답과 비교할 수 있습니다.

### 추론(Reasoning)
다음과 같이 [zero-shot chain-of-thought 프롬프팅](https://www.promptingguide.ai/techniques/cot#zero-shot-cot-prompting)을 사용하여 Gemma의 추론 기능을 이끌어낼 수 있습니다:

프롬프트:
```markdown
<start_of_turn>user
Think and write your step-by-step reasoning before responding.

Explain why the sky is blue.<end_of_turn>
<start_of_turn>model
```

모델 응답(간결함을 위해 잘림):
```
Step 1: Scattering of Light

The sky appears blue due to the phenomenon of scattering of light. When sunlight hits the Earth's atmosphere, it interacts with the particles of air, primarily nitrogen and oxygen molecules.

Step 2: Rayleigh Scattering

Rayleigh scattering is the specific scattering mechanism that occurs when light interacts with particles much smaller than the wavelength of light. In this process, shorter wavelengths (blue light) are scattered more effectively than longer wavelengths (red light).

Step 3: Blue Scattered Light Travels in All Directions

...

Conclusion

The sky appears blue due to the scattering of light, specifically Rayleigh scattering. Shorter wavelengths of light are scattered more effectively, and this scattered light travels in all directions, with the scattered light that travels opposite to the sun's rays being most visible to our eyes.
```

Gemma 모델에 대해 주목할 점은 마크다운 형식을 자동으로 출력할 수 있다는 것입니다. 시연과 간결함을 위해 텍스트 형식을 약간 편집했지만 내용은 모델이 응답한 것과 정확히 동일합니다. 또한 응답의 정확성이나 모델이 환각을 일으키는지 평가하지 않았음을 참고하세요.

## 리소스 및 통합(Resources and Integrations)

다음은 Gemma 출시의 일부였던 여러 리소스 및 통합입니다:

- [Colab](https://ai.google.dev/gemma/docs/get_started) 및 [Kaggle](https://www.kaggle.com/models/google/gemma/code) 노트북
- [Hugging Face 모델들](https://huggingface.co/collections/google/gemma-release-65d5efbccdbb8c4202ec078b)
- [MaxText](https://github.com/google/maxtext)
- [NVIDIA NeMo](https://github.com/NVIDIA/GenerativeAIExamples/tree/main/models/Gemma)
- [TensorRT-LLM](https://developer.nvidia.com/blog/nvidia-tensorrt-llm-revs-up-inference-for-google-gemma/)
- Gemma 7B는 [NVIDIA AI Playground](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/ai-foundation/models/gemma-7b)에서 사용 가능합니다

공식 [블로그 출시](https://blog.google/technology/developers/gemma-open-models/)에 따르면, [이용약관](https://www.kaggle.com/models/google/gemma/license/consent)은 규모에 관계없이 모든 조직의 책임감 있는 상업적 사용 및 배포를 허용합니다.

## 참고문헌(References)

- [Gemma: Introducing new state-of-the-art open models](https://blog.google/technology/developers/gemma-open-models/)
- [Gemma: Open Models Based on Gemini Research and Technology](https://storage.googleapis.com/deepmind-media/gemma/gemma-report.pdf)
- [Responsible Generative AI Toolkit](https://ai.google.dev/responsible)
- [Fast Transformer Decoding: One Write-Head is All You Need](https://arxiv.org/abs/1911.02150)
- [Roformer: Enhanced transformer with rotary position embedding](https://arxiv.org/abs/2104.09864)
- [GLU variants improve transformer](https://arxiv.org/abs/2002.05202)
- [Root mean square layer normalization](http://arxiv.org/abs/1910.07467) 