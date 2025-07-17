# Groq란 무엇인가?

[Groq](https://groq.com/)는 최근 가장 빠른 LLM(대형 언어 모델, Large Language Model) 추론 솔루션 중 하나로 주목받고 있습니다. LLM 실무자들은 LLM 응답의 지연(latency)을 줄이는 데 큰 관심을 가지고 있습니다. 지연 시간은 실시간 AI 애플리케이션을 구현하기 위해 반드시 최적화해야 할 중요한 지표입니다. 현재 LLM 추론 분야에는 여러 기업이 경쟁하고 있습니다.

Groq는 이러한 LLM 추론 기업 중 하나로, 이 글 작성 시점 기준 [Anyscale의 LLMPerf Leaderboard](https://github.com/ray-project/llmperf-leaderboard)에서 다른 주요 클라우드 기반 제공업체 대비 18배 빠른 추론 성능을 주장합니다. Groq는 현재 Meta AI의 Llama 2 70B, Mixtral 8x7B 등 다양한 모델을 API로 제공합니다. 이 모델들은 Groq의 자체 하드웨어인 LPU™(Language Processing Unit) 기반의 Groq LPU™ 추론 엔진으로 구동됩니다. LPU는 LLM 실행을 위해 설계된 맞춤형 하드웨어입니다.

Groq의 FAQ에 따르면, LPU는 단어별 계산 시간을 줄여 텍스트 시퀀스 생성 속도를 크게 높입니다. LPU의 기술적 세부사항과 장점은 ISCA에서 수상한 [2020년 논문](https://wow.groq.com/groq-isca-paper-2020/)과 [2022년 논문](https://wow.groq.com/isca-2022-paper/)에서 자세히 확인할 수 있습니다.

아래는 Groq가 제공하는 모델의 속도 및 가격 비교 차트입니다:

![](../../img/research/groq.png)

아래 차트는 출력 토큰 처리량(tokens/s, 초당 토큰 수)을 비교합니다. 이는 초당 반환되는 평균 출력 토큰 수를 의미합니다. 차트의 수치는 Llama 2 70B 모델 기준, LLM 추론 제공업체별 150회 요청의 평균 출력 토큰 처리량입니다.

![](https://github.com/ray-project/llmperf-leaderboard/blob/main/.assets/output_tokens_per_s.jpg?raw=true)

LLM 추론에서, 특히 스트리밍 애플리케이션에서는 첫 토큰 생성까지 걸리는 시간(TTFT, Time To First Token)도 매우 중요합니다. 아래는 주요 LLM 추론 제공업체별 TTFT(초) 비교 차트입니다:

![](https://github.com/ray-project/llmperf-leaderboard/blob/main/.assets/ttft.jpg?raw=true)

Groq의 LLM 추론 성능에 대한 자세한 내용은 Anyscale의 LLMPerf Leaderboard [여기](https://wow.groq.com/groq-lpu-inference-engine-crushes-first-public-llm-benchmark/)에서 확인할 수 있습니다. 