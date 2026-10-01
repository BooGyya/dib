<div align="center">

<img src="docs/assets/logo.png" alt="dib 로고" width="320"/>

### 실시간 경매를 더 쉽고 안전하게

**AI 기반 거래 안전성을 결합한 C2C 경매 플랫폼**

SSAFY 15기 특화 프로젝트 · 2026.08.18 ~ 2026.09.30 · TEAM B101 (도파민)

![Kotlin](https://img.shields.io/badge/Kotlin_2.2-7F52FF?style=flat-square&logo=kotlin&logoColor=white)
![Jetpack Compose](https://img.shields.io/badge/Jetpack_Compose-4285F4?style=flat-square&logo=jetpackcompose&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_4-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Java](https://img.shields.io/badge/Java_25-007396?style=flat-square&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white)
![Kubernetes](https://img.shields.io/badge/AWS_EKS-FF9900?style=flat-square&logo=kubernetes&logoColor=white)

<img src="docs/assets/hero.png" alt="dib 메인 비주얼" width="720"/>

</div>

---

## 📑 목차

1. [프로젝트 소개](#-프로젝트-소개)
2. [주요 기능](#-주요-기능)
3. [화면 구성](#️-화면-구성)
4. [핵심 기술 — AI 거래 안전성](#-핵심-기술--ai-거래-안전성)
5. [기술 스택](#️-기술-스택)
6. [시스템 아키텍처](#️-시스템-아키텍처)
7. [명세](#-명세)
8. [프로젝트 폴더 구조](#-프로젝트-폴더-구조)
9. [실행 방법](#-실행-방법)
10. [팀원 소개](#-팀원-소개)
11. [협업 방식](#-협업-방식)

---

## 👋 프로젝트 소개

**dib**는 중고 물품을 **실시간 경매**로 사고팔면서, 경매 과정에서 생기는 위험을 **AI가 먼저 걸러 주는** C2C 리커머스 앱입니다.

- **기획 배경** — 리커머스 시장은 17년 만에 10배 이상 성장했지만, 판매자는 *"얼마에 팔아야 할지 모르겠다"*, 구매자는 *"선착순이라 사기 어렵다"*는 고민이 그대로입니다. 경매는 이 두 문제를 동시에 풀어 주는 거래 방식입니다.
- **문제 의식** — 경매에는 판매자 측 계정이 가격을 띄우는 **허위 입찰(Shill Bidding)**, 주류·담배·의약품·무기류 같은 **불법 거래물 등록**이라는 위험이 따라옵니다. dib는 이 둘을 AI로 탐지합니다.
- **차별점** — 숏폼처럼 넘겨보는 **Live 방송 경매**와 일반 경매를 하나의 거래 흐름(입찰 → 결제 → 배송 → 구매확정 → 정산)으로 묶었습니다.

| | |
| --- | --- |
| **서비스명** | dib (딥) — "dibs!" 찜하다, 먼저 차지하다 |
| **프로젝트 기간** | 2026.08.18 ~ 2026.09.30 (SSAFY 15기 특화 프로젝트) |
| **팀** | B101 도파민 (6명) |
| **플랫폼** | Android 네이티브 앱 + 관리자 웹 |
| **배포** | AWS EKS (Terraform으로 시연 기간에만 띄우는 임시 환경) |

---

## ✨ 주요 기능

### 🔨 실시간 경매
- STOMP WebSocket으로 입찰이 모든 참여자에게 즉시 전파되고, 마감 직전 입찰 시 남은 시간이 15초로 재설정됩니다.
- Redisson 분산락과 ShedLock 스케줄러로 Pod가 여러 개여도 입찰 순서와 마감 처리가 꼬이지 않습니다.
- 낙찰자가 결제하지 않으면 차순위 입찰자에게 자동으로 구매를 제안합니다.

### 📺 Live 방송 경매
- 판매자가 LiveKit으로 송출하고, 시청자는 피드를 숏폼처럼 세로로 넘기며 실시간 채팅과 입찰을 함께 합니다.

### 🛡️ AI 거래 안전성
- **허위 입찰 탐지** — 규칙 점수와 LightGBM 모델 점수로 Risk Score를 산출해 관리자에게 알립니다.
- **불법 거래물 검수** — 금칙어 필터 → 멀티모달 LLM → 사람 검토의 3단계로 자동 승인·반려·보류를 결정합니다.
- **개인화 추천** — BGE-M3·SigLIP 임베딩과 행동 로그로 사용자별 추천 경매를 제공합니다.

### 💳 주문 · 결제 · 정산
- 토스페이먼츠 빌링으로 낙찰 즉시 자동 결제되고, 택배사 API로 배송을 조회하며, 배송 완료 5일 후 자동 구매확정 됩니다.
- 수수료 5%를 제한 금액을 판매자에게 정산하고, 주문별 1:1 채팅·신고·문의·후기를 지원합니다.

### 🔐 인증 · 관리자
- 이메일 회원가입, 카카오 OAuth, Firebase 휴대전화 인증, JWT 액세스·리프레시 토큰.
- React 관리자 웹에서 보류 상품 승인·반려, 이상 입찰 조사, 신고·문의 처리, 회원 제재를 처리합니다.

---

## 🖥️ 화면 구성

### 온보딩 & 인증

| 랜딩 | 로그인 | 회원가입 (3단계) |
| --- | --- | --- |
| <img src="docs/assets/screens/welcome.png" width="260"/> | <img src="docs/assets/screens/login.png" width="260"/> | <img src="docs/assets/screens/signup.png" width="260"/> |

### 홈 · 탐색

| 메인 홈 | 카테고리 | 검색 |
| --- | --- | --- |
| <img src="docs/assets/screens/home.png" width="260"/> | <img src="docs/assets/screens/category.png" width="260"/> | <img src="docs/assets/screens/search.png" width="260"/> |

| 마감 임박 경매 | 추천 경매 | 찜한 경매 |
| --- | --- | --- |
| <img src="docs/assets/screens/closing-soon.png" width="260"/> | <img src="docs/assets/screens/recommended.png" width="260"/> | <img src="docs/assets/screens/favorites.png" width="260"/> |

### 경매 상세 · 입찰 · 낙찰

| 경매 상세 | 판매자 프로필 | 낙찰 안내 (자동 결제) |
| --- | --- | --- |
| <img src="docs/assets/screens/auction-detail.png" width="260"/> | <img src="docs/assets/screens/seller-profile.png" width="260"/> | <img src="docs/assets/screens/win-notice.png" width="260"/> |

### Live 방송 경매

| Live 시청 | Live 입찰 시트 | Live 목록 |
| --- | --- | --- |
| <img src="docs/assets/screens/live.png" width="260"/> | <img src="docs/assets/screens/live-bid.png" width="260"/> | <img src="docs/assets/screens/live-list.png" width="260"/> |

### 상품 등록 · 구매 거래

| 상품 등록 | 구매 거래 상세 (결제 전) | 구매 거래 상세 (발송 준비 중) |
| --- | --- | --- |
| <img src="docs/assets/screens/register.png" width="260"/> | <img src="docs/assets/screens/order-before-payment.png" width="260"/> | <img src="docs/assets/screens/order-waiting.png" width="260"/> |

| 상품 도착 · 구매 확정 | 거래 완료 | 거래 채팅 |
| --- | --- | --- |
| <img src="docs/assets/screens/order-arrived.png" width="260"/> | <img src="docs/assets/screens/order-completed-2.png" width="260"/> | <img src="docs/assets/screens/order-chat.png" width="260"/> |

### 마이페이지 · 결제수단 · 알림

| 마이페이지 | 결제수단 관리 | 알림 |
| --- | --- | --- |
| <img src="docs/assets/screens/my.png" width="260"/> | <img src="docs/assets/screens/payment-methods.png" width="260"/> | <img src="docs/assets/screens/notifications.png" width="260"/> |

---

## 🧠 핵심 기술 — AI 거래 안전성

### 1. 허위 입찰(Shill Bidding) 탐지

<div align="center">

| 정상 경매 | 허위 입찰 |
| --- | --- |
| <img src="docs/assets/normal-auction.png" width="400"/> | <img src="docs/assets/shill-bidding.png" width="400"/> |

</div>

- **모델** — 공개 Shill Bidding Dataset의 7개 Feature로 LightGBM을 학습했습니다. 양성 비율 10.7%의 불균형 데이터라 Accuracy 대신 **PR-AUC**(0.705)를 주요 지표로 삼았습니다.
- **단조성 제약** — 7개 Feature 모두 "값이 클수록 위험"으로 방향을 고정해, 위험한 행동이 위험도를 낮추는 모순을 막았습니다.
- **등조 보정** — 모델이 0.7이라고 말할 때 실제 이상 입찰 비율이 22%에 불과한 과신을 보정했습니다.
- **Risk Score** — 입찰 철회 속도, 입찰 독점도, 신규 계정 + 고액 입찰, 판매자 집중도, 종료 직전 입찰 급증 5가지 규칙 점수와 ML 점수를 가중 합산합니다.

| 보정 효과 | 보정 전 | 보정 후 |
| --- | --- | --- |
| Brier Score (예측 확률 오차) | 0.0795 | 0.0437 (약 45% 개선) |
| 고위험군(≥0.6) 검토 대상 건수 | 368건 | 112건 (약 70% 감소) |
| 검토 대상 중 실제 이상 입찰 비율 | 48.4% | 78.6% (약 30%p 증가) |

### 2. 불법 거래물 검수

```
상품 등록 ──▶ 1차 Rule Filter ──▶ 2차 Multimodal LLM ──▶ 사람 검토
              (Aho-Corasick)       (텍스트 + 이미지)
```

- **1차 Rule Filter** — Aho-Corasick 다중 패턴 매칭으로 금칙어를 찾고, `15` → `I5`·`ㅣ5`, `지.우.개`, `ㅇㅕㄴㅍㅣㄹ` 같은 **우회 표기를 정규화**해서 잡습니다.
- **2차 Multimodal LLM** — 상품명·설명과 사진을 함께 보고 문맥을 판단합니다. "전자담배" 키워드가 있어도 사진 속 판매 대상이 아니면 통과시킵니다.
- **최종 판정** — 확신도가 기준에 못 미치면 관리자 검토 대기(PENDING)로 넘깁니다.

### 3. 개인화 추천

- **Base Score** — 마감 임박도 0.4, 인기도 0.3, 경쟁도 0.3의 가중 합으로 누구에게나 볼만한 경매를 먼저 보여 줍니다.
- **임베딩** — 텍스트는 BGE-M3, 이미지는 SigLIP으로 임베딩해 pgvector에서 유사 상품을 찾습니다.
- **행동 기반 Affinity** — BID · PURCHASE · WATCH · SEARCH · CLICK 등 행동에 가중치를 주고, 반감기 14일의 지수 감쇠로 최근 취향을 더 반영합니다.

---

## 🛠️ 기술 스택

### 📱 Android
![Kotlin](https://img.shields.io/badge/kotlin_2.2-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)
![Jetpack Compose](https://img.shields.io/badge/jetpack_compose-4285F4?style=for-the-badge&logo=jetpackcompose&logoColor=white)
![Navigation Compose](https://img.shields.io/badge/navigation_compose-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![OkHttp](https://img.shields.io/badge/okhttp_5-3E4348?style=for-the-badge&logo=square&logoColor=white)
![LiveKit](https://img.shields.io/badge/livekit_android-1F8CEB?style=for-the-badge)
![Firebase](https://img.shields.io/badge/firebase_auth-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![Kakao](https://img.shields.io/badge/kakao_oauth-FFCD00?style=for-the-badge&logo=kakaotalk&logoColor=black)

### ⚙️ Backend
![Java](https://img.shields.io/badge/java_25-007396?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/spring_boot_4.1-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/spring_security-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/spring_data_jpa-6DB33F?style=for-the-badge&logo=spring&logoColor=white)
![MyBatis](https://img.shields.io/badge/mybatis-DC382D?style=for-the-badge)
![WebSocket](https://img.shields.io/badge/stomp_websocket-333333?style=for-the-badge)
![Kafka](https://img.shields.io/badge/spring_kafka-231F20?style=for-the-badge&logo=apachekafka&logoColor=white)
![Redisson](https://img.shields.io/badge/redisson-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Flyway](https://img.shields.io/badge/flyway-CC0200?style=for-the-badge&logo=flyway&logoColor=white)
![Toss Payments](https://img.shields.io/badge/toss_payments-0064FF?style=for-the-badge)

### 🤖 AI
![Python](https://img.shields.io/badge/python_3.13-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/fastapi-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![LightGBM](https://img.shields.io/badge/lightgbm-9ACD32?style=for-the-badge)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![Transformers](https://img.shields.io/badge/transformers-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![Gemini](https://img.shields.io/badge/gemini-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white)

### 🗄️ Database · Messaging · Infra
![PostgreSQL](https://img.shields.io/badge/postgresql_16_+_pgvector-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/redis_7-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Kafka](https://img.shields.io/badge/apache_kafka-231F20?style=for-the-badge&logo=apachekafka&logoColor=white)
![AWS EKS](https://img.shields.io/badge/aws_eks-FF9900?style=for-the-badge&logo=amazoneks&logoColor=white)
![Terraform](https://img.shields.io/badge/terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![Docker](https://img.shields.io/badge/docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/github_actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)

### 🧑‍💼 Admin Web · 협업
![React](https://img.shields.io/badge/react_19-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/typescript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![GitHub](https://img.shields.io/badge/github-181717?style=for-the-badge&logo=github&logoColor=white)
![Figma](https://img.shields.io/badge/figma-F24E1E?style=for-the-badge&logo=figma&logoColor=white)
![Notion](https://img.shields.io/badge/notion-000000?style=for-the-badge&logo=notion&logoColor=white)

---

## 🏗️ 시스템 아키텍처

<div align="center">
<img src="docs/assets/architecture-overview.png" alt="시스템 아키텍처 개요" width="900"/>
</div>

| 포인트 | 설명 |
| --- | --- |
| <img src="docs/assets/architecture-websocket.png" width="420"/> | **입찰 정보를 실시간으로 전달** — 앱과 서버를 STOMP WebSocket으로 연결하고, Redis Pub/Sub으로 Pod 간 브로드캐스트합니다. |
| <img src="docs/assets/architecture-redis-kafka.png" width="420"/> | **데이터 캐싱과 비동기 처리** — Redis가 캐시·분산락을, Kafka가 입찰·마감·주문 이벤트를 AI 서버와 알림으로 비동기 전달합니다. |
| <img src="docs/assets/architecture-k8s.png" width="420"/> | **요청 증가에 맞춰 자동 확장** — Kubernetes HPA가 입찰 트래픽에 따라 Spring Boot Pod를 늘리고 줄입니다. |

<details>
<summary><b>🔍 상세 아키텍처 (AWS VPC · 서브넷 · CQRS 구조)</b></summary>

<div align="center">
<img src="docs/assets/architecture-detail.png" alt="상세 시스템 아키텍처" width="1000"/>
</div>

</details>

### ERD

<div align="center">
<img src="docs/assets/erd.png" alt="ERD" width="1000"/>
</div>

---

## 📋 명세

기획 단계에서 Notion과 Google Sheets에 정리한 명세서입니다. 명세는 설계 기준이고, 일부 항목(입찰 보증금, 에스크로 등)은 구현 과정에서 자동결제 방식으로 단순화됐습니다.

| 문서 | 규모 | 내용 |
| --- | --- | --- |
| 요구사항 명세서 | 53건 | 회원 · 상품 · 경매 · Live · 결제 · 거래 · 배송 · 정산 · 관리자 |
| 기능 명세서 | 138건 | 13개 대분류 — 회원 · Live · 상품 · 일반 경매 · 거래 · 결제 · 관리자 등 |
| API 명세서 | 96개 | Command 55 · Query 41, 인증 권한과 오류 코드 명시 |
| WebSocket · Kafka 이벤트 명세 | 33개 이벤트 · 40개 토픽 | 경매 · 입찰 · 주문 · Live 채널, 파티션 키와 전달 보장 |
| 공통 오류 코드 · Redis Key 정책 | 125개 · 54개 | 도메인별 오류 응답, 세션 · 캐시 · 스케줄러 키와 TTL |

**핵심 정책**
- 종료 30초 이내 유효 입찰이 들어오면 15초씩 연장하고, 종료 시각에 낙찰 또는 유찰로 마감합니다.
- 낙찰 즉시 등록된 결제수단으로 자동 결제하고, 미결제 취소 시 차순위 입찰자에게 한 번 구매를 제안합니다.
- 결제 후 2영업일 안에 발송이 확인되지 않으면 자동 환불하고 판매자 정산을 보류합니다.
- 한 상품은 진행 중인 경매에 중복 등록할 수 없고, 판매자는 자기 경매에 입찰할 수 없습니다.

---

## 📁 프로젝트 폴더 구조

5개 저장소를 하나로 합친 모노레포입니다. 각 폴더는 원래 독립 저장소였고, 커밋 히스토리를 그대로 보존했습니다.

```
.
├── dib-frontend/        # Android 앱 (Kotlin · Jetpack Compose) + 관리자 웹 (React)
├── dib-backend/         # Spring Boot 4 API 서버 (Java 25)
├── dib-ai/              # FastAPI AI 서버 (검수 · 이상입찰 · 추천)
├── dib-infra/           # Terraform · Kubernetes 매니페스트 · 배포 스크립트
├── dib-orchestration/   # 통합 실행(docker-compose) · 설계 문서 · 실행 계획
└── docs/                # README 자산
```

<details>
<summary><b>📱 dib-frontend</b></summary>

```
dib-frontend
├─ app/src/main/java/com/ssafy/dib
│  ├─ core
│  │  ├─ auth/            # KakaoOAuth
│  │  ├─ navigation/      # AppNavHost (5탭 + 딥링크)
│  │  ├─ network/         # OkHttp · STOMP 클라이언트
│  │  ├─ session/
│  │  └─ ui/              # 공통 컴포넌트 · 디자인 토큰
│  ├─ data
│  │  ├─ local/
│  │  ├─ remote/          # live · socket · api
│  │  └─ repository/
│  ├─ domain               # auction · auth · live · member · notification · order
│  │                       # payment · product · report · settlement · support
│  ├─ feature
│  │  ├─ auth/            # EntryScreens · SignupScreen · PasswordReset · FindEmail
│  │  ├─ home/            # HomeScreen · CategoryScreen · LiveListScreen · Recommended · Search
│  │  ├─ feed/            # LiveFeedScreen (숏폼형 Live 피드)
│  │  ├─ auction/         # ProductDetail · BidSheet · AuctionRegister · ProductReport · SellerProfile
│  │  ├─ live/            # LiveBroadcastConsole · LiveManagement · LiveVideoSession
│  │  └─ main/            # MainTab · Transaction · OrderChat · PaymentMethods · Settlement · Settings
│  └─ ui/theme
├─ admin-web/              # React 19 + Vite 관리자 콘솔
└─ docs/                   # design-system.md · backend-integration.md · 와이어프레임 노트
```

</details>

<details>
<summary><b>⚙️ dib-backend</b></summary>

```
dib-backend (com.b101.dib)
├─ auth            # 이메일 · 카카오 · Firebase 휴대전화 인증 · JWT
├─ member          # 회원 · 제재
├─ product         # 상품 등록 · AI 검수 연동 (command / query 분리)
├─ productImage    # 로컬 / S3 스토리지
├─ category
├─ auction         # 경매 생명주기 · 마감 스케줄러 · 분산락
├─ bid             # 입찰 · 입찰 보증금
├─ bookmark
├─ liveBroadcast   # Live 방송 · LiveKit 토큰
├─ liveChatting
├─ order           # 주문 · 배송 조회(carrier)
├─ payment         # Toss Payments 결제 · 웹훅
├─ paymentMethod   # 빌링키
├─ settlement      # 정산 · 지급(payout)
├─ address
├─ chatting        # 주문별 1:1 채팅
├─ notification
├─ question · report · review
├─ fraudDetection  # 이상 입찰 결과
├─ fraudLabel      # 관리자 라벨링
├─ recommendation  # 추천 결과 수신
├─ websocket       # STOMP 설정 · 컨트롤러 · Redis Pub/Sub 브리지
├─ outboxEvent · consumedEvent   # Kafka Outbox · 멱등 소비
└─ common          # ai 클라이언트(HMAC) · lock · messaging(KafkaTopics) · 보안 · 예외
```

</details>

<details>
<summary><b>🤖 dib-ai</b></summary>

```
dib-ai
├─ src
│  ├─ serve.py             # FastAPI 엔트리
│  ├─ internal_api/        # 백엔드 ↔ AI HMAC 인증 · 콜백
│  ├─ moderation/          # 금칙어 정규화 · Aho-Corasick · LLM 2차 검수
│  ├─ moderation_api/
│  ├─ fraud/               # 규칙 R1~R6 · Risk Score
│  ├─ fraud_ml/            # LightGBM 학습 · 보정
│  ├─ fraud_api/
│  ├─ reco/                # Base Score · 유사도 · Affinity
│  ├─ reco_api/
│  └─ embedding/           # BGE-M3 · SigLIP 배치
├─ config/                 # rules.yaml · banned_keywords.yaml · reco.yaml
├─ artifacts/              # shill_lgbm_v0.joblib · model_meta.json
├─ notebooks/              # shill_bidding_eda.ipynb
├─ scripts/ · tests/
└─ requirements*.txt
```

</details>

<details>
<summary><b>☁️ dib-infra</b></summary>

```
dib-infra
├─ infra
│  ├─ persistent/          # ECR · S3 (상시)
│  ├─ ephemeral/           # VPC · EKS · RDS · ElastiCache · CloudFront (시연용)
│  └─ k8s/                 # backend · ai · admin-web · kafka · ingress · hpa 매니페스트
├─ scripts/                # bootstrap · build-push · deploy · teardown (PowerShell)
├─ load-test/              # k6 입찰 시나리오
└─ docs/                   # ARCHITECTURE · DEPLOY_DAY · RUNBOOK
```

</details>

<details>
<summary><b>🧩 dib-orchestration</b></summary>

```
dib-orchestration
├─ docker-compose.yml                  # postgres(pgvector) · redis · kafka · backend · ai · admin-web
├─ docker-compose.firebase.local.yml
├─ ARCHITECTURE.md · AGENTS.md
├─ docs
│  ├─ product-specs/                   # system-overview 등 기능 명세
│  ├─ exec-plans/{active,completed}/   # 실행 계획 20건
│  ├─ GIT_FLOW.md · TEST_STRATEGY.md · LOCAL_SETUP.md · DEPLOYMENT_HARNESS.md
│  └─ Initial Schema.sql
├─ scripts/                            # qa-release-harness.ps1
└─ tools/qa-live-dashboard/
```

</details>

---

## 🚀 실행 방법

```bash
cd dib-orchestration
cp .env.example .env                        # HMAC 시크릿 3종은 서로 다른 값으로 채웁니다
docker compose up -d                        # postgres · redis · kafka
docker compose --profile app up -d --build  # + backend(8080) · ai(8000) · admin-web(3000)
```

Android 앱은 `~/.gradle/gradle.properties`에 `DIB_API_BASE_URL`, `DIB_WS_URL`, `DIB_TOSS_CLIENT_KEY`, 카카오 키를 설정한 뒤 `./gradlew :app:assembleDebug`로 빌드합니다. AWS 배포는 `dib-infra/infra/ephemeral`에서 `terraform apply` 후 `scripts/deploy.ps1`을 실행합니다.

---

## 💛 팀원 소개

| <img src="https://github.com/BooGyya.png" width="120"/> | <img src="https://github.com/1lights1.png" width="120"/> | <img src="https://github.com/2hnK.png" width="120"/> | <img src="https://github.com/MJay1123.png" width="120"/> | <img src="https://github.com/dbswjd0191a.png" width="120"/> | <img src="https://github.com/alswjd131313.png" width="120"/> |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **김보경**<br/>([@BooGyya](https://github.com/BooGyya)) | 임세희<br/>([@1lights1](https://github.com/1lights1)) | 김지훈<br/>([@2hnK](https://github.com/2hnK)) | 서민종<br/>([@MJay1123](https://github.com/MJay1123)) | 윤정<br/>([@dbswjd0191a](https://github.com/dbswjd0191a)) | 황민정<br/>([@alswjd131313](https://github.com/alswjd131313)) |
| **Frontend** | Frontend | Backend | Backend | Backend | AI / Data |

---

## 🤝 협업 방식

- **저장소 구성** — 역할별 저장소 4개(frontend · backend · ai · infra)와 이를 서브모듈로 묶는 orchestration 저장소.
- **브랜치 전략** — Git Flow: `main` / `develop` / `feature/*` / `release/*` / `hotfix/*`. feature는 squash merge로 develop에 합칩니다.
- **커밋 컨벤션** — `type(scope): 한글 설명` 형식의 Conventional Commits.
- **문서 주도 개발** — 기능마다 실행 계획을 쓰고 완료 후 `completed/`로 옮기는 방식으로 20건을 진행했고, 각 저장소의 `AGENTS.md`로 코딩 에이전트 컨벤션을 정의했습니다.
- **CI/CD** — GitHub Actions로 AI 서버 테스트와 GitLab 자동 백업을 돌리고, 배포는 Terraform + 스크립트로 시연 당일 생성·철거했습니다.

---

<div align="center">

<img src="docs/assets/character.png" alt="dib 캐릭터" width="160"/>

**dib** — SSAFY 15기 특화 프로젝트 B101 도파민

</div>
