# [부스트캠프 멤버십 18일차]

## 마스터 클래스

### DBMS의 ORM을 사용하지 않고 Query를 구현하는 것에 대하여...
- 좋은 시도라고 생각한다. 
- 아무리 추상화가 잘 되어 있다고 하더라도, 개발자라면 로우한 레벨에서 다뤄보는 경험을 필수적으로 해야한다고 생각한다. 
- 개인적으론 귀찮아서, 관심 없어서 진지하게 시도해보지 않았는데, 이번 기회에 한번 시도해볼 수 있을것 같다. 

### SOAP - XML RPC 
- 이기종간의 통신 방법 정의
- 결국에 REST나 SOAP이나 해결하고자 하는 문제는 비슷하였음. 방법에 차이가 있었을 뿐.

### 여러개의 서버가 하나의 세션 디비 공유하기 위해서는?
- 세션 클러스터링 사용

### Data vs Database vs DBMS
- Data: 데이터, 저장 가능한 값
- Database: 데이터 저장하는 곳
- DBMS: 데이터베이스를 관리하는 시스템

### NoSQL은 CAP을 중요시하고
- 잘 몰랐던 부분이다. AWS의 Dynamodb나 S3가 얼마큼의 consistency, availability, partition tolerance를 중요하게 생각하고 갖고 있는지 외우기만 했지 이 정보가 의미하는 바를 제대로 알지 못했다.
- 오늘 강의를 통해서 이 세가지 요소가 DB에서 정통적으로 중요하게 보는 요소이고, 이 모두를 제공하기 힘드니 셋중에 하나 이상을 타협보는 형태로 감을 알게 되었다. 
- 근래의 Cloud의 환경에서는 consistency를 타협본다고 한다. (eventually consistency를 떠올리자.)

### SQL은 ACID를 중요시한다
- 트랜잭션과 같은 작업은 ACID를 보장하기 위해 SQL을 주로 사용한다. 

## 오늘의 고민들

### REST 하지 않다
- 우리가 일반적으로 작성하는 API는 REST하지 않음을 알게 되었다. 
- 그리고, 이 REST에서 타협을 보아서 HTTP API를 작성하곤한다. 
- 이렇게 작성된 API를 REST라고 불르는건 옳지 않다고 한다. 

여기서 든 나의 생각은...

> 이렇게 REST를 지키기 어렵고, 존재하기 힘든 이론이라면, 이에 대해 타협을 본 개량형 REST 이론은 왜 나오지 않는걸까,,,,

시간 날때 한번 파보도록 하겠다. 

### 클라우드란 무엇인가?
오늘 동료분께서 클라우드에 올리는것과 로컬 서버에 올리는게 근본적으로 무엇에 차이가 있는지와, 클라우드가 무엇인지 물어봤다. 
이에 대한 나의 답은, 

1. 클라우드는 가상화를 이용하여 서버를 사용자가 필요로 할 때 효율적으로 제공하게 도와주는 기술이다. 이와 더불어 규모의 경제를 통하여 사용자에게 가성비 높은 서비스를 제공한다. 

2. 로컬에서 올리는거랑 클라우드 서버에 올리는거랑 기술적으로 무언가 큰 차이가 있는건 아니다. 
- 다만, 클라우드 밴더별로 더 나은 서비스를 제공하기 위해 기본적인 무언가를 녹여서 제공하고, 이런 부분에 차이가 있을 수 있다. 
  - 예를 들어, 일정한 Range의 포트가 미리 예약되어있어 사용할 수 없다던지
  - VPC에서 IP주소중 일부분이 예약되어 있다던지 
  - 서버의 특정 위치에 기본적인 AWS정보가 녹아있다던지...등등

## 오늘의 회고
또!!!!! 지각을 하였다. 
왜 스스로의 다짐을 지키지 못하는가....
내일이 나의 마지막 기회라 생각하고 다시는 지각하지 말자. 

오늘은 전반적으로 아는 지식을 돌아보는 시간이였다. 
다행히 내가 관심 있던 분야의 내용들 위주였기 때문에 수업에 집중하여 참여할 수 있었다. 

그렇지만, 내가 명확하게 알지 못하던 부분도 몇개씩 보였다. 이런 부분을 정리하여 바로잡을 필요가 있다. 
