# Spring mvc 5 with concurrency
This program deals with following features:

   - Basic REST operation (GET / POST)
   
   - Graphql controller
   
   - Concurrent request test
  
### installation guide

1. clone repository

2. run maven install on project

3. download ojdbc8.jar then include it in build path
 (to be included in maven later updates)

### Problem

마을 한 가운데에 우물이 존재합니다. 

초기에 `30L`의 물을 저장하고 있으며, 우물의 최대 저장량은 `50L`입니다.

어떤 사람이든 이 우물을 퍼갈 수 있습니다.

각 사람들은 한번에 1~2L의 우물을 퍼갈 수 있으며, 

여러분들의 임무는 이 우물양이 `0-50L` 범위 내에 있도록 요청을 처리하는 것입니다.

### Objectvie

최소 10명의 동시다발성 요청을 처리하는 서버 프로그램 만들어보기

### Todos

#### Frontend
1. ``index.jsp``에 각 인원은 고유한 이름을 가지고 있으며 각 인원들이 우물을 얼마나 퍼갔는지

통계를 보여줍니다.

2. 각 인원들의 이름 명칭은 자유며, 클릭시 이벤트를 발생시켜 해당 사용자의 정보와 퍼간 내역을 `index.jsp` 내 특정 공간에다 표시합니다.

#### Backend

1. Restful api Service 작성 (보낼때, 받을 때 모두 json 사용)    
    - ``/list`` : `GET` 우물에서 물을 퍼간 사람의 목록을 **물을 많이 퍼간 순** 출력합니다.
        - 응답 예시 :
        ```js
                    users: [
                  {"username" : "person2", "total": 25},
                  {"username" : "person2", "total": 5}
              ]
         ```
    - ``/draw`` : ``DELETE`` 우물에서 `amount`물을 만큼 퍼갑니다.
        - 요청 예시 : ``{"username" : "person1", "amount": 1}``
    - ``/water``: `UPDATE` 현재 있는 우물에 물을 붓습니다.
        - 요청 예시 : ``{"amount" : "1"}``
        
2. graphql를 사용하여 우물에서 퍼간 사람의 정보와 퍼간 내역들을 표시
    
    2-1. 요청
    
    ```js
    {
      user(name: 'person1') {
        name
        logs {
          time,
          amount
        }
      }
    }     
    ```
    2-2. 응답
    ```js
        {
          "data": {
            "username" : "person1"
            logs: [{
             "time" : "2019-06-27 11:00:48",
             "amount" : "2"    
           },{
             "time" : "2019-06-27 11:00:50",
             "amount" : "1"  
        }]
          }
        }     
      ```
3. 데이터베이스 테이블 구조는 본인이 직접 구현
    

4. **Optional** 최소 10명의 동시다발적 restful 요청을 수행하는 Junit Test 작성

스터디장은 Optional 구현 필수(구현시 공유예정)

### 제한사항

1. 우물은 데이터베이스 정보가 아닌 하나의 객체나 변수로 선언가능, 서버 시작시 초기 값 `30`으로 설정

2. 모든 클라이언트 요청은 xhttprequest이나 fetch 중 하나를 사용할 것

3. 한 우물에서 동시에 퍼갈수 있는 사람 수, 우물을 퍼가는 시간 고려하지 않음

4. 모든 유효한 자원에 대한 요청은 반드시 수용할 것
    - 예시1) 30L가 남았을 때 총 10명이 한꺼번에 2L 씩 퍼갔을 경우 정확히 10L가 남아있어야 합니다.
    - 예시2) 30L가 남았을 때 20명이 동시에 2L 씩 퍼가겠다는 요청을 했을 경우 15명의 요청은 수용, 5명의 요청은 거부