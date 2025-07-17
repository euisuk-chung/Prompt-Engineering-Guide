# PAL(프로그램 보조 언어 모델, Program-Aided Language Models)

[Gao 등(2022)](https://arxiv.org/abs/2211.10435)은 LLM(대형 언어 모델, Large Language Model)을 활용해 자연어 문제를 읽고, 중간 추론 단계로 프로그램을 생성하는 방법을 제안했습니다. 이 방법은 프로그램 보조 언어 모델(PAL, Program-Aided Language Models)로 명명되었으며, 자유 형식 텍스트로 해답을 도출하는 연쇄적 사고(chain-of-thought) 프롬프트와 달리, 해답 도출 단계를 파이썬 인터프리터(Python interpreter)와 같은 프로그래밍 런타임에 위임한다는 점에서 차별화됩니다.

이미지 출처: [Gao 등(2022)](https://arxiv.org/abs/2211.10435)

LangChain과 OpenAI GPT-3를 활용한 예시를 살펴보겠습니다. 여기서는 질문을 해석하고, 파이썬 인터프리터를 활용해 답을 도출하는 간단한 애플리케이션을 개발하는 것이 목표입니다.

특히, 날짜 이해(date understanding)가 필요한 질문에 대해 LLM이 답변할 수 있도록 하는 기능을 구현하고자 합니다. LLM에 몇 가지 예시가 포함된 프롬프트를 제공하며, 예시는 [여기](https://github.com/reasoning-machines/pal/blob/main/pal/prompt/date_understanding_prompt.py)에서 가져왔습니다.

필요한 파이썬 임포트 예시는 다음과 같습니다:

```python
import openai
from datetime import datetime
from dateutil.relativedelta import relativedelta
import os
from langchain.llms import OpenAI
from dotenv import load_dotenv
```

환경 설정 예시:

```python
load_dotenv()

# API 설정
openai.api_key = os.getenv("OPENAI_API_KEY")

# LangChain용
os.environ["OPENAI_API_KEY"] = os.getenv("OPENAI_API_KEY")
```

모델 인스턴스 설정:

```python
llm = OpenAI(model_name='text-davinci-003', temperature=0)
```

프롬프트 및 질문 예시:

```python
question = "Today is 27 February 2023. I was born exactly 25 years ago. What is the date I was born in MM/DD/YYYY?"

DATE_UNDERSTANDING_PROMPT = """
# Q: 2015 is coming in 36 hours. What is the date one week from today in MM/DD/YYYY?
# If 2015 is coming in 36 hours, then today is 36 hours before.
today = datetime(2015, 1, 1) - relativedelta(hours=36)
# One week from today,
one_week_from_today = today + relativedelta(weeks=1)
# The answer formatted with %m/%d/%Y is
one_week_from_today.strftime('%m/%d/%Y')
# Q: The first day of 2019 is a Tuesday, and today is the first Monday of 2019. What is the date today in MM/DD/YYYY?
# If the first day of 2019 is a Tuesday, and today is the first Monday of 2019, then today is 6 days later.
today = datetime(2019, 1, 1) + relativedelta(days=6)
# The answer formatted with %m/%d/%Y is
today.strftime('%m/%d/%Y')
# Q: The concert was scheduled to be on 06/01/1943, but was delayed by one day to today. What is the date 10 days ago in MM/DD/YYYY?
# If the concert was scheduled to be on 06/01/1943, but was delayed by one day to today, then today is one day later.
today = datetime(1943, 6, 1) + relativedelta(days=1)
# 10 days ago,
ten_days_ago = today - relativedelta(days=10)
# The answer formatted with %m/%d/%Y is
ten_days_ago.strftime('%m/%d/%Y')
# Q: It is 4/19/1969 today. What is the date 24 hours later in MM/DD/YYYY?
# It is 4/19/1969 today.
today = datetime(1969, 4, 19)
# 24 hours later,
later = today + relativedelta(hours=24)
# The answer formatted with %m/%d/%Y is
today.strftime('%m/%d/%Y')
# Q: Jane thought today is 3/11/2002, but today is in fact Mar 12, which is 1 day later. What is the date 24 hours later in MM/DD/YYYY?
# If Jane thought today is 3/11/2002, but today is in fact Mar 12, then today is 3/12/2002.
today = datetime(2002, 3, 12)
# 24 hours later,
later = today + relativedelta(hours=24)
# The answer formatted with %m/%d/%Y is
later.strftime('%m/%d/%Y')
# Q: Jane was born on the last day of Feburary in 2001. Today is her 16-year-old birthday. What is the date yesterday in MM/DD/YYYY?
# If Jane was born on the last day of Feburary in 2001 and today is her 16-year-old birthday, then today is 16 years later.
today = datetime(2001, 2, 28) + relativedelta(years=16)
# Yesterday,
yesterday = today - relativedelta(days=1)
# The answer formatted with %m/%d/%Y is
yesterday.strftime('%m/%d/%Y')
# Q: {question}
""".strip() + '\n'
```

```python
llm_out = llm(DATE_UNDERSTANDING_PROMPT.format(question=question))
print(llm_out)
```

이 코드는 다음과 같은 출력을 생성합니다:

```
# If today is 27 February 2023 and I was born exactly 25 years ago, then I was born 25 years before.
today = datetime(2023, 2, 27)
# I was born 25 years before,
born = today - relativedelta(years=25)
# The answer formatted with %m/%d/%Y is
born.strftime('%m/%d/%Y')
```

`llm_out`의 내용은 파이썬 코드 스니펫입니다. 아래와 같이 `exec` 명령어로 해당 코드를 실행할 수 있습니다.

```python
exec(llm_out)
print(born)
```

이 코드는 다음과 같은 결과를 출력합니다: `02/27/1998`
