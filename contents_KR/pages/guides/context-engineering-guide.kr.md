# **컨텍스트 엔지니어링 가이드(Context Engineering Guide)**

## **목차(Table of Contents)**

* [컨텍스트 엔지니어링이란 무엇인가요?](#what-is-context-engineering)  
* [실제 컨텍스트 엔지니어링](#context-engineering-in-action)  
  * [시스템 프롬프트](#system-prompt)  
  * [지침](#instructions)  
  * [사용자 입력](#user-input)  
  * [구조화된 입력과 출력](#structured-inputs-and-outputs)  
  * [도구](#tools)  
  * [RAG & 메모리](#rag--memory)  
  * [상태 & 역사적 컨텍스트](#states--historical-context)  
* [고급 컨텍스트 엔지니어링](#advanced-context-engineering-wip)  
* [참고 자료](#resources)

## **컨텍스트 엔지니어링이란 무엇인가요?**

몇 년 전, 많은 사람들, 심지어 최고의 AI 연구자들까지 프롬프트 엔지니어링이 지금쯤이면 사라질 것이라고 주장했습니다.

분명히, 그들은 매우 잘못되었고, 실제로는 프롬프트 엔지니어링이 이제 그 어느 때보다 더 중요해졌습니다. 너무나 중요해서 이제 ***컨텍스트 엔지니어링***으로 재브랜딩되고 있습니다.

네, LLM이 작업을 효과적으로 수행하기 위해 필요한 지침과 관련 컨텍스트를 조정하는 중요한 과정을 설명하는 또 다른 멋진 용어입니다.

컨텍스트 엔지니어링에 대해서는 이미 많은 글이 작성되었습니다([Ankur Goyal](https://x.com/ankrgyl/status/1913766591910842619), [Walden Yan](https://cognition.ai/blog/dont-build-multi-agents), [Tobi Lutke](https://x.com/tobi/status/1935533422589399127), [Andrej Karpathy](https://x.com/karpathy/status/1937902205765607626)), 하지만 저는 이 주제에 대한 제 생각을 쓰고 AI 에이전트 워크플로우 개발에서 컨텍스트 엔지니어링을 실제로 적용하는 구체적인 단계별 가이드를 보여드리고 싶습니다.

컨텍스트 엔지니어링을 처음 만든 사람이 누구인지 확실하지 않지만, 컨텍스트 엔지니어링이 무엇인지 간단히 설명하는 [Dex Horthy](https://x.com/dexhorthy/status/1933283008863482067)의 이 그림을 기반으로 설명하겠습니다.

![컨텍스트 엔지니어링의 중첩된 측면을 보여주는 다이어그램](../../img/context-engineering-guide/context-engineering-diagram.jpg)

저는 컨텍스트 엔지니어링이라는 용어가 마음에 듭니다. 프롬프트 엔지니어링에 들어가는 대부분의 작업을 더 잘 설명하는 더 넓은 용어로 느껴지기 때문입니다. 여기에는 다른 관련 작업들도 포함됩니다.

프롬프트 엔지니어링이 진지한 기술이라는 것에 대한 의심은 많은 사람들이 이를 맹목적 프롬프팅(blind prompting, ChatGPT 같은 LLM에서 사용하는 짧은 작업 설명)과 혼동하기 때문입니다. 맹목적 프롬프팅에서는 단순히 시스템에 질문을 하는 것입니다. 프롬프트 엔지니어링에서는 프롬프트의 컨텍스트와 구조에 대해 더 신중하게 생각해야 합니다. 아마도 처음부터 컨텍스트 엔지니어링이라고 불렸어야 했을 것 같습니다.

컨텍스트 엔지니어링은 다음 단계로, 전체 컨텍스트를 설계하는 단계입니다. 이는 많은 경우 단순한 프롬프팅을 넘어서 시스템을 위한 지식을 획득하고, 향상시키고, 최적화하는 더 엄격한 방법으로 들어가는 것을 요구합니다.

개발자의 관점에서, 컨텍스트 엔지니어링은 원하는 결과를 달성하기 위해 LLM에 제공하는 지침과 컨텍스트를 최적화하는 반복적인 과정을 포함합니다. 여기에는 전술이 작동하는지 측정하기 위한 공식적인 프로세스(예: 평가 파이프라인)를 갖는 것도 포함됩니다.

AI 분야의 빠른 발전을 고려할 때, 저는 컨텍스트 엔지니어링의 더 넓은 정의를 제안합니다: ***LLM과 고급 AI 모델이 작업을 효과적으로 수행하기 위한 지침과 관련 컨텍스트를 설계하고 최적화하는 과정.*** 이는 텍스트 기반 LLM뿐만 아니라 점점 더 널리 사용되고 있는 멀티모달 모델의 컨텍스트 최적화도 포함합니다. 여기에는 모든 프롬프트 엔지니어링 노력과 다음과 같은 관련 프로세스들이 포함될 수 있습니다:

* 프롬프트 체인 설계 및 관리 (해당하는 경우)  
* 지침/시스템 프롬프트 조정  
* 프롬프트의 동적 요소 관리 (예: 사용자 입력, 날짜/시간 등)  
* 관련 지식 검색 및 준비 (즉, RAG)  
* 쿼리 증강  
* 도구 정의 및 지침 (에이전트 시스템의 경우)  
* few-shot 데모 준비 및 최적화  
* 입력과 출력 구조화 (예: 구분자, JSON 스키마)  
* 단기 메모리 (즉, 상태/역사적 컨텍스트 관리) 및 장기 메모리 (예: 벡터 스토어에서 관련 지식 검색)  
* 그리고 원하는 작업을 달성하기 위해 LLM 시스템 프롬프트를 최적화하는 데 유용한 기타 많은 트릭들

다시 말해, 컨텍스트 엔지니어링에서 달성하려는 것은 LLM의 컨텍스트 윈도우에서 제공하는 정보를 최적화하는 것입니다. 이는 또한 노이즈가 많은 정보를 필터링하는 것을 의미하며, 이는 자체적으로 과학입니다. LLM의 성능을 체계적으로 측정하는 것을 요구하기 때문입니다.

모든 사람이 컨텍스트 엔지니어링에 대해 글을 쓰고 있지만, 여기서는 AI 에이전트를 구축할 때 컨텍스트 엔지니어링이 어떻게 보이는지 구체적인 예시를 통해 안내해드리겠습니다.

## **실제 컨텍스트 엔지니어링**

개인용으로 구축한 멀티 에이전트 딥 리서치 애플리케이션을 위한 최근 컨텍스트 엔지니어링 작업의 구체적인 예시를 살펴보겠습니다.

저는 n8n 내부에 에이전트 워크플로우를 구축했지만, 도구는 중요하지 않습니다. 제가 구축한 완전한 에이전트 아키텍처는 다음과 같습니다:

![멀티 에이전트 딥 리서치 애플리케이션을 보여주는 n8n 워크플로우 이미지](../../img/context-engineering-guide/context-engineering-workflow.jpg)

제 워크플로우의 검색 계획자(Search Planner) 에이전트는 사용자 쿼리를 기반으로 검색 계획을 생성하는 역할을 담당합니다.

### **시스템 프롬프트**

아래는 이 서브에이전트를 위해 제가 구성한 시스템 프롬프트입니다:

```
You are an expert research planner. Your task is to break down a complex research query (delimited by <user_query></user_query>) into specific search subtasks, each focusing on a different aspect or source type.
        
The current date and time is: {{ $now.toISO() }}

For each subtask, provide:
1. A unique string ID for the subtask (e.g., 'subtask_1', 'news_update')
2. A specific search query that focuses on one aspect of the main query
3. The source type to search (web, news, academic, specialized)
4. Time period relevance (today, last week, recent, past_year, all_time)
5. Domain focus if applicable (technology, science, health, etc.)
6. Priority level (1-highest to 5-lowest)
        
All fields (id, query, source_type, time_period, domain_focus, priority) are required for each subtask, except time_period and domain_focus which can be null if not applicable.
        
Create 2 subtasks that together will provide comprehensive coverage of the topic. Focus on different aspects, perspectives, or sources of information.

Each substask will include the following information:

id: str
query: str
source_type: str  # e.g., "web", "news", "academic", "specialized"
time_period: Optional[str] = None  # e.g., "today", "last week", "recent", "past_year", "all_time"
domain_focus: Optional[str] = None  # e.g., "technology", "science", "health"
priority: int  # 1 (highest) to 5 (lowest)

After obtaining the above subtasks information, you will add two extra fields. Those correspond to start_date and end_date. Infer this information given the current date and the time_period selected. start_date and end_date should use the format as in the example below:

"start_date": "2024-06-03T06:00:00.000Z",
"end_date": "2024-06-11T05:59:59.999Z",
```

이 프롬프트에는 계획 에이전트가 작업을 효과적으로 수행하기 위해 제공하는 정확한 컨텍스트에 대해 신중하게 고려해야 하는 많은 부분들이 있습니다. 보시다시피, 이는 단순한 프롬프트나 지침을 설계하는 것이 아니라, 모델이 작업을 최적으로 수행하기 위한 중요한 컨텍스트를 제공하는 실험과 과정을 요구합니다.

문제를 효과적인 컨텍스트 엔지니어링의 핵심 구성 요소로 분해해보겠습니다.

### **지침**

지침은 시스템에 정확히 무엇을 해야 하는지 알려주는 고수준 지침입니다.

```
You are an expert research planner. Your task is to break down a complex research query (delimited by <user_query></user_query>) into specific search subtasks, each focusing on a different aspect or source type.
```

많은 초보자와 심지어 경험 있는 AI 개발자들도 여기서 멈출 것입니다. 위에서 전체 프롬프트를 공유했으므로, 시스템이 원하는 대로 작동하기 위해 얼마나 더 많은 컨텍스트를 제공해야 하는지 이해할 수 있을 것입니다. 이것이 바로 컨텍스트 엔지니어링의 핵심입니다. 시스템에게 문제 범위와 우리가 정확히 원하는 것에 대해 더 많은 정보를 제공합니다.

### **사용자 입력**

사용자 입력은 시스템 프롬프트에 표시되지 않았지만, 아래는 어떻게 보일지에 대한 예시입니다.

```
<user_query> What's the latest dev news from OpenAI? </user_query>
```

구분자(delimiter) 사용에 주목하세요. 이는 프롬프트를 더 잘 구조화하는 것입니다. 혼란을 피하고 사용자 입력이 무엇이고 시스템이 생성하기를 원하는 것이 무엇인지 명확하게 하는 것이 중요합니다. 때로는 우리가 입력하는 정보의 유형이 모델이 출력하기를 원하는 것과 관련이 있습니다(예: 쿼리가 입력이고, 서브쿼리가 출력).

### **구조화된 입력과 출력**

고수준 지침과 사용자 입력 외에도, 계획 에이전트가 생성해야 하는 서브태스크와 관련된 세부사항에 상당한 노력을 기울인 것을 눈치챘을 것입니다. 아래는 사용자 쿼리가 주어졌을 때 서브태스크를 생성하기 위해 계획 에이전트에게 제공한 상세한 지침입니다.

```
For each subtask, provide:
1. A unique string ID for the subtask (e.g., 'subtask_1', 'news_update')
2. A specific search query that focuses on one aspect of the main query
3. The source type to search (web, news, academic, specialized)
4. Time period relevance (today, last week, recent, past_year, all_time)
5. Domain focus if applicable (technology, science, health, etc.)
6. Priority level (1-highest to 5-lowest)
        
All fields (id, query, source_type, time_period, domain_focus, priority) are required for each subtask, except time_period and domain_focus which can be null if not applicable.
        
Create 2 subtasks that together will provide comprehensive coverage of the topic. Focus on different aspects, perspectives, or sources of information.
```

위의 지침을 자세히 보면, 계획 에이전트가 생성하기를 원하는 필수 정보 목록을 구조화하고, 데이터 생성 과정을 더 잘 안내하기 위한 힌트/예시를 제공하기로 결정했습니다. 이는 에이전트에게 기대하는 것에 대한 추가 컨텍스트를 제공하는 것이 중요합니다. 예를 들어, 우선순위 수준을 1-5 척도로 원한다고 말하지 않으면, 시스템은 1-10 척도를 선호할 수 있습니다. 다시 한번, 이 컨텍스트가 매우 중요합니다!

다음으로, 구조화된 출력에 대해 이야기해보겠습니다. 계획 에이전트로부터 일관된 출력을 얻기 위해, 우리가 기대하는 서브태스크 형식과 필드 유형에 대한 컨텍스트도 제공하고 있습니다. 아래는 에이전트에게 추가 컨텍스트로 전달하는 예시입니다. 이는 에이전트에게 출력으로 기대하는 것에 대한 힌트와 단서를 제공할 것입니다:

```
Each substask will include the following information:

id: str
query: str
source_type: str  # e.g., "web", "news", "academic", "specialized"
time_period: Optional[str] = None  # e.g., "today", "last week", "recent", "past_year", "all_time"
domain_focus: Optional[str] = None  # e.g., "technology", "science", "health"
priority: int  # 1 (highest) to 5 (lowest)
```

이에 더해, n8n 내부에서는 도구 출력 파서를 사용할 수도 있습니다. 이는 본질적으로 최종 출력을 구조화하는 데 사용될 것입니다. 제가 사용하고 있는 옵션은 다음과 같은 JSON 예시를 제공하는 것입니다:

```
{
  "subtasks": [
    {
      "id": "openai_latest_news",
      "query": "latest OpenAI announcements and news",
      "source_type": "news",
      "time_period": "recent",
      "domain_focus": "technology",
      "priority": 1,
      "start_date": "2025-06-03T06:00:00.000Z",
      "end_date": "2025-06-11T05:59:59.999Z"
    },
    {
      "id": "openai_official_blog",
      "query": "OpenAI official blog recent posts",
      "source_type": "web",
      "time_period": "recent",
      "domain_focus": "technology",
      "priority": 2,
      "start_date": "2025-06-03T06:00:00.000Z",
      "end_date": "2025-06-11T05:59:59.999Z"
    },
...
}
```

그러면 도구가 이러한 예시에서 자동으로 스키마를 생성하고, 이는 시스템이 적절한 구조화된 출력을 파싱하고 생성할 수 있게 합니다. 아래 예시와 같이:

```
[
  {
    "action": "parse",
    "response": {
      "output": {
        "subtasks": [
          {
            "id": "subtask_1",
            "query": "OpenAI recent announcements OR news OR updates",
            "source_type": "news",
            "time_period": "recent",
            "domain_focus": "technology",
            "priority": 1,
            "start_date": "2025-06-24T16:35:26.901Z",
            "end_date": "2025-07-01T16:35:26.901Z"
          },
          {
            "id": "subtask_2",
            "query": "OpenAI official blog OR press releases",
            "source_type": "web",
            "time_period": "recent",
            "domain_focus": "technology",
            "priority": 1.2,
            "start_date": "2025-06-24T16:35:26.901Z",
            "end_date": "2025-07-01T16:35:26.901Z"
          }
        ]
      }
    }
  }
]
```

이런 것들이 복잡해 보이지만, 오늘날 많은 도구들이 구조화된 출력 기능을 기본적으로 제공하므로, 직접 구현할 필요가 없을 가능성이 높습니다. n8n은 컨텍스트 엔지니어링의 이 부분을 매우 쉽게 만듭니다. 이것은 제가 많은 AI 개발자들이 어떤 이유로 무시하는 컨텍스트 엔지니어링의 과소평가된 측면 중 하나입니다. 희망적으로, 컨텍스트 엔지니어링이 이러한 중요한 기술들에 더 많은 빛을 비춰줄 것입니다. 이는 특히 에이전트가 워크플로우의 다음 구성 요소에 특별한 형식으로 전달되어야 하는 일관되지 않은 출력을 받고 있을 때 매우 강력한 접근 방식입니다.

### **도구**

우리는 에이전트를 구축하기 위해 n8n을 사용하고 있으므로, 현재 날짜와 시간을 컨텍스트에 쉽게 넣을 수 있습니다. 다음과 같이 할 수 있습니다:

```
The current date and time is: {{ $now.toISO() }}
```

이는 n8n에서 호출되는 간단하고 편리한 함수이지만, 일반적으로 이것을 전용 도구로 구축하여 더 동적으로 만들 수 있습니다(즉, 쿼리가 필요로 할 때만 날짜와 시간을 가져옴). 이것이 바로 컨텍스트 엔지니어링의 핵심입니다. 구축자로서 LLM에 어떤 컨텍스트를 언제 전달할지에 대해 구체적인 결정을 내리도록 강제합니다. 이는 애플리케이션에서 가정과 부정확성을 제거하기 때문에 훌륭합니다.

날짜와 시간은 시스템의 중요한 컨텍스트입니다. 그렇지 않으면 현재 날짜와 시간에 대한 지식을 요구하는 쿼리에서 잘 수행하지 않는 경향이 있습니다. 예를 들어, 지난 주에 발생한 OpenAI의 최신 개발 뉴스를 검색하라고 시스템에 요청하면, 단순히 날짜와 시간을 추측할 것이고, 이는 차선의 쿼리와 결과적으로 부정확한 웹 검색으로 이어질 것입니다. 시스템이 올바른 날짜와 시간을 가지고 있을 때, 검색 에이전트와 도구에 중요한 날짜 범위를 더 잘 추론할 수 있습니다. LLM이 날짜 범위를 생성할 수 있도록 컨텍스트의 일부로 이것을 추가했습니다:

```
After obtaining the above subtasks information, you will add two extra fields. Those correspond to start_date and end_date. Infer this information given the current date and the time_period selected. start_date and end_date should use the format as in the example below:

"start_date": "2024-06-03T06:00:00.000Z",
"end_date": "2024-06-11T05:59:59.999Z",
```

우리는 아키텍처의 계획 에이전트에 집중하고 있으므로, 여기에 추가해야 할 도구가 많지 않습니다. 추가하는 것이 합리적인 유일한 다른 도구는 쿼리가 주어졌을 때 관련 서브태스크를 검색하는 검색 도구입니다. 이 아이디어에 대해 아래에서 논의해보겠습니다.

### **RAG & 메모리**

제가 구축한 딥 리서치 애플리케이션의 첫 번째 버전은 단기 메모리 사용을 요구하지 않지만, 다양한 사용자 쿼리에 대해 서브쿼리를 캐싱하는 버전을 구축했습니다. 이는 워크플로우에서 일부 속도 향상/최적화를 달성하는 데 유용합니다. 유사한 쿼리가 이전에 사용자에 의해 사용되었다면, 해당 결과를 벡터 스토어에 저장하고 이를 검색하여 이미 생성되고 벡터 스토어에 존재하는 계획에 대해 새로운 서브쿼리 세트를 만들 필요를 피할 수 있습니다. 기억하세요, LLM API를 호출할 때마다 지연 시간과 비용이 증가합니다.

이는 영리한 컨텍스트 엔지니어링입니다. 애플리케이션을 더 동적이고, 저렴하고, 효율적으로 만들기 때문입니다. 보시다시피, 컨텍스트 엔지니어링은 단순히 프롬프트를 최적화하는 것이 아니라, 목표로 하는 목표에 맞는 올바른 컨텍스트를 선택하는 것입니다. 벡터 스토어를 유지하는 방법과 기존 서브태스크를 컨텍스트로 가져오는 방법에 대해서도 더 창의적일 수 있습니다. 창의적이고 새로운 컨텍스트 엔지니어링이 바로 모트(moat)입니다!

### **상태 & 역사적 컨텍스트**

딥 리서치 에이전트의 v1에서는 보여주지 않지만, 이 프로젝트의 중요한 부분은 최종 보고서를 생성하기 위해 결과를 최적화하는 것이었습니다. 많은 경우, 에이전트 시스템은 모든 쿼리나 서브태스크의 하위 집합, 그리고 잠재적으로 웹 검색 API에서 가져오는 데이터를 수정해야 할 수도 있습니다. 이는 시스템이 문제에 대해 여러 번 시도하고 이전 상태와 잠재적으로 시스템의 모든 역사적 컨텍스트에 접근해야 함을 의미합니다.

이것이 우리 사용 사례의 컨텍스트에서 무엇을 의미할까요? 우리 예시에서는 에이전트에게 서브태스크의 상태, 수정사항(있는 경우), 워크플로우의 각 에이전트로부터의 과거 결과, 그리고 수정 단계에서 도움이 되는 기타 필요한 컨텍스트에 대한 접근을 제공하는 것일 수 있습니다. 이러한 유형의 컨텍스트에 대해, 우리가 전달하는 것은 무엇을 최적화하고 있는지에 따라 달라질 것입니다. 여기서 많은 의사결정이 이루어질 것입니다. 컨텍스트 엔지니어링이 항상 간단하지는 않으며, 이 구성 요소가 얼마나 많은 반복을 요구할지 상상하기 시작할 수 있을 것입니다. 이것이 제가 평가와 같은 다른 영역의 중요성을 계속 강조하는 이유입니다. 이러한 모든 것들을 측정하지 않는다면, 컨텍스트 엔지니어링 노력이 작동하고 있는지 어떻게 알 수 있을까요?

## **고급 컨텍스트 엔지니어링 [진행 중]**

이 글에서 다루지 않는 컨텍스트 엔지니어링의 다른 측면들이 많이 있습니다. 예를 들어, 컨텍스트 압축, 컨텍스트 관리 기술, 컨텍스트 안전성, 컨텍스트 효과성 평가(즉, 시간이 지남에 따라 해당 컨텍스트가 얼마나 효과적인지 측정) 등이 있습니다. 이러한 주제들에 대한 더 많은 아이디어를 향후 글에서 공유할 예정입니다.

컨텍스트는 희석되거나 비효율적이 될 수 있습니다(즉, 오래되고 관련 없는 정보로 채워짐). 이는 이러한 문제를 포착하기 위한 특별한 평가 워크플로우를 요구합니다.

저는 컨텍스트 엔지니어링이 AI 개발자/엔지니어를 위한 중요한 기술 세트로 계속 발전할 것으로 예상합니다. 수동 컨텍스트 엔지니어링을 넘어서, 효과적인 컨텍스트 엔지니어링의 처리를 자동화하는 방법을 구축할 기회도 있습니다. 이를 시도한 몇 가지 도구를 보았지만, 이 영역에서 더 많은 진전이 필요합니다.

<Callout type="info" emoji="🎓">
  <strong>이 가이드는 진행 중인 작업입니다.</strong>  

  자신의 프로젝트에 컨텍스트 엔지니어링을 적용하는 방법을 배우세요.

  실용적인 워크플로우, 에이전트 설계 등을 위해 [DAIR.AI Academy](https://dair-ai.thinkific.com/)의 실습 과정에 참여하세요.
  
  또한 2025년 7월 31일에 Pro 회원을 위한 컨텍스트 엔지니어링 워크샵을 개최할 예정입니다. 그곳에서 여러분 중 일부를 만나기를 희망합니다.
    
  <strong>Pro 멤버십 20% 할인을 위해 코드 <code>PROMPTING20</code>을 사용하세요.</strong>
</Callout>

## **참고 자료**

다음은 최근에 컨텍스트 엔지니어링에 대해 글을 쓴 다른 사람들의 권장 읽을거리입니다:
 
* [https://rlancemartin.github.io/2025/06/23/context\_engineering/](https://rlancemartin.github.io/2025/06/23/context_engineering/)  
* [https://x.com/karpathy/status/1937902205765607626](https://x.com/karpathy/status/1937902205765607626)  
* [https://www.philschmid.de/context-engineering](https://www.philschmid.de/context-engineering)  
* [https://simple.ai/p/the-skill-thats-replacing-prompt-engineering?](https://simple.ai/p/the-skill-thats-replacing-prompt-engineering?)  
* [https://github.com/humanlayer/12-factor-agents](https://github.com/humanlayer/12-factor-agents)  
* [https://blog.langchain.com/the-rise-of-context-engineering/](https://blog.langchain.com/the-rise-of-context-engineering/)

*이 글에서 실수를 발견하시면 연락해 주세요.* 