# Gemini 1.5 Flash의 컨텍스트 캐싱(Context Caching)

Google은 최근 Gemini 1.5 Pro 및 Gemini 1.5 Flash 모델을 통해 Gemini API에서 사용할 수 있는 [컨텍스트 캐싱(context-caching)](https://ai.google.dev/gemini-api/docs/caching?lang=python)이라는 새로운 기능을 출시했습니다. 이 가이드에서는 Gemini 1.5 Flash에서 컨텍스트 캐싱을 활용하는 기본 예시를 소개합니다.

https://youtu.be/987Pd89EDPs?si=j43isgNb0uwH5AeI

### 활용 사례: 1년치 ML 논문 요약 분석

이 가이드에서는 컨텍스트 캐싱을 활용해 [지난 1년간 정리한 ML 논문 요약](https://github.com/dair-ai/ML-Papers-of-the-Week)을 분석하는 방법을 보여줍니다. 이 요약들은 텍스트 파일로 저장되어 있으며, 이제 Gemini 1.5 Flash 모델에 입력해 효율적으로 질의할 수 있습니다.

### 프로세스: 업로드, 캐싱, 질의

1. **데이터 준비:** 요약이 담긴 readme 파일을 일반 텍스트 파일로 변환합니다.
2. **Gemini API 활용:** Google의 `generativeai` 라이브러리를 사용해 텍스트 파일을 업로드합니다.
3. **컨텍스트 캐싱 구현:** `caching.CachedContent.create()` 함수를 사용해 캐시를 생성합니다. 주요 단계는 다음과 같습니다:
    * Gemini Flash 1.5 모델 지정
    * 캐시 이름 지정
    * 모델에 대한 지시문(instruction) 정의(예: "당신은 AI 연구 전문가입니다...")
    * 캐시의 TTL(time-to-live, 예: 15분) 설정
4. **모델 생성:** 캐시된 콘텐츠를 활용해 생성 모델 인스턴스를 만듭니다.
5. **질의:** 자연어로 다음과 같은 질문을 할 수 있습니다:
    * "이번 주 최신 AI 논문을 알려줄 수 있나요?"
    * "Mamba가 언급된 논문을 모두 나열해 주세요. 논문 제목과 요약을 알려주세요."
    * "롱컨텍스트 LLM 관련 혁신에는 어떤 것들이 있나요? 논문 제목과 요약을 알려주세요."

결과는 매우 긍정적이었습니다. 모델은 텍스트 파일에서 정보를 정확히 검색·요약했으며, 컨텍스트 캐싱 덕분에 매번 전체 텍스트 파일을 전송할 필요 없이 효율적으로 질의할 수 있었습니다.

이 워크플로우는 연구자에게 다음과 같은 이점을 제공합니다:

* 대용량 연구 데이터를 신속하게 분석·질의
* 문서를 일일이 검색하지 않고도 특정 결과를 빠르게 찾기
* 프롬프트 토큰 낭비 없이 대화형 연구 세션 진행

특히, 에이전트 기반 워크플로우 등 더 복잡한 시나리오에서 컨텍스트 캐싱의 추가 활용 가능성에 기대가 큽니다.

노트북 예시는 아래에서 확인할 수 있습니다:

[Context Caching with Gemini APIs](https://github.com/dair-ai/Prompt-Engineering-Guide/blob/main/notebooks/gemini-context-caching.ipynb) 