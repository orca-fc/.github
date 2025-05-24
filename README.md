# ORCA

## 프로젝트 개요

### 요약

- 축구 동호회 회원 관리 및 경기 매칭 서비스입니다.
- 사용자가 선수로 등록하고 클럽에 가입 신청을 하거나, 클럽을 생성/관리하며, 클럽 간 매칭을 통해 경기를 진행하고 결과를 기록할 수 있습니다.

### 팀 구성

- **개인 프로젝트**

### 주요 기술

- **서버 애플리케이션**: Spring Boot, Spring WebFlux (Reactive), Kotlin Coroutine
- **데이터베이스**: MongoDB, Redis
- **메시지 큐**: Apache Kafka (이벤트 스트리밍 및 비동기 통신)
- 인프라:
    - Kubernetes (배포관리 및 네임스페이스 기반 Dev / Prod 환경 분리)
    - Docker
    - GitHub Actions (CI/CD 자동화)

---

## 서비스 아키텍처

### 개요

- **MSA**를 채택하여 서비스 기능별로 독립적인 서버로 분리
- **Kafka MQ**를 활용한 이벤트 기반 비동기 통신으로 서비스 간 데이터 동기화 및 보상 트랜잭션 처리 (SAGA 패턴)
- **Kubernetes**를 사용해 Dev/Prod 환경을 분리하고, CI/CD 파이프라인을 통해 배포 자동화 구현

## 서비스 구성

### [API Gateway](https://github.com/orca-fc/api-gateway)

- **역할**
    - 모든 클라이언트 요청의 진입점(Front Controller) 역할을 수행하며, 각 마이크로서비스로 요청 라우팅
- **기능**
    - 사용자 인증 토큰(JWT) 검증
    - Kubernetes 서비스 이름을 기반으로 각 마이크로서비스로 라우팅
- **특징**
    - Dev / Prod 환경별로 네임스페이스를 분리하여 라우팅 관리
    - API Gateway는 NodePort로 외부에 노출, 나머지 서비스는 ClusterIP로 설정하여 private하게 관리, 오직 API Gateway를 통해서만 요청이 전달되도록 격리

### [Auth Service](https://github.com/orca-fc/auth)

- **역할**
    - 사용자 인증 관리
- **기능**
    - 회원가입 및 로그인
    - JWT 토큰 발급 및 검증
    - 토큰 재발급

### [Player Service](https://github.com/orca-fc/player)

- **역할**
    - 선수 정보 관리
- **기능**
    - 선수 등록 및 정보 수정
    - 클럽 가입 신청 현황 조회
      
<img width="1472" alt="Image" src="https://github.com/user-attachments/assets/8d20a92a-c236-4d19-8e42-dddd035c4350" />

### [Club Service](https://github.com/orca-fc/club)

- **역할**
    - 클럽 관리
- **기능**
    - 클럽 생성 및 정보 수정
    - 클럽 가입 신청 관리 (승인 / 거절)
    - 클럽 소속 선수단 관리
 
<img width="1442" alt="Image" src="https://github.com/user-attachments/assets/86de8934-1e85-44b6-bf5f-29f113432dd3" />

### [Match Service](https://github.com/orca-fc/match)

- **역할**
    - 클럽 간 매칭 및 경기 결과 관리
- **기능**
    - 매칭 등록 및 진행 현황 관리
    - 경기 결과 기록
    - 상대 클럽 평가

<img width="1469" alt="Image" src="https://github.com/user-attachments/assets/7283a15e-1b7e-4a82-b975-8791bdaa7697" />

### [Chat Service](https://github.com/orca-fc/chat)

- **역할**
    - 채팅 서비스 제공
- **기능**
    - 1:1 채팅
    - 클럽 단체 채팅
    - 실시간 채팅 송/수신
