## LLaMA: 오픈 및 효율적인 기초 언어 모델(Open and Efficient Foundation Language Models)

<Callout emoji="⚠️">
  이 섹션은 활발한 개발 중입니다.
</Callout>

## 새로운 점은 무엇인가요?(What's new?)

이 논문은 7B에서 65B 파라미터에 이르는 기초 언어 모델 컬렉션을 소개합니다.

모델들은 공개적으로 사용 가능한 데이터셋으로 수조 개의 토큰에 대해 훈련되었습니다.

[(Hoffman et al. 2022)](https://arxiv.org/abs/2203.15556)의 연구는 주어진 컴퓨팅 예산에서 훨씬 더 많은 데이터로 훈련된 더 작은 모델들이 더 큰 모델들보다 더 나은 성능을 달성할 수 있음을 보여줍니다. 이 연구는 200B 토큰으로 10B 모델을 훈련하는 것을 권장합니다. 그러나 LLaMA 논문은 7B 모델의 성능이 1T 토큰 이후에도 계속 개선됨을 발견했습니다.

<Screenshot src={LLAMA1} alt="LLAMA1" />

이 연구는 더 많은 토큰으로 훈련하여 다양한 추론 예산에서 최고의 성능을 달성하는 모델(LLaMA) 훈련에 중점을 둡니다.

## 기능 및 주요 결과(Capabilities & Key Results)

전반적으로, LLaMA-13B는 10배 작고 단일 GPU에서 실행 가능함에도 불구하고 많은 벤치마크에서 GPT-3(175B)를 능가합니다. LLaMA 65B는 Chinchilla-70B 및 PaLM-540B와 같은 모델들과 경쟁력이 있습니다.

*논문:* [LLaMA: Open and Efficient Foundation Language Models](https://arxiv.org/abs/2302.13971)

*코드:* https://github.com/facebookresearch/llama

## 참고문헌(References)

- [Koala: A Dialogue Model for Academic Research](https://bair.berkeley.edu/blog/2023/04/03/koala/) (April 2023)
- [Baize: An Open-Source Chat Model with Parameter-Efficient Tuning on Self-Chat Data](https://arxiv.org/abs/2304.01196) (April 2023)
- [Vicuna: An Open-Source Chatbot Impressing GPT-4 with 90%* ChatGPT Quality](https://vicuna.lmsys.org/) (March 2023)
- [LLaMA-Adapter: Efficient Fine-tuning of Language Models with Zero-init Attention](https://arxiv.org/abs/2303.16199) (March 2023)
- [GPT4All](https://github.com/nomic-ai/gpt4all) (March 2023)
- [ChatDoctor: A Medical Chat Model Fine-tuned on LLaMA Model using Medical Domain Knowledge](https://arxiv.org/abs/2303.14070) (March 2023)
- [Stanford Alpaca](https://github.com/tatsu-lab/stanford_alpaca) (March 2023)