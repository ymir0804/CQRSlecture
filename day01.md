MSA(Microservice Architecture)의 이해
=============
## 개론 핵심 정리
+ 수직확장
  + Scale-up (수직 확장) 
    + 인스턴스의 스펙 증가
    + Ex) RAM 8GB -> RAM 16GB
  + Scale-out (수평 확장)
    + 인스턴의 갯수 증가
    + Ex) RAM 8GB 인스턴스를 여러 대 증가시켜 부하를 분산
+ Cloud Native와 MicroService
  + 인프라가 가변적으로 쉽게 변경되는 환경이기 떄문에 MicroService가 이용 가능
    + 전체 시스템의 인스턴스를 한꺼번에 확장하는 것이 아니라, 사용량이 몰리는 일부 기능(MicroService로 나뉘어진) 리소스만 증가를 하면 되기 때문
+ Monolith Architecture VS MicroService Architecture
  + Monolith Architecture
    + 애플리케이션이 한 덩어리로 구성되어있어, 일부만 수정하게 되는 경우 한꺼번에 수정, 배포가 되어야함
    + 수정한 부분 때문에 빌드, 배포가 실패한 경우 전체 시스템 빌드, 배포가 실패
  + MicroService Architecture
    + 하나의 어플리케이션이 여러 개의 서비스 조각으로 나뉘어져 있음
    + 서비스는 각각의 독립적인 기능을 제공
      + 각각의 서비스는 다른 서비스들과 완벽하게 분리되어있음(DataBase 포함)
    + 서비스 수정 후 빌드, 배포 실패 시 전체 어플리케이션에 영향을 주는것이 아닌 수정 한 서비스에만 영향을 주게됨
    + 확장 시 독립적으로 확장을 할 수 있으며, 독립적으로 서로 다른 언어로 개발 할수 있음(Polyglot)
    +  <span style="color: #fffb51"/> API Layer같은 경우 Business Logic을 넣지 않음
+ MSA(MicroService Architecture)의 성공 조건
  + 아키텍쳐만 바꾸는 것이 아니라 전반적인 변화가 필요
    + 조직
      + 도메인에 대해서 자율적 권한, 책임을지는 DevOps
      + 다 기능팀으로 팀을 구성
        + 2 pizza Team이라 하여 피자 두판을 먹을 수 있는 인력으로 구성 개발+운영인원
      + <span style="color: #808080"/> SI같은 경우에는 Dev와 Ops와 확연하게 분리되어있어, SI 프로젝트 진행 시 Dev가 책임없이 개발하고 Ops에게 폭탄돌리기를 하는 경우가 많다
    + 인프라
      + 확장성이 쉬운 CloudNative Infra 구성(Auto-Scaing)
    + 자동화
      + DevOps Infra(CI/CD)
    + 개발 프로세스
      + Agile Process VS Waterfall Process
        + Agile Process 같은 경우 Product위주 이고, Waterfall 같은 경우 Project위주
        + <span style="color: #dcffe4"/> SI 같은 경우에는 Waterfall process가 적합 한 것 같고, 솔루션 같은 경우에는 agile process가 적합 한 것 같다
    + 개발 문화
      + 담당하는 개발자가 없으면 개밣을 할 수 없는 개발문화가 아닌, 서로 간 공유/학습을 하는 개발문화
    + 설계 방식
      + 데이터는 독립적인 서비스로 나뉘어져있기 떄문에 중복이 발생할 수 있으나, 결과적으로 데이터의 일관성을 추구하는 방식
  + 구축 방식
    + 고도의 디커플링을 요구하기 때문에, 재사용보다 중복을 지향함
    + DataBase의 완벽한 격리가 필요하며, 데이터 일관성 처리를 위한 일관성 매커니즘이 필요
### 아키텍처 & 아키텍트
+ 하는 일
  + 시스템 제약조건, 개발자가 해도 되는 것과 하면 안되는 것을 정의
  + 업무 분석 후 적합 한 아키텍처 도출
  + 아키텍처 준수를 위하여 개발자에게 지속적으로 가이드 및 멘토링 제공 
  + <span style="color: #fffb51"/> 아키텍처 설계 시 가장 중요한 것은 경험!!
+ 종류
  + Layered Architecture
    + 일반적으로 Presentation, business, persistance, database로 구성
    + 아키텍처 싱크홀 안티 패턴이 생길 수 있음
      + 다른 레이어로 이동 시, 아무 로직도 처리하지 않고 지나는 것을 말함
    + 각 레이어간 폐쇄를 하거나 개방할 수 있음
    + MVC패턴과 비교
      + Layered Architecture로 표현
        + Model은 로직 수행 후 나온 결과물이며, View는 화면이고, C는 Presentation Layer에 해당
    + Pipeline Architecture
      + 주로 Batch프로그래밍 시 사용하는 Architecture
      + EDI, ETL시 사용
    + MicroKernel Architecture
      + Plug-in Architecture
      + 주로 SW(Eclipse, 젠킨스)등에 사용
    + Service Oriented Architecture
      + 재 사용성 극대화
      + 대표적인 예시 
        + RestAPI
      + 중앙 공유 DataBase 사용
        + 신뢰성
        + 내고장성에 취약
          + DB변경(Column명 변경 및 추가) 시 SideEffect 발생 가능성이 많음
          + DB의 논리적 분할, 전용 공유 라이브러리화, 도메인별 접근권한 분리를 하여 해당 취약점을 어느정도 보완할수 있음
      + 다양한 Architecture 변형 가능
        + 단일 모놀리식 유저 인터페이스
        + 도메인 기반 유저 인터페이스
        + 서비스 기반 유저 인터페이스
      +  각 데이터베이스에 있는 도메인 데이터를 다른 도메인의 서비스가 필요하지 않도록 설계하는 것이 중요
      +  
    + Event Driven Architecture
      + 확장성이 뛰어나고 널리쓰이는 비동기 분산 아키텍쳐
      + 이벤트에 반응하는 방식(Pub/Sub)
        + 카페예시
          + 카운터에서 주문을 받으면 바리스타가 해당 주문을 통해 커피 생성
            + 주문이 많은 경우 카운터 받는 사람을 증원 시키면되고, 주문 받은 음료의 양이 많은 경우 바리스타를 증가시키면됨
      + 장점
        + 성능, 응답성, 확장섬
      + 단점
        + 비동기 분산 아키텍쳐다 보니 트랜젝션 통제, 교착상태, 데이터비일관성 문제 발생
        + 에러 발생 시 처리하기가 어려움
      + 예시
        + RabbitMQ, Apache Camel
      + 중재자 토폴로지
        + 이벤트 발생 시 이벤트 중재자를 두어 이벤트를 처리하도록 함
        + 장점
          + 오케스트레이션, 에러 처리 시 편함
        + 단점
          + 확장시 확장이 어려움
          + 중재자 부분이 병목 지점이라 성능 저하 발생가능성 높음
    + Space based Architecture
      + 사용자가 급증하거나 급감 시 사용하는 아키텍쳐
## MSA의 관심사
+ MSA Architecture 구성요소
  + https://landscape.cncf.io/
+ Inner/Outer Architecture
  + Inner Architecture
    + 프론트엔드 서비스, 백엔드 서비스에 해당하는 것이 Inner
  + Outer Architecture
    + Platform, Infrastructre에 해당하는 부분이 Outer
+ MSA의 패턴
  + InfraStructure
    + CI/CD
    + VM, Container관리
      + IaaS
      + CaaS
  + Platform
  + Application