# API Implementation & Verification Status

> API 명세를 기준으로 각 Endpoint의 구현 및 검증 상태를 관리한다.
>
> - **Implemented**: Backend API 구현이 완료되었는지 확인
> - **Verified**: 실제 API 호출을 통해 명세대로 동작하는지 확인
>
> API의 구현 및 검증이 완료된 경우 해당 Endpoint의 상태를 별도로 기록하거나 관련 문서 및 테스트 결과를 함께 관리한다.

---

## 🔐 1. 인증 및 회원 서비스 (User & Auth)

| Method | Endpoint | Description | Auth |
| ------ | -------- | ----------- | ---- |
| `POST` | `/auth/signup` | 사용자 회원가입 | - |
| `POST` | `/auth/login` | 사용자/관리자 로그인 (JWT 및 쿠키 발급) | - |
| `POST` | `/auth/refresh` | Refresh Token으로 Access Token 갱신 | - |
| `GET` | `/users/{userId}` | 회원 정보 조회 | User |
| `PUT` | `/users/{userId}` | 회원 정보 수정 | User |
| `DELETE` | `/users/{userId}` | 회원 정보 삭제 | User |

---

## 📅 2. 카탈로그 서비스 (Catalog)

| Method | Endpoint | Description | Auth |
| ------ | -------- | ----------- | ----- |
| `GET` | `/hotels/{hotelId}` | 호텔 기본 정보 및 부대시설 조회 | - |
| `GET` | `/hotels/{hotelId}/rules` | 호텔/객실 이용 규정 및 환불 규정 조회 | - |
| `POST` | `/hotels/{hotelId}/rooms` | 신규 객실 정보 및 요일별 요금 등록 | Admin |
| `PUT` | `/hotels/{hotelId}/rooms/{roomId}` | 기존 객실 정보 및 요금 수정 | Admin |
| `GET` | `/hotels/{hotelId}/roomSearch` | 객실 목록 및 요일별 기본 요금 조회 | - |
| `GET` | `/hotels/{hotelId}/rooms/{roomId}/availability` | 특정 기간(`?startDate=&endDate=`) 예약 가능 여부 조회 | - |

---

## 💳 3. 예약 및 결제 서비스 (Booking & Payment)

| Method | Endpoint | Description | Auth |
| ------ | -------- | ----------- | ---- |
| `POST` | `/bookings` | 객실 선점 및 예약 생성 (결제 대기 상태) | User |
| `GET` | `/bookings/{userId}` | 특정 유저 예약 내역 목록 조회 | User |
| `GET` | `/bookings/{bookingId}` | 예약 상세 정보 조회 | User |
| `PUT` | `/bookings/{bookingId}` | 예약 취소 (내부적으로 환불 프로세스 호출) | User |
| `POST` | `/payments` | 특정 예약 건에 대한 결제 승인 요청 | User |
| `POST` | `/payments/webhook` | PG사 서버 측 결제 결과 통보 (비동기 콜백) | PG사 |

---

## 🤖 4. 챗봇 및 알림 서비스 (Chatbot & Notification)

| Method | Endpoint | Description | Auth |
| ------ | -------- | ----------- | ---- |
| `POST` | `/chat/sessions` | 새로운 챗봇 대화 세션 생성 | - |
| `POST` | `/chat/sessions/{sessionId}/messages` | 챗봇 질문 전송 및 답변 수신 (Streaming/REST) | - |
| `GET` | `/notices` | 호텔 전체 공지사항 목록 조회 | - |

---

## 🛠️ 5. 백오피스 관리자 서비스 (Admin)

| Method | Endpoint | Description | Auth |
| ------ | -------- | ----------- | ---- |
| `POST` | `/notices` | 공지사항 생성 및 등록 | Admin |
| `PUT` | `/notices/{noticeId}` | 등록된 공지사항 수정 | Admin |

---

## ☸️ 6. 인프라 헬스 체크 (AWS EKS Probes)

| Method | Endpoint | Description | Auth |
| ------ | -------- | ----------- | ---- |
| `GET` | `/health/liveness` | Application 데드락 상태 확인 (Liveness Probe) | - |
| `GET` | `/health/readiness` | DB/Redis 연결 등 트래픽 수신 가능 상태 확인 (Readiness Probe) | - |

---

## Verification Criteria

API Endpoint의 구현 및 검증 상태를 판단할 때 다음 기준을 사용한다.

### Implemented

다음 조건을 모두 만족하면 해당 Endpoint를 **Implemented** 상태로 판단한다.

1. **Endpoint 구현**
   - Backend에 해당 Endpoint가 구현되어 있다.

2. **HTTP Method 및 Endpoint 일치**
   - HTTP Method와 Endpoint가 API 명세와 일치한다.

3. **Authentication / Authorization**
   - 명세에 정의된 Authentication 및 Authorization이 적용되어 있다.

4. **Request / Response 구조**
   - Request 및 Response 구조가 정의되어 있다.
   - 필요한 경우 DTO, Schema 등을 통해 구조가 명확하게 정의되어 있다.

5. **예외 처리**
   - 주요 예외 상황에 대한 처리가 구현되어 있다.
   - 적절한 HTTP Status Code 및 Error Response를 반환한다.

### Verified

다음 조건을 실제 API 호출을 통해 확인하면 해당 Endpoint를 **Verified** 상태로 판단한다.

1. **정상 Request 검증**
   - 정상적인 Request에 대해 예상한 Response가 반환된다.

2. **HTTP Status Code 검증**
   - HTTP Status Code가 API 명세와 일치한다.

3. **Response Body 검증**
   - Response Body의 구조와 데이터가 API 명세와 일치한다.

4. **Authentication / Authorization 검증**
   - 인증된 사용자와 인증되지 않은 사용자에 대한 접근 제어가 정상적으로 동작한다.
   - 필요한 경우 사용자 권한에 따른 접근 제어가 정상적으로 동작한다.

5. **Error Response 검증**
   - 잘못된 Request에 대해 예상한 Error Response가 반환된다.
   - 필수 값 누락, 잘못된 형식, 존재하지 않는 Resource 등의 상황을 검증한다.

6. **Edge Case 검증**
   - 해당 Endpoint의 주요 Edge Case가 정상적으로 처리된다.