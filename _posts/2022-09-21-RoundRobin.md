---
title:  "RoundRobin"
last_modified_at: 2022-09-21T09:00:00+09:00
categories:
  - Programming
tags: 
  - 

---

## RoundRobin 

라운드 로빈은 가장 기본적인 로드 밸런싱 알고리즘 중 하나이다. 이 알고리즘은 클라이언트 요청을 순서대로 각각의 서버에 분산시키는 방식으로 동작한다. 예를들어, 클라이언트 A가 첫 번째 요청을 보냈을 때, 이 요청은 서버 1에 전달된다. 그리고 다음 요청은 서버 2에, 다음 요청은 서버 3에, 이어서 다시 서버 1에 요청이 전달되는 식으로 순서대로 서버에 요청을 분산시킨다.

라운드 로빈은 각 서버에게 공평한 분배를 보장하는 장점이 있다. 또한, 새로운 서버가 추가되거나 기존 서버가 제거되더라도, 다른 설정 변경이 없이 로드 밸런서에 의해 자동으로 적용될 수 있는 유연성을 가지고 있다. 하지만, 서버들의 성능이나 상태에 대한 고려가 없이 순서대로 요청을 분산시키기 때문에, 모든 서버의 부하가 동일하지 않거나 일부 서버가 다운되어 있을 경우, 성능 저하나 서비스 중단 문제가 발생할 수 있다.

따라서, 라운드 로빈을 포함한 다른 로드 밸런서 알고리즘을 선택할 때는 서버의 상태나 부하 분포를 고려하여 적절한 알고리즘을 선택해야 한다.

---

### 라운드 로빈 알고리즘을 C언어로 구현한 예시

이 코드는 요청을 받아 각 서버에 순서대로 분배하는 기본적인 라운드 로빈 알고리즘 코드이다.

```c

#include <stdio.h>
#include <stdlib.h>

#define SERVER_NUM 3 // 서버의 수
#define REQUEST_NUM 10 // 요청의 수

void server1();
void server2();
void server3();

int main() {
  int i;
  void (*servers[SERVER_NUM])() = {server1, server2, server3};
  int current_server = 0; // 현재 요청이 전달 될 서버의 인덱스

  for (i=0; i<REQUEST_NUM; i++) {
    servers[current_server](); // 요청을 현재 서버에 전달
    current_server = (current_server + 1) % SERVER_NUM; // 다음 요청이 전달 될 서버의 인덱스 계산
  }
  return 0;
}

void server1() {
  printf("Request handled by server 1\n");
}

void server2() {
  printf("Request handled by server 2\n");
}

void server3() {
  printf("Request handled by server 3\n");
}

```

이 코드는 3개의 서버와 10개의 요청이 있다고 가정하고, 요청을 서버에 라운드 로빈 방식으로 분배한다. 이 예시 코드에서는 서버의 수와 요청이 수를 매크로 상수로 정의하고, 서버의 함수를 배열에 저장한 후, 반복문을 통해 각 요청을 현재 서버에 전달하고 다음 요청이 전달 될 서버의 인덱스를 계산한다. 요청이 처리될 때마다 해당 서버의 정보를 출력한다.
