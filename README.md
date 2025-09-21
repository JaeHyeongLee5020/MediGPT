# 🤖 MediGPT: AI와 E-Commerce를 결합한 개인 맞춤형 헬스케어 플랫폼

> **"몸이 좋지 않을 때, 어떤 약을 찾아야 할지 막막했던 경험이 있나요?"**
>
> MediGPT는 바로 그 불편함에서 시작되었습니다. 사용자가 자신의 증상을 이야기하면, AI가 분석을 통해 가장 적합한 의약품을 추천하고, 결제까지 한 번에 해결하는 원스톱 헬스케어 솔루션입니다.

단순한 쇼핑몰을 넘어, **OpenAI API 연동**, **KakaoPay 결제**, **하이브리드 DB 설계**, **클라우드 스토리지 업로드** 등 복합적인 기술 과제를 해결하며 서비스의 완성도를 높였습니다.

---

## ✨ 핵심 기능 (Key Features)

| 기능 | 설명 |
| :--- | :--- |
| **🩺 AI 헬스 컨설팅** | OpenAI API가 사용자의 증상(텍스트/음성)을 분석하여 예상 진단명과 추천 의약품 리스트를 제공합니다. |
| **💊 맞춤형 E-Commerce** | AI 추천 약품을 포함, 다양한 의약품을 검색하고 상세 정보를 확인 후 장바구니에 담을 수 있습니다. |
| **💳 간편 결제 시스템** | `fetch` API 기반 비동기 장바구니와 **KakaoPay REST API** 연동을 통해 간편하게 결제할 수 있습니다. |
| **🧾 QR 처방전 및 영수증 발급** | AI 상담 결과로 **QR 코드 포함 처방전**을 동적으로 생성하고, 결제 완료 시 영수증을 제공합니다. |
| **🖼️ 이미지 업로드 파이프라인** | Google Cloud Storage(GCS)와 로컬 저장소를 활용해 게시글·프로필 이미지를 업로드 및 관리합니다. |
| **👤 마이 페이지** | 프로필 관리, AI 진료 기록, 결제 내역 조회 등 개인화된 헬스케어 이력을 관리합니다. |
| **🔐 관리자 페이지** | 관리자 전용 페이지에서 의약품 재고 및 정보를 효율적으로 등록·관리할 수 있습니다. |

---

## 🛠️ 기술 스택 (Tech Stack)

- **Backend**: `Java 17`, `Spring Boot`, `Spring Data JPA`
- **Frontend**: `JSP`, `JSTL`, `jQuery`, `Bootstrap 5`
- **Database**: **Hybrid** → `Oracle DB (JPA)` + `Firebase Realtime Database`
- **Infra & Cloud**: `Google Cloud Storage`, `Gradle`
- **External APIs**: `OpenAI API`, `KakaoPay REST API`

---

## 🏛️ 아키텍처 (Architecture)

3-Tier 아키텍처 기반으로 안정성과 확장성을 확보했습니다.

- **Client (Browser)**  
  - `JSP`, `jQuery`, `Bootstrap` 기반 UI/UX  
  - 💬 사용자 입력 → HTTP 요청 전송

- **Server (Spring Boot)**  
  - `Controller`, `Service`, `Repository` 구조  
  - 🔄 AI API 호출, DB 연동, 결제 처리 담당

- **External Services & DB**  
  - `OpenAI API` (AI 상담)  
  - `KakaoPay API` (결제)  
  - `Firebase Realtime DB` (장바구니, 진단 기록, 결제 내역)  
  - `Oracle DB` (회원, 게시글, 정형 데이터)  
  - `Google Cloud Storage` (이미지 업로드)

---

## 🚀 주요 도전 과제 및 해결 (Key Challenges & Solutions)

### 1. Firebase 비동기 데이터 처리
- **문제**: Firebase의 비동기 특성으로 `NullPointerException`, 데이터 정합성 문제 발생  
- **해결**: `CountDownLatch`를 적용해 콜백 완료 시점까지 대기 → **동기식 흐름 확보**

### 2. 하이브리드 DB 설계
- **문제**: 실시간 동기화(장바구니·진단 기록)와 트랜잭션 관리(회원·게시판)가 공존  
- **해결**: **Firebase**(실시간 데이터) + **Oracle JPA**(정형 데이터) 병행 → 성능과 안정성 모두 확보

### 3. 외부 AI 연동 안정화
- **문제**: ChatGPT API 호출 지연·실패 시 사용자 경험 저하  
- **해결**: 예외 처리 및 fallback 메시지 반환 → 안정적 응답 경험 제공

### 4. 이미지 업로드와 보안
- **문제**: GCS 업로드 시 파일명 충돌·보안 키 노출 위험  
- **해결**: 타임스탬프 기반 파일명 부여, 서비스 계정 키를 환경 변수로 관리하도록 개선 계획 수립

---

## 📌 얻은 성과와 역량
- **복합 데이터 저장소 아키텍처 설계 경험** (RDBMS + NoSQL 혼합)  
- **외부 API 안정화 패턴 학습** (AI/결제)  
- **클라우드 스토리지 활용 경험** (대용량 파일 업로드 파이프라인)  
- **실시간 데이터 처리와 트랜잭션 보장 간 균형 잡기**

---
