
# ✉️ A Letter to Myself - Microservice Architecture

> 사용자 감정 기반 편지 공유 서비스  
> Monolithic → Microservices with Docker, Kubernetes, GCP

## 📌 서비스 개요

“A Letter to Myself”는 감정 기반 편지 작성·공유 웹 서비스입니다.  
사용자는 편지를 작성하고 미래 열람을 예약할 수 있으며, AI 감정 분석과 루틴 알림을 통해 자신과의 대화를 지속할 수 있습니다.


## 🧩 마이크로서비스 구성

총 12개의 마이크로서비스 + 1개의 인프라 설정 레포로 구성되어 있습니다. 각 서비스는 독립적인 Docker 이미지로 관리되고 Kubernetes 클러스터에서 배포됩니다.

| 번호 | 서비스명 | 설명 |
|------|----------|------|
| 01 | `auth-service` | 사용자 인증 관련 서비스 (JWT 발급/검증) |
| 02 | `user-service` | 사용자 프로필 등록/조회/수정 서비스 |
| 03 | `letter-service` | 편지 작성/열람/삭제 처리 서비스 |
| 04 | `letter-storage-service` | 편지 첨부 이미지 GCS 저장 서비스 |
| 05 | `routine-service` | 사용자 루틴 등록/삭제/조회 서비스 |
| 06 | `scheduler-service` | 루틴 기반 이메일 예약 스케줄러 (Celery Beat/Worker) |
| 07 | `notification-service` | 알림 메일 전송 서비스 (RabbitMQ 소비) |
| 08 | `emotion-ml-service` | 감정 분석 및 통계 처리 AI 서비스 |
| 09 | `emotion-store-service` | 감정 분석 결과 저장 및 제공 서비스 |
| 10 | `recommend-service` | 감정 기반 콘텐츠 추천 서비스 |
| — | `api-gateway` | API 요청 라우팅 및 내부 서비스 통합 |
| — | `web-client` | 정적 웹 프론트엔드 (HTML/CSS/JS) |
| — | `infra` | Kubernetes 설정 및 시크릿/DB/RabbitMQ 설정용 레포 |



## ☁️ 클라우드 인프라

| 구성 요소     | 설명 |
|--------------|------|
| **GCP**           | GKE, GCS 이미지 저장 |
| **PostgreSQL**    | 서비스별 DB Schema 분리 |
| **RabbitMQ**      | 비동기 메시징 (Celery 연동) |
| **Docker Hub**    | 각 서비스 이미지 저장 |



## 🛠 기술 스택

- **Backend**: Python (Django, DRF), Celery
- **Frontend**: HTML, CSS, JS
- **Database**: PostgreSQL
- **Message Queue**: RabbitMQ
- **Containerization**: Docker, Docker Hub
- **Orchestration**: Kubernetes (GKE)
- **DevOps**: GitHub Actions, Secrets, Health Probes


## 🔁 마이크로서비스 통신 구조

- 모든 서비스는 `auth-service`를 통해 JWT 토큰 검증
- `letter-service` ↔ `letter-storage-service` (이미지 저장)
- `routine-service` → `scheduler-service` → `notification-service` (루틴 기반 알림)
- 내부 서비스는 `ClusterIP`, 외부 노출은 `api-gateway`만 `LoadBalancer`


## 🧪 주요 기능

- JWT 기반 사용자 인증 및 관리
- 감정 분석 기반 편지 저장/열람/제한
- 날짜 기반 루틴 등록 및 예약 알림
- 컨테이너 기반 마이크로서비스 + 쿠버네티스 자동 배포


## 🧱 레포지토리 구조

GitHub Organization: [a-letter-to-myself-cc](https://github.com/orgs/a-letter-to-myself-cc/repositories)

- [`auth-service`](https://github.com/a-letter-to-myself-cc/auth-service)
- [`user-service`](https://github.com/a-letter-to-myself-cc/user-service)
- [`letter-service`](https://github.com/a-letter-to-myself-cc/letter-service)
- [`letter-storage-service`](https://github.com/a-letter-to-myself-cc/letter-storage-service)
- [`routine-service`](https://github.com/a-letter-to-myself-cc/routine-service)
- [`scheduler-service`](https://github.com/a-letter-to-myself-cc/scheduler-service)
- [`notification-service`](https://github.com/a-letter-to-myself-cc/notification-service)
- [`emotion-analysis-service`](https://github.com/a-letter-to-myself-cc/emotion-analysis-service)
- [`emotion-store-service`](https://github.com/a-letter-to-myself-cc/emotion-store-service)
- [`emotion-recommendation-service`](https://github.com/a-letter-to-myself-cc/emotion-recommendation-service)
- [`api-gateway`](https://github.com/a-letter-to-myself-cc/api-gateway)
- [`web-client`](https://github.com/a-letter-to-myself-cc/web-client)
- [`k8s-configs`](https://github.com/a-letter-to-myself-cc/k8s-configs) - K8s 통합 배포 설정

---

> ✅ 클라우드, 컨테이너, 마이크로서비스, DevOps 전반을 실습하며 완성한 프로젝트입니다.  
