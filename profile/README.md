<h1 align="center">Slack Weather & Schedule Bot</h1>

<p align="center">
  <b>매일 아침 날씨·강의 시간표·Google Calendar 일정을 Slack으로 자동 전달하는 봇</b><br/>
  슬래시 커맨드로 개인 일정·알림을 관리하고, 강의 시간표는 ICS 캘린더로 내보냅니다
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/Slack-4A154B?style=for-the-badge&logo=slack&logoColor=white" alt="Slack"/>
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI"/>
  <img src="https://img.shields.io/badge/APScheduler-2E7D32?style=for-the-badge" alt="APScheduler"/>
  <img src="https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL"/>
  <img src="https://img.shields.io/badge/Google%20Calendar-4285F4?style=for-the-badge&logo=googlecalendar&logoColor=white" alt="Google Calendar"/>
</p>

<p align="center">
  <a href="https://github.com/se-slackbot/slackbot">Repository</a>
</p>

---

## 프로젝트 소개

- **날씨·강의 일정·Google Calendar**를 하나의 데일리 브리프로 묶어 매일 지정 시각에 Slack으로 전송
- **슬래시 커맨드**로 즉시 조회와 개인 시간표·알림 설정 관리, 강의 시간표는 **ICS로 내보내** 외부 캘린더에서 구독

## 팀 구성

<table>
  <tr><td align="center"><a href="https://github.com/ken-jeong"><img src="https://github.com/ken-jeong.png" width="60px" alt="정상겸"/></a></td><td><b>정상겸</b><br/><sub>Leader · Backend</sub></td><td>프로젝트 설계 · 시간표 기능 · 사용자 설정 · 테스트 · 문서</td></tr>
  <tr><td align="center"><a href="https://github.com/eyes25"><img src="https://github.com/eyes25.png" width="60px" alt="김준서"/></a></td><td><b>김준서</b><br/><sub>Backend · Infra</sub></td><td>스케줄러 · Google Calendar 연동 · DB · 배포</td></tr>
  <tr><td align="center"><a href="https://github.com/Rustica0411"><img src="https://github.com/Rustica0411.png" width="60px" alt="안현빈"/></a></td><td><b>안현빈</b><br/><sub>Backend</sub></td><td>Slack App 연동 · 슬래시 커맨드 · Block Kit 메시지</td></tr>
</table>

## 시스템 아키텍처

```text
Slack Slash Command ─┐
APScheduler ─────────┼──▶ main.py ──▶ weather · schedule · google_calendar
FastAPI (ICS API) ───┘                      │
                                            ▼
                                  SQLite / PostgreSQL
```

`main.py`가 Slack Bolt 앱, APScheduler 스케줄러, FastAPI ICS API를 함께 구동합니다. `SLACK_APP_TOKEN`이 있으면 Socket Mode, 없으면 HTTP 모드로 동작합니다.

## 핵심 기능

| 기능 | 설명 |
|------|------|
| 데일리 브리프 | 지정 시각에 날씨·강의 일정·Google Calendar를 Slack 채널/DM으로 자동 전송 |
| 슬래시 커맨드 | `/weather` `/schedule` `/config` `/brief` 로 즉시 조회·설정 (한글 별칭 지원) |
| 개인 시간표 관리 | Slack 사용자별 강의 일정 추가·수정·삭제, ICS 내보내기로 외부 캘린더 구독 |
| Google Calendar 연동 | 사용자별 OAuth 토큰으로 오늘 일정을 브리프에 포함 |
| 장애 대응 | 날씨 API 캐시 폴백, Slack 전송 재시도(최대 3회), 오류 알림 |