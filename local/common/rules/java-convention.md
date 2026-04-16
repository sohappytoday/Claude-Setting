---
glob: "**/*.java"
---

# Java 코드 작성 규칙

## 패키지 구조
```
com.example
 ├─ {domain}
 │   ├─ controller
 │   ├─ service
 │   │   ├─ common / admin / client
 │   │   │   ├─ command   # 쓰기
 │   │   │   └─ query     # 읽기
 │   ├─ dto
 │   │   ├─ request / search
 │   │   ├─ command
 │   │   ├─ result
 │   │   ├─ response
 │   │   └─ query
 │   ├─ domain
 │   ├─ repository
 │   └─ exception
 ├─ global
 │   ├─ config / exception / security / logging / common
 └─ Application.java
```

## 네이밍
| 요소 | 규칙 | 예시 |
|------|------|------|
| 클래스 | PascalCase | `UserController` |
| 메서드/변수 | camelCase | `getMyProfile`, `maxHeadCount` |
| Enum | SCREAMING_SNAKE_CASE | `REFRESH_TOKEN` |
| 패키지 | 소문자 | `user` |

## 클래스별 기본 애노테이션
```java
// Controller
@RestController
@RequiredArgsConstructor
@RequestMapping(...)

// Service
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true) // 또는 @Transactional
// @Slf4j는 로그가 필요한 경우에만 추가

// Domain
@Entity
@Getter
@Table(name = "...")
@NoArgsConstructor(access = AccessLevel.PROTECTED)
```

## 지양하는 애노테이션 (모든 레이어 공통)
- `@Setter` : 불변성 보장을 위해 사용 금지
- `@Data` : Setter 포함으로 사용 금지
- `@AllArgsConstructor` : 필드 순서 의존, 실수 유발 → `@RequiredArgsConstructor` 또는 정적 팩토리 사용

## Domain 규칙
- 필드 순서: `식별자` → `기본 도메인` → `상태` → `연관 관계`
- Column명: `{엔티티명}_{필드_스네이크케이스}` (예: `reservation_head_count`)
- 양방향 연관관계 금지 (영속성 전이 필요 시 예외)
- 외부 노출 엔티티는 `publicId` 필수 생성
- 생성자는 `private`, 정적 팩토리 `create()` 사용
- 상속 관계 → JOIN 전략

## DTO 규칙
- `from(단일 파라미터)` / `of(복수 파라미터)` 정적 팩토리 사용
- 파라미터에 엔티티 객체 불가
- Null 가능 → 래퍼 타입(`Integer`), 불가 → 기본형(`int`)

## Service
- 메서드명은 Controller 메서드명과 동일하게 유지 (분기 호출 제외)

## Repository
- fetch join 필요 시 `@Query`로 JPQL 작성
- QueryDSL은 `RepositoryImpl`에서 구현

## API URL
- 명사만, 복수형, 케밥 케이스 (`/user-profiles`)
- PathVariable은 camelCase (`{userPublicId}`)
- `PUT` 사용 금지
- `DELETE`에 RequestBody 금지 (여러 개 삭제 → `POST` + RequestBody)
- Entity Id 노출 금지 → `publicId` 사용
- 쿼리 파라미터는 필터/검색/페이징에만

## 코드 스타일
- 들여쓰기 4칸
- 중괄호는 같은 줄에 (`if (condition) {`)
- 한 줄짜리 메서드도 줄바꿈
- Builder는 모든 필드 줄바꿈
- Controller 파라미터는 정렬, 그 외 5개 이상이면 절반씩 나눔 (홀수: N, N+1)
