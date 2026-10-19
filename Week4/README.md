# Week4: 스택(Stack)과 큐(Queue)

## 1. 실습 목표
- LIFO/FIFO 원리와 ADT 인터페이스 이해
- Python으로 Stack, Queue 클래스 설계·구현·검증
- 괄호 검사와 후위 표기식 계산에 Stack 적용
- 리스트와 `collections.deque`의 큐 구현 비용 비교

## 2. 구현한 내용
| 구현 | 설명 | 시간 복잡도 |
|---|---|---|
| `Stack.push` | top에 추가 (`list.append`) | O(1) |
| `Stack.pop` / `peek` | top 제거·조회, 비어 있으면 `IndexError` | O(1) |
| `Queue.enqueue` | rear에 추가 (`deque.append`) | O(1) |
| `Queue.dequeue` / `front` | front 제거·조회, 비어 있으면 `IndexError` | O(1) |
| `is_balanced` | 괄호 검사 | O(n) |
| `evaluate_postfix` | 후위 표기식 계산 | O(n) |
| `process_jobs` | 작업 대기열 처리 | O(n) |

## 3. 코드 설명
- **Stack**: 내부 저장소로 리스트를 쓰고, 오른쪽 끝을 top으로 사용합니다. 비어 있을 때 `pop`/`peek`은 `IndexError`를 발생시킵니다.
- **Queue**: `deque`를 사용해 front 제거가 O(1)입니다. 리스트의 `pop(0)`은 나머지 원소를 모두 이동시켜 O(n)이라 큐에는 `deque`가 적합합니다.
- **is_balanced**: 여는 괄호는 push하고, 닫는 괄호가 나오면 스택이 비었는지와 top이 대응하는 여는 괄호인지 확인합니다. 모든 문자를 처리한 뒤 스택이 비어 있어야 `True`입니다.
- **evaluate_postfix**: 숫자는 push하고, 연산자는 두 번 pop해서 계산한 결과를 다시 push합니다. 먼저 pop한 값이 오른쪽 피연산자(`b`), 나중에 pop한 값이 왼쪽 피연산자(`a`)입니다. 마지막에 스택에 값이 1개가 아니면 `ValueError`를 발생시킵니다.
- **process_jobs**: 작업을 모두 enqueue한 뒤 큐가 빌 때까지 dequeue해서, 들어온 순서(FIFO) 그대로 처리합니다.

## 4. 테스트 결과
- Stack, Queue 기본 테스트 통과 (빈 상태 예외 처리 포함)
- 괄호 검사: `(a + b) * [c - d]` → True, `([)]` → False, `(()` → False, 빈 문자열 → True
- 후위 표기식: `2 3 + 4 *` → 20, `10 2 / 3 -` → 2
- 작업 대기열: 입력 순서대로 출력

## 5. Stack과 Queue 비교
| 구조 | 삽입 | 제거/조회 | 구현 |
|---|---|---|---|
| Stack (LIFO) | push O(1) | pop·peek O(1) | `list` 오른쪽 끝 |
| Queue (FIFO) | enqueue O(1) | dequeue O(1) | `deque` |

## 6. 느낀 점
괄호 검사처럼 '가장 최근에 열린 것부터 닫는다'는 구조가 스택의 LIFO와 딱 맞는 점이 가장 기억에 남습니다. 리스트의 pop(0)이 O(n)이라 큐에는 deque를 써야 하는 이유도 알게 되었습니다.
