# Gemini 1.5 Flash를 활용한 컨텍스트 캐싱(Context Caching with Gemini 1.5 Flash)

Google은 최근 Gemini 1.5 Pro와 Gemini 1.5 Flash 모델을 통해 Gemini API에서 사용할 수 있는 [컨텍스트 캐싱](https://ai.google.dev/gemini-api/docs/caching?lang=python)이라는 새로운 기능을 출시했습니다. 이 가이드는 Gemini 1.5 Flash를 사용한 컨텍스트 캐싱의 기본 예시를 제공합니다.

### 사용 사례: 1년치 ML 논문 분석(The Use Case: Analyzing a Year's Worth of ML Papers)

이 가이드는 컨텍스트 캐싱을 사용하여 [지난 1년간 문서화한 모든 ML 논문](https://github.com/dair-ai/ML-Papers-of-the-Week)의 요약을 분석하는 방법을 보여줍니다. 우리는 이러한 요약을 텍스트 파일에 저장하며, 이제 Gemini 1.5 Flash 모델에 공급하고 효율적으로 쿼리할 수 있습니다.

### 프로세스: 업로드, 캐싱, 쿼리(The Process: Uploading, Caching, and Querying)

1. **데이터 준비**: 먼저 요약이 포함된 readme 파일을 일반 텍스트 파일로 변환합니다.
2. **Gemini API 활용**: Google `generativeai` 라이브러리를 사용하여 텍스트 파일을 업로드할 수 있습니다.
3. **컨텍스트 캐싱 구현**: `caching.CachedContent.create()` 함수를 사용하여 캐시가 생성됩니다. 여기에는 다음이 포함됩니다:
    * Gemini Flash 1.5 모델 지정
    * 캐시에 대한 이름 제공
    * 모델에 대한 지시사항 정의(예: "당신은 전문 AI 연구원입니다...")
    * 캐시의 TTL(Time-to-Live) 설정(예: 15분)
4. **모델 생성**: 그런 다음 캐시된 콘텐츠를 사용하여 생성 모델 인스턴스를 생성합니다.
5. **쿼리**: 다음과 같은 자연어 질문으로 모델에 쿼리를 시작할 수 있습니다:
    * "최신 AI 주간 논문을 알려주실 수 있나요?"
    * "Mamba를 언급하는 논문들을 나열해주세요. 논문 제목과 요약을 나열하세요."
    * "긴 컨텍스트 LLM 주변의 혁신은 무엇인가요? 논문 제목과 요약을 나열하세요."

결과는 유망했습니다. 모델은 텍스트 파일에서 정보를 정확하게 검색하고 요약했습니다. 컨텍스트 캐싱은 매우 효율적이어서 각 쿼리마다 전체 텍스트 파일을 반복적으로 보낼 필요가 없었습니다.

이 워크플로우는 연구자들에게 다음과 같은 가치 있는 도구가 될 잠재력이 있습니다:

* 대량의 연구 데이터를 빠르게 분석하고 쿼리할 수 있습니다.
* 문서를 수동으로 검색하지 않고도 특정 발견사항을 검색할 수 있습니다.
* 프롬프트 토큰을 낭비하지 않고 대화형 연구 세션을 수행할 수 있습니다.

우리는 에이전트 워크플로우와 같은 더 복잡한 시나리오 내에서 특히 컨텍스트 캐싱의 추가 응용 프로그램을 탐색하는 것을 기대합니다.

노트북은 아래에서 찾을 수 있습니다:

**Gemini API를 활용한 컨텍스트 캐싱**
[노트북 보기](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/notebooks/gemini-context-caching.ipynb)

🎓 **정보**: 새로운 AI 과정에서 캐싱 방법에 대해 자세히 알아보세요. [지금 참여하세요!](https://dair-ai.thinkific.com/)
PROMPTING20 코드를 사용하면 추가 20% 할인을 받을 수 있습니다. 