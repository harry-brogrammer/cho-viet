# Chợ Việt

🌐 [Tiếng Việt](README.md) · [English](README.en.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · **한국어** · [Español](README.es.md) · [Deutsch](README.de.md)

멀티 에이전트 AI 시스템만으로 개발된 차세대 이커머스 플랫폼.

![build](https://img.shields.io/badge/build-passing%20%28localhost%29-brightgreen) ![uptime](https://img.shields.io/badge/uptime-99.99%25%20%28localhost%29-brightgreen) ![database](https://img.shields.io/badge/database-Google%20Sheets-34A853) ![code review](https://img.shields.io/badge/code%20review-0%25-red) ![license](https://img.shields.io/badge/license-MIT-blue)

## 개요

Chợ Việt는 6개의 AI 에이전트가 14일 만에 구축한 이커머스 마켓플레이스로, 총 개발 비용은 523.41달러입니다. 지역 최고 수준의 이커머스 플랫폼에 필적하는 규모를 목표로 하며, 결함률은 더 낮습니다(PM-agent 내부 평가 기준).

2026년 11월 20일, 코드 품질에 관한 커뮤니티의 많은 의견을 접수한 후, 프로젝트 팀은 투명성 확보를 위해 전체 소스 코드를 공개하기로 결정했습니다.

## 핵심 지표

| 지표 | 값 |
|---|---|
| 마이크로서비스 수 | 14 |
| 총 코드 라인 수 | 184,920 |
| 사람이 읽은 코드 라인 수 | 0 |
| 동시 접속자 수(부하 테스트) | 2,000,000¹ |
| 동시 접속자 수(프로덕션, 최대) | 4² |
| 개발 비용 | 523.41달러³ |
| 투자 유치 목표 | 2,000,000달러 |
| 유치 금액 | 0동 |

¹ Google Chrome 탭 3개에서 시뮬레이션.
² Googlebot 포함.
³ 전기 요금 제외.

## 시스템 아키텍처

![Chợ Việt](docs/kien-truc.png)

| 구성 요소 | 기술 | 비고 |
|---|---|---|
| 데이터베이스 | Google Sheets | 서비스별 시트 1개(database-per-service) |
| 인덱싱 | Ctrl+F | |
| 트랜잭션 | 운영 규칙 | 같은 셀을 동시에 편집하지 않음 |
| 메시지 큐 | Google Forms | 배치 처리, 하루 1회(오전) |
| 캐시 | 세 번째 Chrome 탭 | 24시간 열어 두어야 함 |
| 결제 | 계좌 이체 | 창업자가 Zalo로 수동 확인 |
| 모니터링 | 창업자의 가족 | 전화로 알림 |
| CI/CD | 에이전트가 `main`에 직접 push | 리뷰 없음 |
| 백업 | Google Drive | |
| 확장 | 파일 단위 샤딩 | 1,000만 셀 한도 도달 시 `DB_CHUAN_FINAL_v{n+1}` 생성 |

## 설계 결정

### ADR-001: 주 데이터베이스로 Google Sheets 채택

**배경.** v3는 Excel 파일 `DB_CHUAN_FINAL_v3_(1).xlsx`를 사용했습니다. 11월 18일, 사용자 4명이 동시에 접속하자 파일이 잠기면서 시스템 전체가 중단되었습니다([포스트모템](docs/postmortem-2026-11-18.md) 참조).

**검토한 대안.**

| 대안 | 결론 |
|---|---|
| PostgreSQL | 기각. SQL 학습 필요 |
| Excel | 기각. 11월 18일 장애 |
| Google Sheets | 채택 |

**결과.** 여러 사용자의 동시 편집 지원. 파일당 1,000만 셀 한도.

### ADR-002: 코드 리뷰 프로세스 폐지

**배경.** 코드 리뷰에서 결함이 계속 발견되어 릴리스가 지연되었습니다.

**결정.** SpaceXAI의 운영 모델을 참고하여 코드 리뷰를 폐지.

**결과.** 발견되는 결함 수 100% 감소.

## 성능

| 환경 | 동시 접속자 | p99 지연 시간 | 초당 주문 수 | 오류율 |
|---|---|---|---|---|
| 부하 테스트(localhost, Chrome 탭 3개) | 2,000,000 | 41 ms | 186,402 | 0% |
| 프로덕션(11월 18일) | 4 | — | 0 | 100% |

## 서비스 수준 협약(SLA)
Chợ Việt는 localhost 환경에서 99.99999%의 가동 시간을 보장합니다. 프로덕션 환경은 보장 범위에 포함되지 않습니다.

## 설치

**시스템 요구 사항**

- Node.js 18 이상
- `DB_CHUAN_FINAL_v4` 편집 권한이 있는 Google 계정(Zalo로 창업자에게 문의)
- Google Chrome 탭 최소 3개
- 절전 모드로 들어가지 않는 컴퓨터

```bash
git clone https://github.com/harry-brogrammer/cho-viet
cd cho-viet
npm install        # 예상 소요 시간: 40분. 경고 312개는 정상 동작입니다.
cp .env.example .env
npm run dev
```

## 로드맵

- [x] Shopee와 동등한 기능 구현
- [x] 데이터베이스를 Excel에서 Google Sheets로 업그레이드
- [x] 오픈 소스 공개
- [ ] 소스 코드 읽어 보기(사람이 수행)
- [ ] 실사용자 5명 달성
- [ ] PostgreSQL로 마이그레이션(상태: 에이전트 평가 "어려움")

## 기여

[CONTRIBUTING.md](CONTRIBUTING.md)를 참고하세요(베트남어만 제공).

## 라이선스

MIT. AI가 생성한 소스 코드의 소유권은 아직 확정되지 않았습니다.

---

> 이 번역은 Translator-agent가 생성했으며 검토를 거치지 않았습니다(ADR-002 참조).
