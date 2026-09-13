<div align="center">

# 단짝 (Danjjak)

**가족이 도와드리는 똑똑한 금융 생활**

고령 사용자가 자주 이용하는 금융 업무를 단축번호와 음성 안내로 쉽게 수행하는 해커톤 MVP

[![Vue](https://img.shields.io/badge/Vue-3.5-42b883)](frontend/package.json)
[![Spring Framework](https://img.shields.io/badge/Spring%20Framework-5.3-6db33f)](backend/build.gradle)
[![Java](https://img.shields.io/badge/Java-17-orange)](backend/build.gradle)
[![Tomcat](https://img.shields.io/badge/Tomcat-9-f8dc75)](backend/README.md)
[![OpenAPI](https://img.shields.io/badge/OpenAPI-3.0-6ba539)](contracts/openapi.yaml)

2026 KB IT's Your Life 해커톤 · 팀 '단짝친구' (5인)

**본 저장소 작성자는 사용자·단축번호 도메인을 Full-stack으로 담당했습니다**

</div>

---

## 📝 프로젝트 소개

전화기의 **단축번호**를 금융 앱에 옮겨온 서비스입니다.

고령 사용자에게 금융 앱이 어려운 이유는 기능이 없어서가 아니라, **매번 같은 일을 하는데도 매번 처음처럼 길을 찾아야 하기 때문**입니다. 단짝은 가족이 미리 금융 패턴을 등록해두면, 시니어가 **번호 하나로 실행**할 수 있게 합니다.

- 자주 사용하는 금융 업무를 단축번호로 등록
- TTS와 화면 하이라이트로 금융 절차 단계별 안내
- Mock 계좌와 거래 데이터로 송금 흐름 시연
- 메인 송금 기능에서 Rule-based FDS 시연
- 사용 로그를 기반으로 간단한 이용 분석 제공

---

## 👤 담당 범위

**5인 팀**이며, 저는 **사용자·단축번호 도메인 전체(FE·BE·DB)** 를 맡았습니다.
화면 마크업은 UI 담당이 만들고, 거기에 API·DB·로직을 연결하는 구조였습니다.

| 영역 | 구현 내용 |
|---|---|
| **단축번호** | 기본 8개 시드 · 최대 12개 등록 · 추가/수정/삭제 · 번호 이동·교환·비활성화 |
| **금융 패턴** | 패턴 템플릿, 단계별 문구 저장, 실행 전 확인 화면과 진입 흐름 |
| **모의 계좌·인물** | 복수 받는 계좌 연결, 기존 인물의 추가 계좌, 모의 본인 계좌 불러오기, 연속 송금 단계 |
| **접근성** | 428×926 소형 화면 대응, 큰 글씨, 모바일 키보드·안전 영역, 동의·접근성 설정 |
| **로컬 인프라** | Docker Compose(MySQL) + Flyway 마이그레이션 구조 초기 구성 |
| **API 계약** | 사용자 설정·금융 패턴 OpenAPI 계약 작성 |
| **표기 일관성** | 금액 숫자·한글 병기, 'AI 음성' 호칭 통일 |

> 로그인·인증, 송금·FDS 코어, TTS·행동 로그는 다른 팀원이 담당했습니다.

---

## ✨ MVP 구현 범위

### 🔢 금융 단축번호

- 자주 사용하는 금융 업무 등록 및 실행
- 단축번호 순서 변경
- 미구현 기능은 비활성 버튼으로 표시

### 💸 안내형 송금

- 출금 계좌 선택 → 수취 계좌 선택 또는 입력 → 금액 입력 → 비밀번호 확인 → Mock 거래 완료

### 🔊 음성 안내

- 단계별 TTS 안내와 선택할 UI 요소 하이라이트
- 안내 문구 저장 및 조회
- 시작·단계별 가족 음성 녹음·교체와 TTS 대체
- 문구 비교 후 명시적 적용 및 해당 단계 재녹음 연결

### 🚨 이상거래 탐지

- 등록 수취인·직접 입력 송금에 동일하게 적용
- 1천만원 이상 고액 및 최근 10분 내 완료 송금 반복 규칙
- 위험 사유 표시, 보호자 전화와 HIGH 위험 카카오 알림 연결

### 📊 이용 분석

- 단계별 소요시간, 도움 요청·재시도 기록
- 어려움을 겪은 단계 요약

---

## 🧭 주요 시연 흐름

초기 시드·FDS 재현·검증 결과는 [반복 실행 및 최종 검증](docs/demo-verification.md)을 참고하세요.

**정상 송금**

```text
홈 -> 단축번호 선택 -> 음성 안내 -> 송금 단계 진행 -> 완료
```

**이상 송금**

```text
홈 -> 송금하기 -> 계좌와 금액 입력 -> FDS 판단 -> 재확인 또는 보호자 확인
```

---

## 🛠 기술 스택

| 영역 | 기술 |
| --- | --- |
| Frontend | Vue 3 · Vite · JavaScript |
| Backend | Java 17 · Spring Framework 5.3 · Spring MVC · MyBatis |
| Runtime | Gradle WAR · Apache Tomcat 9 |
| Database | MySQL 8.4 · Flyway |
| Local environment | Docker Compose |
| API contract | OpenAPI 3.0 · Redocly |
| Logging | Log4j2 |

> Spring Boot가 아닌 **Spring Framework 5.3 + WAR** 구성입니다. `WebAppInitializer`로 서블릿 컨텍스트를 직접 등록하고, 트랜잭션 매니저와 데이터소스를 `RootConfig`에서 수동 구성했습니다.

## 🏗 시스템 구조

```text
Vue frontend
    -> Tomcat 9
        -> Spring MVC controller
            -> Service
                -> MyBatis mapper
                    -> MySQL
```

---

## 🚀 시작하기

```powershell
git clone https://github.com/jjuny0326/danjjak-app.git
cd danjjak-app
```

로컬 개발은 데이터베이스를 먼저 띄우고 시작합니다. **DB와 Flyway 실행에는 Docker Desktop만 있으면 됩니다.**
프론트엔드는 Node.js, 백엔드는 JDK 17과 Tomcat 9가 별도로 필요합니다.

```powershell
cd infra
Copy-Item .env.example .env   # 최초 1회, MYSQL_ROOT_PASSWORD 만 채우면 됩니다
docker compose up -d ; docker compose logs flyway --tail 20
```

| 항목 | 안내 |
| --- | --- |
| Database · 로컬 환경 | [infra/README.md](infra/README.md) |
| DB 스키마 · 마이그레이션 | [db/README.md](db/README.md) |
| Frontend | [frontend/README.md](frontend/README.md) |
| Backend | [backend/README.md](backend/README.md) |
| API contract | [contracts/openapi.yaml](contracts/openapi.yaml) |

---

## 🌐 API 명세

- 원본: [`contracts/openapi.yaml`](contracts/openapi.yaml)
- API 변경 시 명세와 구현을 함께 수정
- 현재 제공: 인증·사용자 설정, 계좌·거래 조회, 등록 인물, 패턴·실행 로그, Mock 송금·FDS, 보호자 연락처·카카오 알림, TTS API

## 🤝 협업 규칙

- 커밋 및 코드 컨벤션: [`CONTRIBUTING.md`](CONTRIBUTING.md)
- 브랜치 규칙: `<type>/<issue-number>-<summary>`
- 이슈 기반 진행 — 기능 단위로 이슈를 만들고 PR로 병합

---

<div align="center">

**단짝 (Danjjak)** · 2026 KB IT's Your Life 해커톤

<sub>Mock 금융 데이터로 구현하는 시연용 프로젝트입니다. 실제 금융망과 연결되지 않습니다.</sub>

</div>
