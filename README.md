# 🤖 MediGPT: AI와 E-Commerce를 결합한 개인 맞춤형 헬스케어 플랫폼

> **"몸이 좋지 않을 때, 어떤 약을 찾아야 할지 막막했던 경험이 있나요?"**
>
> MediGPT는 바로 그 불편함에서 시작되었습니다. 사용자가 자신의 증상을 이야기하면, AI가 전문적인 분석을 통해 가장 적합한 의약품을 추천하고, 결제까지 한 번에 해결하는 원스톱 헬스케어 솔루션입니다.

단순한 쇼핑몰을 넘어, **OpenAI API 연동**, **카카오페이 결제**, **하이브리드 DB 설계** 등 복합적인 기술 과제를 해결하며 서비스의 완성도를 높였습니다.

---

## ✨ 핵심 기능 (Key Features)

| 기능 | 설명 |
| :--- | :--- |
| **🩺 AI 헬스 컨설팅** | GPT-3.5가 사용자의 증상(텍스트/음성)을 분석하여, 예상되는 진단명과 추천 의약품 리스트를 제공합니다. |
| **💊 맞춤형 E-Commerce** | AI 추천 약품을 포함, 다양한 의약품을 검색하고 상세 정보를 확인 후 장바구니에 담을 수 있습니다. |
| **💳 간편 결제 시스템** | `fetch` API를 활용한 비동기 장바구니와 아임포트(I'mport) 연동을 통해 **카카오페이**로 간편하게 결제할 수 있습니다. |
| **🧾 동적 처방전 및 영수증** | AI 상담 결과를 바탕으로 **QR 코드가 포함된 처방전**을 동적으로 생성하고, 결제 완료 시 영수증을 발급합니다. |
| **👤 마이 페이지** | 프로필 관리, AI 진료 기록, 결제 내역 조회 등 개인화된 헬스케어 이력을 관리합니다. |
| **🔐 관리자 페이지** | 관리자만 접근 가능한 페이지에서 의약품 재고 및 정보를 효율적으로 등록하고 관리합니다. |

*(여기에 프로젝트 핵심 기능 GIF 이미지를 추가하면 좋습니다!)*

---

## 🛠️ 기술 스택 (Tech Stack)

- **Backend**: `Java`, `Spring Boot`, `Spring Data JPA`
- **Frontend**: `JSP`, `JSTL`, `Vanilla JavaScript (ES6+)`, `jQuery`, `Bootstrap 5`
- **Database**: **(Hybrid)** `RDBMS` + `Firebase Realtime Database`
- **Infra & Cloud**: `Google Cloud Storage`, `Gradle`
- **External APIs**: `OpenAI (GPT-3.5)`, `KakaoPay (아임포트)`

---

## 🏛️ 아키텍처 (Architecture)

3-Tier 아키텍처를 기반으로, 각 기술 스택이 역할을 분담하여 안정적이고 확장성 있는 구조를 설계했습니다.

- **`Client (Browser)`**
  - **역할**: UI/UX, 사용자 상호작용
  - **기술**: `JSP`, `JavaScript`, `Bootstrap`
  - 💬 `HTTP Requests` ↔ `Server`

- **`Server (Spring Boot)`**
  - **역할**: 비즈니스 로직, API 요청/응답 처리, 데이터 관리
  - **기술**: `Controller`, `Service`, `Repository`
  - 🔄 `API Calls & DB Queries` ↔ `External Services & DB`

- **`External Services & Databases`**
  - **역할**: AI 연산, 결제 처리, 데이터 저장 및 관리
  - **기술**: `OpenAI API`, `KakaoPay API`, `Firebase DB`, `RDBMS`, `GCS`

---

## 🚀 주요 도전 과제 및 해결 기록 (Key Challenges & Solutions)

### 1. Firebase 비동기 데이터 처리의 함정 극복기

- **문제 상황**:
  Firebase의 비동기적 특성 때문에, DB에서 데이터를 미처 다 받지 못한 상태에서 다음 코드가 실행되어 `NullPointerException`이 발생하고 데이터 정합성이 깨지는 문제가 있었습니다.

- **해결 전략**:
  Java의 동시성 처리 유틸리티인 **`CountDownLatch`**를 도입하여 비동기 흐름을 제어했습니다.
  1. Firebase에 데이터 요청 직전, `CountDownLatch`를 `1`로 초기화합니다.
  2. 메인 스레드는 `latch.await()`를 호출하여 대기 상태에 들어갑니다.
  3. Firebase의 `onDataChange` 콜백 (데이터 수신 완료 시점)에서 `latch.countDown()`을 호출합니다.
  4. Latch의 카운트가 `0`이 되면, 대기하던 메인 스레드가 다시 실행됩니다.

- **결과**:
  **비동기 코드를 안정적인 동기식 흐름처럼 제어**하는 데 성공하여, 데이터의 정합성과 서비스 안정성을 모두 확보했습니다.

### 2. 데이터 특성에 맞는 최적의 '하이브리드 DB' 설계

- **문제 상황**:
  실시간 동기화가 중요한 '장바구니/진단 기록'과 트랜잭션 관리가 중요한 '회원/게시글 정보'가 공존하여, 단일 DB로는 최적의 성능을 내기 어려웠습니다.

- **해결 전략**:
  데이터의 성격에 따라 저장소를 분리하는 **하이브리드 방식**을 채택했습니다.
  - **`Firebase Realtime DB`**: 빠른 응답과 실시간 동기화가 필수적인 **장바구니, 진단/결제 내역** 등을 담당합니다.
  - **`RDBMS` (with JPA)**: 정합성과 트랜잭션 관리가 중요한 **회원 정보, 게시판** 등 정형 데이터를 담당합니다.

- **결과**:
  각 데이터의 특성에 맞는 DB를 선택적으로 사용하여, **빠른 성능과 데이터 안정성**이라는 두 마리 토끼를 모두 잡는 효율적인 시스템을 구축했습니다.
