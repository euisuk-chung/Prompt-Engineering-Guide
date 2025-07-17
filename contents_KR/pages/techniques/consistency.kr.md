# 셀프 컨시스턴시(Self-Consistency)

프롬프트 엔지니어링(Prompt Engineering)에서 가장 발전된 기법 중 하나는 셀프 컨시스턴시(Self-Consistency)입니다. [Wang 외, 2022](https://arxiv.org/abs/2203.11171)가 제안한 이 방법은 "체인 오브 쏘트(Chain-of-Thought, CoT) 프롬프트에서 사용되는 단순한 탐욕적(greedy) 디코딩을 대체"하는 것을 목표로 합니다. 핵심 아이디어는 퓨샷 CoT를 통해 다양한 추론 경로를 여러 번 샘플링하고, 이 결과들 중 가장 일관된 답을 선택하는 것입니다. 이를 통해 산술 및 상식 추론이 필요한 과제에서 CoT 프롬프트의 성능을 크게 높일 수 있습니다.

아래는 산술 추론 예시입니다:

*프롬프트:*
```
When I was 6 my sister was half my age. Now
I’m 70 how old is my sister?
```

*출력:*
```
35
```

출력이 틀렸습니다! 셀프 컨시스턴시로 어떻게 개선할 수 있을까요? Wang 외(2022)의 퓨샷 예시(Table 17)를 활용해 보겠습니다:

*프롬프트:*
```
Q: There are 15 trees in the grove. Grove workers will plant trees in the grove today. After they are done,
there will be 21 trees. How many trees did the grove workers plant today?
A: We start with 15 trees. Later we have 21 trees. The difference must be the number of trees they planted.
So, they must have planted 21 - 15 = 6 trees. The answer is 6.

Q: If there are 3 cars in the parking lot and 2 more cars arrive, how many cars are in the parking lot?
A: There are 3 cars in the parking lot already. 2 more arrive. Now there are 3 + 2 = 5 cars. The answer is 5.

Q: Leah had 32 chocolates and her sister had 42. If they ate 35, how many pieces do they have left in total?
A: Leah had 32 chocolates and Leah’s sister had 42. That means there were originally 32 + 42 = 74
chocolates. 35 have been eaten. So in total they still have 74 - 35 = 39 chocolates. The answer is 39.

Q: Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops. How many lollipops
did Jason give to Denny?
A: Jason had 20 lollipops. Since he only has 12 now, he must have given the rest to Denny. The number of
lollipops he has given to Denny must have been 20 - 12 = 8 lollipops. The answer is 8.

Q: Shawn has five toys. For Christmas, he got two toys each from his mom and dad. How many toys does
he have now?
A: He has 5 toys. He got 2 from mom, so after that he has 5 + 2 = 7 toys. Then he got 2 more from dad, so
in total he has 7 + 2 = 9 toys. The answer is 9.

Q: There were nine computers in the server room. Five more computers were installed each day, from
monday to thursday. How many computers are now in the server room?
A: There are 4 days from monday to thursday. 5 computers were added each day. That means in total 4 * 5 =
20 computers were added. There were 9 computers in the beginning, so now there are 9 + 20 = 29 computers.
The answer is 29.

Q: Michael had 58 golf balls. On tuesday, he lost 23 golf balls. On wednesday, he lost 2 more. How many
golf balls did he have at the end of wednesday?
A: Michael initially had 58 balls. He lost 23 on Tuesday, so after that he has 58 - 23 = 35 balls. On
Wednesday he lost 2 more so now he has 35 - 2 = 33 balls. The answer is 33.

Q: Olivia has $23. She bought five bagels for $3 each. How much money does she have left?
A: She bought 5 bagels for $3 each. This means she spent $15. She has $8 left.

Q: When I was 6 my sister was half my age. Now I’m 70 how old is my sister?
A:
```

*출력 1:*
```
When I was 6 my sister was half my age, so she was 3. Now I am 70, so she is 70 - 3 = 67. The answer is 67.
```

*출력 2:*
```
When the narrator was 6, his sister was half his age, which is 3. Now that the narrator is 70, his sister would be 70 - 3 = 67 years old. The answer is 67.
```

*출력 3:*
```
When I was 6 my sister was half my age, so she was 3. Now I am 70, so she is 70/2 = 35. The answer is 35.
```

최종 답을 계산하는 과정은 몇 단계가 더 필요하지만(자세한 내용은 논문 참고), 단순하게 다수결로 이미 정답이 도출되는 것을 볼 수 있습니다. 이처럼 셀프 컨시스턴시는 다양한 추론 경로를 활용해 더 신뢰도 높은 답을 얻는 데 효과적입니다.