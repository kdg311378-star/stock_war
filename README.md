# 📈 STOCK WAR: Real-time Stock Simulation Platform
> **Firebase 실시간 데이터베이스와 QR 시스템을 결합한 몰입형 주식 투자 시뮬레이션 게임**

![Tech Stack](https://img.shields.io/badge/Stack-HTML5%20%7C%20TailwindCSS%20%7C%20Firebase-orange)
![License](https://img.shields.io/badge/License-MIT-blue)
![Live Demo](https://img.shields.io/badge/Demo-Live%20Now-brightgreen)
[Live Demo - 유저용](https://[kdg311378-star].github.io/STOCK_WAR/index.html)

[Admin Panel - 관리자용](https://[kdg311378-star].github.io/STOCK_WAR/admin.html)

## 1. 프로젝트 개요
* **프로젝트 명칭:** STOCK WAR (스톡 워)
* **목적:** 오프라인 행사 및 교육 현장에서 다수의 사용자가 실시간 주가 변동 정보를 확인하고 투자에 참여하는 시뮬레이션 플랫폼.
* **핵심 가치:** 0.1초 단위의 실시간 데이터 동기화, 사용자 중심의 예외 처리, 강력한 관리자 중앙 관제.

---

## 2. 주요 기능 (Core Features)

### 🕹️ 사용자 터미널 (User Terminal - Mobile)
* **전략적 투자:** 보유 자산 내에서 기업별 투자 금액을 설정하고 포지션을 확정합니다.
* **QR 기반 정보 획득:** 오프라인에 배치된 QR 코드를 스캔하여 라운드당 **단 1개**의 기업 기밀 정보를 코인으로 구매할 수 있습니다.
* **상태 유지 및 세션 복구:** 네트워크 끊김이나 브라우저 종료 시에도 `localStorage`와 Firebase 동기화를 통해 **기존 투자 상태와 자산을 즉시 복구**합니다.
* **라운드 정산 시스템:** 게임의 몰입감을 위해 관리자가 다음 라운드를 활성화한 시점에만 자산 변동 내역을 확인할 수 있습니다.

### 🖥️ 마스터 패널 (Admin Dashboard - Web)
* **실시간 유저 모니터링:** 모든 참가자의 자산, 코인, 투자 확정 여부 및 준비 상태를 한눈에 파악합니다.
* **운영 지원 기능:** * 유저가 실수로 확정 버튼을 누른 경우 원격으로 **[확정 해제]** 지원.
    * 잘못 입력된 투자 금액을 관리자가 직접 수정하거나 추가 코인을 지급하여 원활한 게임 진행 도모.
    * 비정상 행위 유저 실시간 **[추방(Kick)]** 및 데이터 **[전체 리셋]** 기능.
* **보안 토큰 인증:** Firebase Security Rules와 연동된 `Admin Token` 방식을 적용하여 비권한자의 데이터 조작을 차단합니다.

---

## 3. 기술적 해결 (Technical Highlights)

### ✅ 데이터 무결성 및 동시성 제어
* **문제:** 다수의 사용자가 동시에 코인을 소모하거나 자산을 수정할 때 발생하는 Race Condition 문제.
* **해결:** Firebase의 `transaction()` API를 사용하여 데이터 업데이트의 **원자성(Atomicity)**을 확보, 자산 오차 발생률을 0%로 유지했습니다.

### ✅ 서버리스 환경의 보안 설계
* **문제:** 별도의 백엔드 서버 없이 클라이언트 코드만으로 DB 주소가 노출될 경우 보안 취약점 발생.
* **해결:** Firebase Security Rules에 `newData.child('adminToken')` 검증 로직을 설계하여, 특정 토큰이 포함된 관리자 패널의 요청만 수용하도록 보안을 강화했습니다.

### ✅ 비동기 상태 동기화 및 세션 보존
* **문제:** 모바일 웹 환경 특성상 잦은 페이지 이탈 시 데이터 초기화 우려.
* **해결:** 사용자의 마지막 활동 노드(Node) 정보를 `localStorage`에 캐싱하고, 페이지 로드 시 Firebase 실시간 리스너를 통해 데이터 일관성을 유지하는 복구 알고리즘을 구현했습니다.

---

## 4. 데이터 아키텍처 (Data Structure)

```json
{
  "rankings": {
    "팀이름": {
      "assets": 10000000,      // 현재 자산
      "coins": 3,              // 보유 코인
      "isLocked": true,        // 투자 확정 여부 (관리자가 해제 가능)
      "isWaiting": false,      // 정산 대기 상태
      "investments": {         // 기업별 투자액
        "BAUL": 5000000,
        "SAMSON": 2000000
      }
    }
  },
  "game_settings": {
    "currentRound": 1,        // 전역 라운드 정보
    "adminToken": "stock_war_77" // 보안 인증 토큰
  }
}

---

###  5. 기술 스택(Tech Stack)

Category,Tech
Frontend,"HTML5, JavaScript (ES6+), Tailwind CSS"
Backend,Firebase Realtime Database (BaaS)
Library,Html5-Qrcode (QR Scanner)
Deployment,GitHub Pages
