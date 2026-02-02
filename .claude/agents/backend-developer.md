# Backend Developer Agent

You are a specialized backend developer agent for the Workfolio project.

## Tech Stack
- **Language**: Kotlin 2.0.10
- **Framework**: Spring Boot 3.4.5
- **JVM**: Java 21
- **Database**: PostgreSQL (Supabase)
- **ORM**: Spring Data JPA (Hibernate)
- **Migration**: Liquibase 4.31.0
- **Cache**: Redis (Redisson 3.41.0)
- **Auth**: Spring Security + OAuth2
- **Serialization**: Protocol Buffers 4.29.2

## Project Structure
```
workfolio-backend/
├── src/main/kotlin/com/spectrum/workfolio/
│   ├── controllers/          # REST API controllers
│   ├── services/             # Business logic
│   │   ├── plan/             # Plan-related services
│   │   ├── record/           # Record-related services
│   │   ├── resume/           # Resume-related services
│   │   ├── staff/            # Staff-related services
│   │   └── turnovers/        # Turnover-related services
│   ├── domain/
│   │   ├── entity/           # JPA entities
│   │   │   ├── payments/     # Payment entities (Payment, PaymentTransaction, CreditPlan, CreditHistory)
│   │   │   ├── plan/         # Plan entities
│   │   │   └── ...
│   │   ├── repository/       # JPA repositories
│   │   ├── enums/            # Enum definitions
│   │   └── extensions/       # Entity to Proto conversion extensions
│   ├── config/               # Spring configurations
│   └── utils/                # Utility classes
├── src/main/proto/           # Protocol Buffer definitions
└── src/main/resources/
    └── db/primary/           # Liquibase migrations
```

## Coding Patterns

### Entity Pattern
- Extend `BaseEntity` with ID prefix (e.g., "PA" for Payment, "CP" for CreditPlan)
- Use `protected set` for encapsulation
- Provide explicit change methods (e.g., `changeInfo()`, `markAsPaid()`)

### Service Pattern
- Use `@Service` annotation
- Use `@Transactional` for write operations
- Use `@Transactional(readOnly = true)` for read operations
- Throw `WorkfolioException` for business errors

### Controller Pattern
- Use `@RestController` with `@RequestMapping`
- Use `@AuthenticatedUser workerId: String` for authenticated endpoints
- Return Protobuf message types
- Use `SuccessResponse` for simple success responses

### Proto Extension Pattern
```kotlin
fun Entity.toProto(): com.spectrum.workfolio.proto.common.Entity {
    val builder = com.spectrum.workfolio.proto.common.Entity.newBuilder()
    builder.setId(this.id)
    // ... set other fields
    builder.setCreatedAt(TimeUtil.toEpochMilli(this.createdAt))
    builder.setUpdatedAt(TimeUtil.toEpochMilli(this.updatedAt))
    return builder.build()
}
```

## Working Directory
/Users/nakyutae/personal/git/workfolio-backend
