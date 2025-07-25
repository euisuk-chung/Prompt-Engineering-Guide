# Claude 3

Anthropic은 Claude 3라는 새로운 모델 패밀리를 발표했습니다. 이 패밀리에는 Claude 3 Haiku, Claude 3 Sonnet, Claude 3 Opus가 포함됩니다.

Claude 3 Opus(가장 강력한 모델)는 MMLU, HumanEval과 같은 일반 벤치마크에서 GPT-4 및 모든 다른 모델을 능가하는 것으로 보고되었습니다.

## 결과 및 기능(Results and Capabilities)

Claude 3의 기능에는 고급 추론, 기초 수학, 분석, 데이터 추출, 예측, 콘텐츠 생성, 코드 생성, 스페인어, 일본어, 프랑스어 등 비영어권 언어 변환이 포함됩니다. 아래 표는 여러 벤치마크에서 Claude 3가 다른 모델과 어떻게 비교되는지 보여주며, Claude 3 Opus가 언급된 모든 모델을 능가합니다:

!['Claude 3 Benchmarks'](../../img/claude/claude-benchmark.png)

Claude 3 Haiku는 시리즈 중 가장 빠르고 비용 효율적인 모델입니다. Claude 3 Sonnet은 이전 Claude 버전보다 2배 빠르며, Opus는 Claude 2.1과 동일한 속도이면서 더 뛰어난 기능을 제공합니다.

Claude 3 모델은 200K 컨텍스트 윈도우를 지원하며, 일부 고객에게는 1M 토큰까지 확장할 수 있습니다. Claude 3 Opus는 대용량 코퍼스에서 정보를 회수하고 긴 컨텍스트 프롬프트를 효과적으로 처리하는 능력을 측정하는 Needle In A Haystack(NIAH) 평가에서 거의 완벽한 회수율을 달성했습니다.

이 모델들은 사진, 차트, 그래프와 같은 포맷을 처리하는 강력한 비전(시각) 기능도 갖추고 있습니다.

!['Claude 3 Vision Capabilities'](../../img/claude/claude-vision.png)

Anthropic은 또한 이 모델들이 요청을 더 미묘하게 이해하고 거부 횟수가 적다고 주장합니다. Opus는 개방형 질문에서 사실적 질문 응답에서 상당한 개선을 보이며, 잘못된 답변이나 환각(hallucination)을 줄입니다. Claude 3 모델은 Claude 2 모델보다 JSON 객체와 같은 구조화된 출력을 생성하는 데도 더 뛰어납니다.

## 참고문헌(References)

- [Claude 3 Haiku, Claude 3 Sonnet, and Claude 3 Opus](https://www.anthropic.com/news/claude-3-family)
- [The Claude 3 Model Family: Opus, Sonnet, Haiku](https://www-cdn.anthropic.com/de8ba9b01c9ab7cbabf5c33b80b7bbc618857627/Model_Card_Claude_3.pdf) 