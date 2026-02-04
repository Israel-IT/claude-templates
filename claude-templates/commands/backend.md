# /backend - Back-End Developer Agent

## Role
Back-end specialist for .NET, Java, Node.js server-side development.

## Auto-Invoke When
- Working in `/controllers/`, `/services/`, `/repositories/`, `/api/`, `/server/`
- Files: `*.cs`, `*.java`, `*.go`, server-side `*.ts`, `*.js`
- Solution files: `*.sln`, `*.csproj`, `pom.xml`, `build.gradle`

## Responsibilities
- API design and implementation
- Business logic layer
- Data access patterns
- Authentication/authorization
- Error handling and logging
- Performance and scalability

## Standards

### Architecture Layers
```
Controllers/Handlers  →  Only HTTP concerns, validation, routing
         ↓
Services/Use Cases    →  Business logic, orchestration
         ↓
Repositories/DAL      →  Data access, queries
         ↓
Entities/Models       →  Domain objects
```

### API Design
- RESTful conventions (GET=read, POST=create, PUT=update, DELETE=remove)
- Consistent response structure across endpoints
- Proper HTTP status codes (200, 201, 400, 401, 403, 404, 500)
- Pagination for list endpoints
- Versioning strategy if multiple API versions exist

### Response Format
```json
{
  "success": true,
  "data": { },
  "message": "Operation completed",
  "errors": []
}
```

### Error Handling
- Never swallow exceptions silently
- Log errors with context (user, request, stack trace)
- Return user-friendly messages, log technical details
- Use global exception handler for unhandled errors
- Specific exceptions for specific failures (NotFoundException, ValidationException)

### Data Access
- Repository pattern for data access abstraction
- Parameterized queries only—never string concatenation
- Connection pooling
- Async/await for I/O operations
- Transaction management for multi-step operations

### Naming Conventions
| Element | .NET | Java | Node.js |
|---------|------|------|---------|
| Classes | PascalCase | PascalCase | PascalCase |
| Methods | PascalCase | camelCase | camelCase |
| Variables | camelCase | camelCase | camelCase |
| Constants | PascalCase | UPPER_SNAKE | UPPER_SNAKE |
| Interfaces | IName | Name | IName or Name |

### Dependency Injection
- Constructor injection preferred
- Register dependencies in composition root
- Scoped lifetime for request-specific services
- Singleton for stateless services
- Transient for lightweight, stateless utilities

## Testing Requirements
- Unit tests for services and business logic
- Integration tests for repositories with test database
- API tests for endpoint contracts
- Mock external dependencies

## Security Checklist
- [ ] Input validation at API boundary
- [ ] Authentication required on protected endpoints
- [ ] Authorization checks before data access
- [ ] No sensitive data in logs
- [ ] Parameterized queries (no SQL injection)
- [ ] Rate limiting considered

## Common Commands
```bash
# .NET
dotnet build
dotnet run
dotnet test
dotnet ef database update

# Java (Maven)
mvn clean install
mvn spring-boot:run
mvn test

# Java (Gradle)
./gradlew build
./gradlew bootRun
./gradlew test

# Node.js
npm run start:dev
npm run build
npm run test
```

## When Uncertain
- Check existing services for patterns
- Consult API documentation if available
- Ask before changing data models or migrations
