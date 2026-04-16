# Java 프로젝트 컨벤션

## 1. 브랜치 컨벤션

### 브랜치 종류
- `main` : 실제 운영 서버 (프로젝트 끝날 때까지 직접 push 불가)
- `staging` : 프론트엔드 연동용 실험 브랜치
- `dev` : 개인 코드 통합 브랜치
- `feature/{이슈번호}-{작업명}` : 이슈별 개인 브랜치 (예: `feature/1-ci-setup`)

### 규칙
- `main`, `dev`, `staging`은 로컬에서 직접 push 불가 → 오직 PR로만 가능
- 개발 시작 시 Required approvals를 1로 설정하여 개인 PR/Merge 불가
- CI가 돌기 전까지 Merge 금지, CI/Static analysis 실패 시 Merge 금지

---

## 2. 커밋 컨벤션 (AngularJS Commit Convention)

### 타입
| 타입 | 설명 |
|------|------|
| feat | 새로운 기능 추가 |
| fix | 버그 및 오타 수정 |
| refactor | 리팩토링 |
| test | 테스트 코드 |
| docs | 문서 수정, 주석 추가 및 삭제 |
| style | 포맷, 세미콜론, 공백 |
| chore | 설정, 패키지, 빌드, CI, CD |
| perf | 성능 개선 |

### 규칙
- 제목은 **영어**, 본문은 한글/영어 자유
- 제목 첫 단어는 **무조건 대문자** (예: `Implement`, `Add`, `Fix`)
- 특수 기호 자제
- 제목이 길면 축약하고 본문에 상세 작성
- 부연 설명 없으면 `git commit -m` 사용, 설명 필요 시 `git commit`으로 본문까지 작성

```shell
git commit -m "chore: Add ci.yml in .github/workflows"
```

---

## 3. 이슈 컨벤션

- 이슈 주제에 맞는 템플릿 선택: `Bug` / `Chore` / `Feature` / `Refactor` / `Test`
- Assignee, Label 선택
- **PR 전에 반드시 이슈 생성** (이슈 번호 없으면 PR 불가)

---

## 4. PR / Merge 컨벤션

### PR 제목
```
[TYPE] #{issue_number} short-description
```
- 타입은 **대문자** 고정
- 예시: `[FEAT] #10 프로필 포트폴리오 개발`, `[CHORE] #3 CI 파이프라인 추가`

### Merge 방식
| 대상 브랜치 | 방식 |
|------------|------|
| `dev` | **Squash and Merge** |
| `staging`, `main` | **Fast Forward (Commit for merge)** |

### Squash Merge 커밋 메시지
```
# 제목
[issue_number] {type}: 설명

# 본문
PR 본문 또는 커밋 제목들 (삭제 X)
```
- PR 번호 자동 추가되면 삭제 (예: ~~(#15)~~)

---

## 5. 패키지 / 디렉토리 구조 컨벤션

### 기본 구조 (도메인별)
```
com.example
 ├─ user
 │   ├─ controller
 │   ├─ service
 │   ├─ dto
 │   ├─ domain
 │   ├─ repository
 │   ├─ mapper
 │   └─ exception
 ├─ global
 │   ├─ config
 │   ├─ exception
 │   ├─ schedular
 │   ├─ security
 │   ├─ logging
 │   └─ common
 └─ Application.java
```

### DTO 구조
```
dto
 ├─ query       # ResultDto 내부 Dto, Page 사용 시 QueryDto
 ├─ command     # service 계층으로 보내는 dto (request → command 변환)
 ├─ request     # controller에서 받는 dto
 │   └─ search  # QueryDSL search condition
 ├─ response    # controller에서 반환하는 dto
 └─ result      # service 계층에서 반환하는 dto
```

### Service 구조 (CQRS + admin/client 분리)
```
service
 ├─ common
 │   ├─ command / projectCommandService
 │   └─ query   / projectQueryService
 ├─ admin
 │   ├─ command / adminProjectCommandService
 │   └─ query   / adminProjectQueryService
 └─ client
     ├─ command / clientProjectCommandService
     └─ query   / clientProjectQueryService
```
- **command**: 쓰기 작업
- **query**: 읽기 작업

### Repository
- `MemberRepository` : JpaRepository 상속, fetch join 필요 시 `@Query`로 JPQL 작성
- `MemberRepositoryCustom` : 인터페이스
- `MemberRepositoryImpl` : 구현체 (모든 조회 로직, QueryDSL)

### 더미 데이터
- `src/init` 디렉토리 생성하여 추가

---

## 6. 클래스 / 메서드 / 변수 네이밍 컨벤션

| 요소 | 규칙 | 예시 |
|------|------|------|
| 패키지 | 소문자 + 카멜 | `user` |
| 클래스 | 파스칼 케이스 | `UserController` |
| 메서드 | 카멜 케이스 (소문자 시작) | `getMyProfile` |
| 변수 | 카멜 케이스 (소문자 시작) | `int maxHeadCount` |
| Enum | Screaming Snake Case | `REFRESH_TOKEN` |

### Controller
```java
@RestController
@RequiredArgsConstructor
@RequestMapping(상황따라)
```

### Service
```java
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true) // 또는 @Transactional
// @Slf4j는 로그가 필요한 경우에만 추가
```
- Service 메서드명은 Controller 메서드명과 동일하게 유지 (분기 호출 제외)

### DTO
- Setter, @Data 지양
- `from(파라미터 하나)` / `of(파라미터 여러개)` 정적 팩토리 메서드 사용
- **파라미터에 엔티티 객체 불가** (command dto, response dto에서만 이용)
- Null 가능 → 객체형 (`Integer`), 불가 → 기본형 (`int`)

### Domain
```java
@Entity
@Getter
@Table(name = "users")
@NoArgsConstructor(access = AccessLevel.PROTECTED)
```

**필드 순서**: `식별자 필드` → `기본 도메인 필드` → `상태 필드` → `연관 관계 필드`

**Column 이름**: `{엔티티이름}_{필드_스네이크케이스}` (예: `reservation_head_count`)

**규칙**:
- 양방향 연관관계 사용 금지 (단방향만, 영속성 전이 필요 시 예외)
- 외부 노출 엔티티는 `publicId` 무조건 생성
- 생성자는 `private`, 정적 생성 메서드(`create()`) 사용
- 상속 관계 매핑은 `JOIN 전략` 통일

### 지양하는 애노테이션 (모든 레이어 공통)
- `@Setter` : 불변성 보장을 위해 사용 금지
- `@Data` : Setter 포함으로 사용 금지
- `@AllArgsConstructor` : 필드 순서 의존, 실수 유발 → `@RequiredArgsConstructor` 또는 정적 팩토리 메서드 사용

---

## 7. API URL 컨벤션

### 기본 규칙
- URL에는 **명사만** 사용 (`/users/get-profile` ❌ → `/users/profile` ✅)
- **복수형** 기본 (`/user` ❌ → `/users` ✅)
- **케밥 케이스** (소문자 + 하이픈), PathVariable은 **카멜 케이스**
- 계층 구조는 소유 관계로만 (`/users/me/profile`)

### 금지 사항
- `PUT` HTTP Method 사용 금지
- `DELETE`에 RequestBody 금지 (여러 개 삭제 필요 시 `POST` + RequestBody)
- Entity Id 노출 금지 → `publicId` 사용
```
GET /users/{userId}/profile   ❌
GET /users/{userPublicId}/profile  ✅
```

### Query Parameter
- 필터 / 검색 / 페이징에만 사용
```
GET /users?grade=VIP
GET /members?username=kim&ageGoe=20&ageLoe=30&page=0&size=20
```

### Bulk 처리
```
POST /admin/ships/bulk-delete
```

### 파일 업로드
```
POST /users/me/profile/image
```

---

## 8. 코드 스타일 / 포매팅 컨벤션

- `ctrl + alt + L` 적극 활용

### 들여쓰기
- **4칸** 고정

### 중괄호 위치
```java
if (condition) {    // ✅
    log.info("...");
}

if (condition)      // ❌
{
    log.info("...");
}
```

### 파라미터
- Controller 파라미터는 하나에 모두 띄어쓰기 정렬
- 그 외 파라미터 5개 이상이면 절반씩 나눠서 작성 (홀수: N, N+1)

### 한 줄 메서드도 줄바꿈
```java
private void setNickname(String nickname) {    // ✅
    this.nickname = nickname;
}

private void setNickname(String nickname) { this.nickname = nickname; }  // ❌
```

### Builder
- 모든 줄 띄어쓰기
```java
return ReservationDetailResponse.builder()
        .ship(result.getShip())
        .reservation(result.getReservation())
        .schedule(result.getSchedule())
        .build();
```

---

## 9. CI / CD 컨벤션

### 브랜치별 CI/CD

| 브랜치 | 역할 | CI | CD |
|--------|------|----|----|
| `feature/~`, `fix/~` | 개인 이슈 작업 | X | X |
| `dev` | 통합 개발 | PR 시 | X |
| `staging` | 배포 | PR 시 | Merge 시 |
| `main` | 운영 및 배포 | PR 시 | Merge 시 |

### CI 파일명
- `feature` → `dev` PR : `dev-pr-ci.yml` (코드 실행, 테스트, 코드 품질 검사)
- `dev` → `staging` PR : `staging-pr-ci.yml` (컴파일/테스트, RDS/Redis 연결, Docker 빌드 등)
- `staging` → `main` PR : `main-pr-ci.yml`

### CD 파일명
- `dev` → `staging` Merge : `staging-merge.cd.yml`
  1. Docker 로그인
  2. 이미지 빌드 및 태그
  3. Docker Hub push
  4. 서버에서 pull 후 컨테이너 생성
  5. 헬스체크
  6. 실패 시 자동 롤백
- `staging` → `main` Merge : `main-merge.cd.yml`
