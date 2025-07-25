# 코드 생성(Generating Code)

⚠️ **주의**: 이 섹션은 개발 중입니다.

ChatGPT와 같은 LLM은 코드 생성에 매우 효과적입니다. 이 섹션에서는 ChatGPT를 사용한 코드 생성의 많은 예시를 다룰 것입니다.

아래의 모든 예시에는 OpenAI의 Playground(Chat Mode)와 `gpt-3.5-turbo` 모델이 사용됩니다.

OpenAI의 모든 채팅 모델과 마찬가지로, `System Message`를 사용하여 응답의 동작과 형식을 정의할 수 있습니다. 프롬프트 예시에 대해 다음 System Message를 사용할 것입니다:

```
당신은 주니어 개발자에게 코딩을 가르칠 수 있는 도움이 되는 코드 어시스턴트입니다. 선택한 언어는 Python입니다. 코드를 설명하지 말고 코드 블록 자체만 생성하세요.
```

## 기본 예시(Basic Example)

`User Message`는 모델이 생성하도록 원하는 특정 코드의 지시사항과 함께 프롬프트 자체가 됩니다. 아래는 코드 생성을 위한 첫 번째 기본 프롬프트의 스냅샷입니다. 최종 코드는 `Assistant`에 의해 생성되었습니다.

직접 시도해보고 싶다면 다음 프롬프트를 사용하세요:

```
사용자에게 이름을 물어보고 "안녕하세요"라고 말하는 코드를 작성하세요
```

## 주석을 코드로 변환(Turn Comments Into Code)

이러한 LLM의 코드 생성 기능을 사용하여 주석만으로도 코드를 생성할 수 있습니다. 지시사항을 주석 블록으로 전달하는 또 다른 예시를 살펴보겠습니다:

*프롬프트:*
```
"""
1. 영화 목록을 만드세요
2. 이 영화들의 평점 목록을 만드세요
3. 10개 영화와 그들의 평점으로 구성된 json 객체를 만드세요.
"""
```

*출력:*
```python
movies = ["The Shawshank Redemption", "The Godfather", "The Dark Knight", "Schindler's List", "Pulp Fiction", "The Lord of the Rings: The Return of the King", "Forrest Gump", "Star Wars: Episode V - The Empire Strikes Back", "Inception", "The Silence of the Lambs"]

ratings = [9.3, 9.2, 9.0, 8.9, 8.9, 8.9, 8.8, 8.7, 8.7, 8.6]

movie_ratings = {}

for i in range(len(movies)):
    movie_ratings[movies[i]] = ratings[i]

json_object = json.dumps(movie_ratings, indent=4)

print(json_object)
```

생성된 코드는 작동하는 코드처럼 보입니다. 그러나 출력에는 중요한 `import` 문, 즉 `import json`이 포함되어 있지 않습니다. 이러한 모델들이 작동하는 코드를 생성하지만, 이런 작은 세부사항에 주의를 기울이고 항상 코드를 테스트해야 합니다.

## 함수 완성 또는 다음 줄(Complete Functions or Next Line)

이러한 LLM은 GitHub Copilot과 같은 도구에도 통합되어 개발자에게 유용합니다. 유용한 기능 중 하나는 모델이 함수를 완성할 수 있는 능력입니다.

*프롬프트:*
```python
# 두 숫자를 곱하고 75를 더하는 함수

def multiply(
```

*출력:*
```python
a, b):
    result = a * b
    result += 75
    return result
```

## MySQL 쿼리 생성(MySQL Query Generation)

위의 기본 코드 생성 예시 외에도, 모델을 사용하여 MySQL 쿼리 생성 및 테스트와 같은 프로그래밍의 다른 측면에서 사용할 수 있는 유용한 코드를 생성할 수 있습니다.

프롬프트의 일부로 포함할 수 있는 정보가 있는 데이터셋이 있다고 가정하고, 특정 쿼리를 생성하도록 지시할 수 있습니다. 예를 들어:

```
"""
테이블 departments, 컬럼 = [DepartmentId, DepartmentName]
테이블 students, 컬럼 = [DepartmentId, StudentId, StudentName]
컴퓨터 과학 학부의 모든 학생을 위한 MySQL 쿼리를 생성하세요
"""
```

*출력:*
```mysql
SELECT students.StudentId, students.StudentName
FROM students
INNER JOIN departments
ON students.DepartmentId = departments.DepartmentId
WHERE departments.DepartmentName = 'Computer Science';
```

모델이 쿼리를 생성했습니다! 그러나 이 예시에서는 데이터베이스를 만들어냈기 때문에 생성된 쿼리를 테스트할 데이터베이스가 없습니다. 이 예시를 테스트한다면, 이 데이터베이스를 생성하는 것이 유용할 수 있습니다. 다행히 ChatGPT를 사용하여 이것도 생성할 수 있습니다. 한 가지 접근 방법은 위의 프롬프트를 약간 다른 지시사항으로 수정하여 데이터베이스 스키마를 생성하는 것입니다:

```mysql
테이블 departments, 컬럼 = [DepartmentId, DepartmentName]
테이블 students, 컬럼 = [DepartmentId, StudentId, StudentName]
위의 테이블과 컬럼으로 유효한 데이터베이스 스키마를 생성하세요
```

모델이 다음 스키마를 생성했습니다:

```mysql
CREATE TABLE departments (
  DepartmentId INT PRIMARY KEY,
  DepartmentName VARCHAR(50)
);

CREATE TABLE students (
  DepartmentId INT,
  StudentId INT PRIMARY KEY,
  StudentName VARCHAR(50),
  FOREIGN KEY (DepartmentId) REFERENCES departments(DepartmentId)
);
```

[sqliteonline.com](https://sqliteonline.com/)과 같은 것을 사용하여 임시 데이터베이스를 생성할 수 있습니다.

원래 생성된 쿼리가 유효한지 테스트하려면 더미 데이터도 필요합니다. 이것도 ChatGPT를 사용해보겠습니다! 프롬프트는 다음과 같을 수 있습니다:

```mysql
CREATE TABLE departments (
  DepartmentId INT PRIMARY KEY,
  DepartmentName VARCHAR(50)
);

CREATE TABLE students (
  DepartmentId INT,
  StudentId INT PRIMARY KEY,
  StudentName VARCHAR(50),
  FOREIGN KEY (DepartmentId) REFERENCES departments(DepartmentId)
);

위의 데이터베이스 스키마가 주어졌을 때, 각 테이블에 4개 행을 포함하는 유효한 insert 문을 생성하세요.
```

insert 문이 포함된 출력:

```mysql
INSERT INTO departments (DepartmentId, DepartmentName)
VALUES (1, 'Computer Science'), (2, 'Mathematics'), (3, 'Physics'), (4, 'Biology');

INSERT INTO students (DepartmentId, StudentId, StudentName)
VALUES (1, 101, 'John Doe'), (1, 102, 'Jane Doe'), (2, 201, 'Alice Smith'), (2, 202, 'Bob Johnson');
```

이제 데이터베이스와 더미 데이터가 있으므로 원래 쿼리를 테스트할 수 있습니다. 더미 데이터를 기반으로 결과는 `Jane Doe`와 `John Doe`가 포함된 두 행이어야 합니다.

## 코드 설명(Explain Code)
특정 언어로 프로그래밍을 배우고 있다면, 모델에게 특정 코드 비트를 설명하도록 프롬프팅하는 것이 유용할 수 있습니다. 위에서 생성된 쿼리를 재사용하고 모델에게 설명을 요청해보겠습니다.

이전과 같은 System Message를 사용하고 있다면, 모델을 지시하는 방법에 주의하세요. 예를 들어, 다음 예시를 사용하면 모델은 Python의 코드 어시스턴트라고 불평하지만 여전히 설명을 제공합니다. 때로는 모델이 System Message를 따르도록 설계되어 있기 때문에 지시하는 것을 거부합니다.

*프롬프트:*

```mysql
SELECT students.StudentId, students.StudentName
FROM students
INNER JOIN departments
ON students.DepartmentId = departments.DepartmentId
WHERE departments.DepartmentName = 'Computer Science';

위의 SQL 문을 설명하세요.
```

*출력:*

```
죄송합니다. Python의 코드 어시스턴트로서 SQL 코드를 생성할 수 없습니다. 그러나 제공된 SQL 문의 목적을 설명할 수 있습니다.

이 SQL 문은 `departments` 테이블의 `DepartmentName` 컬럼이 'Computer Science'와 같은 `students` 테이블에서 `StudentId`와 `StudentName` 컬럼을 선택합니다. 이를 위해 문은 `students`와 `departments` 테이블 간의 `DepartmentId` 컬럼에서 내부 조인을 수행합니다.

더 간단히 말하면, 컴퓨터 과학 학부에 등록된 학생들의 이름과 ID를 선택하는 것입니다.
```

## 코드 편집(Editing Code)
예시가 곧 나올 예정입니다!

## 코드 디버깅(Debugging Code)
예시가 곧 나올 예정입니다!

## 모범 사례(Best practices)
곧 나올 예정입니다!
