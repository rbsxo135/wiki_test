# API Implementation & Verification Status

> API 명세를 기준으로 각 Endpoint의 구현 및 검증 상태를 관리한다.
>
> - **Implemented**: Backend API 구현이 완료되었는지 확인
> - **Verified**: 실제 API 호출을 통해 명세대로 동작하는지 확인
> - `⬜`: 미완료
> - `☑️`: 완료

---

## 🔐 1. 인증 및 회원 서비스 (User & Auth)

| Method | Endpoint | Description | Auth | Implemented | Verified |
|---|---|---|---|:---:|:---:|
| `POST` | `/auth/signup` | 사용자 회원가입 | - | ⬜ | ⬜ |
| `POST` | `/auth/login` | 사용자/관리자 로그인 (JWT 및 쿠키 발급) | - | ⬜ | ⬜ |
| `POST` | `/auth/refresh` | Refresh Token으로 Access Token 갱신 | - | ⬜ | ⬜ |
| `GET` | `/users/{userId}` | 회원 정보 조회 | User | ⬜ | ⬜ |
| `PUT` | `/users/{userId}` | 회원 정보 수정 | User | ⬜ | ⬜ |
| `DELETE` | `/users/{userId}` | 회원 정보 삭제 | User | ⬜ | ⬜ |

---

## 📅 2. 카탈로그 서비스 (Catalog)

| Method | Endpoint | Description | Auth | Implemented | Verified |
|---|---|---|---|:---:|:---:|
| `GET` | `/hotels/{hotelId}` | 호텔 기본 정보 및 부대시설 조회 | - | ⬜ | ⬜ |
| `GET` | `/hotels/{hotelId}/rules` | 호텔/객실 이용 규정 및 환불 규정 조회 | - | ⬜ | ⬜ |
| `POST` | `/hotels/{hotelId}/rooms` | 신규 객실 정보 및 요일별 요금 등록 | Admin | ⬜ | ⬜ |
| `PUT` | `/hotels/{hotelId}/rooms/{roomId}` | 기존 객실 정보 및 요금 수정 | Admin | ⬜ | ⬜ |
| `GET` | `/hotels/{hotelId}/roomSearch` | 객실 목록 및 요일별 기본 요금 조회 | - | ⬜ | ⬜ |
| `GET` | `/hotels/{hotelId}/rooms/{roomId}/availability` | 특정 기간(`?startDate=&endDate=`) 예약 가능 여부 조회 | - | ⬜ | ⬜ |

---

## 💳 3. 예약 및 결제 서비스 (Booking & Payment)

| Method | Endpoint | Description | Auth | Implemented | Verified |
|---|---|---|---|:---:|:---:|
| `POST` | `/bookings` | 객실 선점 및 예약 생성 (결제 대기 상태) | User | ⬜ | ⬜ |
| `GET` | `/bookings/{userId}` | 특정 유저 예약 내역 목록 조회 | User | ⬜ | ⬜ |
| `GET` | `/bookings/{bookingId}` | 예약 상세 정보 조회 | User | ⬜ | ⬜ |
| `PUT` | `/bookings/{bookingId}` | 예약 취소 (내부적으로 환불 프로세스 호출) | User | ⬜ | ⬜ |
| `POST` | `/payments` | 특정 예약 건에 대한 결제 승인 요청 | User | ⬜ | ⬜ |
| `POST` | `/payments/webhook` | PG사 서버 측 결제 결과 통보 (비동기 콜백) | PG사 | ⬜ | ⬜ |

---

## 🤖 4. 챗봇 및 알림 서비스 (Chatbot & Notification)

| Method | Endpoint | Description | Auth | Implemented | Verified |
|---|---|---|---|:---:|:---:|
| `POST` | `/chat/sessions` | 새로운 챗봇 대화 세션 생성 | - | ⬜ | ⬜ |
| `POST` | `/chat/sessions/{sessionId}/messages` | 챗봇 질문 전송 및 답변 수신 (Streaming/REST) | - | ⬜ | ⬜ |
| `GET` | `/notices` | 호텔 전체 공지사항 목록 조회 | - | ⬜ | ⬜ |

---

## 🛠️ 5. 백오피스 관리자 서비스 (Admin)

| Method | Endpoint | Description | Auth | Implemented | Verified |
|---|---|---|---|:---:|:---:|
| `POST` | `/notices` | 공지사항 생성 및 등록 | Admin | ⬜ | ⬜ |
| `PUT` | `/notices/{noticeId}` | 등록된 공지사항 수정 | Admin | ⬜ | ⬜ |

---

## ☸️ 6. 인프라 헬스 체크 (AWS EKS Probes)

| Method | Endpoint | Description | Auth | Implemented | Verified |
|---|---|---|---|:---:|:---:|
| `GET` | `/health/liveness` | Application 데드락 상태 확인 (Liveness Probe) | - | ⬜ | ⬜ |
| `GET` | `/health/readiness` | DB/Redis 연결 등 트래픽 수신 가능 상태 확인 (Readiness Probe) | - | ⬜ | ⬜ |

---

## Verification Criteria

### Implemented

`Implemented`는 다음 조건을 만족할 경우 체크한다.

- [ ] Endpoint가 Backend에 구현되어 있다.
- [ ] HTTP Method와 Endpoint가 API 명세와 일치한다.
- [ ] Authentication / Authorization이 명세에 맞게 적용되어 있다.
- [ ] Request 및 Response 구조가 정의되어 있다.
- [ ] 주요 예외 상황에 대한 처리가 구현되어 있다.

### Verified

`Verified`는 실제 API 호출을 통해 다음 항목을 확인한 경우 체크한다.

- [ ] 정상적인 Request에 대해 예상한 Response가 반환된다.
- [ ] HTTP Status Code가 명세와 일치한다.
- [ ] Response Body가 명세와 일치한다.
- [ ] Authentication / Authorization이 정상적으로 동작한다.
- [ ] 잘못된 Request에 대한 Error Response가 정상적으로 반환된다.
- [ ] 주요 Edge Case가 정상적으로 처리된다.