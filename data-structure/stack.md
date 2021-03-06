# Stack

## Stack이란?

데이터를 제한적으로 접근할 수 있는 구조. 가장 나중에 넣은 데이터를 가장 먼저 빼낼 수 있는 데이터 구조.\(LIFO\)

* push: 데이터를 스택에 넣기
* pop: 데이터를 스택에서 빼

## 활용

컴퓨터 내부의 프로세스 구조의 함수 동작 방식

## 장단점

* 장점: 구조가 단순해서 구현이 쉽다. 데이터 저장/읽기 속도가 빠르다.
* 단점: 데이터의 최대 갯수를 미리 정해야 한다. 저장 공간의 낭비가 발생할 수 있다.\(최대 갯수를 계속 확보하고 있으니까\)
* 스택은 단순하고 빠른 성능을 위해 사용되므로 보통 배열 구조를 활용해서 구현하지만, 위의 단점이 발생할 수 있음.

## 구현

javascript에서는 실제로 push, pop 메서드가 있다.

