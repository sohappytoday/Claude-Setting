---
glob: "**/*.py"
---

# Python 코드 작성 규칙

## 프로젝트 구조
```
project/
 ├─ {domain}/
 │   ├─ router.py       # FastAPI 라우터 또는 Blueprint
 │   ├─ service.py      # 비즈니스 로직
 │   ├─ repository.py   # DB 접근
 │   ├─ schema.py       # 요청/응답 DTO (Pydantic)
 │   ├─ model.py        # ORM 모델
 │   └─ exception.py    # 도메인 예외
 ├─ core/
 │   ├─ config.py       # 환경 설정
 │   ├─ database.py     # DB 연결
 │   ├─ security.py     # 인증/인가
 │   └─ exception.py    # 전역 예외
 └─ main.py
```

## 네이밍
| 요소 | 규칙 | 예시 |
|------|------|------|
| 클래스 | PascalCase | `UserService` |
| 함수/변수 | snake_case | `get_user_profile`, `max_head_count` |
| 상수 | SCREAMING_SNAKE_CASE | `REFRESH_TOKEN_EXPIRE` |
| 모듈/패키지 | snake_case 소문자 | `user_profile` |
| 비공개 | 언더스코어 prefix | `_validate_token` |

## 타입 힌트
- 모든 함수에 타입 힌트 필수
- 반환 타입 명시 필수
- `Optional` 대신 `X | None` 사용 (Python 3.10+)

```python
# Good
def get_user(user_id: int) -> UserResponse | None:
    ...

# Bad
def get_user(user_id):
    ...
```

## 클래스 구조
```python
# Router (FastAPI 기준)
router = APIRouter(prefix="/users", tags=["users"])

# Service
class UserService:
    def __init__(self, repo: UserRepository) -> None:
        self._repo = repo

# Schema (Pydantic)
class UserResponse(BaseModel):
    model_config = ConfigDict(from_attributes=True)

    id: int
    name: str

# Model (SQLAlchemy)
class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(100))
```

## 지양하는 패턴
- `*` import 금지 (`from module import *`)
- 가변 기본 인수 금지 (`def f(items=[])` → `def f(items=None)`)
- `except Exception` 단독 사용 금지 → 구체적인 예외 명시
- 전역 변수 사용 금지

## Schema (DTO) 규칙
- 요청: `{Action}Request` (예: `CreateUserRequest`)
- 응답: `{Domain}Response` (예: `UserResponse`)
- 엔티티 객체를 파라미터로 직접 받지 않음
- Null 가능 필드는 `| None` + `default=None` 명시

## Service 규칙
- 메서드명은 Router 핸들러명과 동일하게 유지
- 트랜잭션 단위로 메서드 분리
- 비즈니스 로직만 포함, DB 직접 접근 금지

## Repository 규칙
- DB 접근은 Repository에서만
- 반환 타입은 모델 또는 `None` (예외 발생 금지, 없으면 `None` 반환)
- 복잡한 쿼리는 메서드명에 의도 명시 (`find_active_users_by_team`)

## 코드 스타일
- 들여쓰기 4칸
- 한 줄 최대 120자
- 함수/클래스 사이 빈 줄 2개, 메서드 사이 빈 줄 1개
- f-string 사용 (`f"Hello {name}"`)
- `if x is None` / `if x is not None` 명시적 비교
