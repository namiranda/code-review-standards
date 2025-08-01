# Go REST API PR Review Checklist

## 🏗️ **Code Structure & Organization**

### Package Structure
- [ ] **Package naming**: Uses lowercase, singular nouns (e.g., `user`, `order`, not `users`, `User`)
- [ ] **Package organization**: Follows domain-driven structure, not technical layers
- [ ] **Internal packages**: Uses `internal/` for private packages that shouldn't be imported

### File Organization
- [ ] **File naming**: Uses snake_case for multi-word files (`user_handler.go`, not `userHandler.go`)
- [ ] **File size**: Individual files are reasonable in size (<500 lines as guideline)
- [ ] **Separation of concerns**: Handlers, models, services are properly separated

## 🔌 **API Design & REST Compliance**

### HTTP Methods & Resources
- [ ] **Proper HTTP verbs**: GET (read), POST (create), PUT (update/replace), PATCH (partial update), DELETE (remove)
- [ ] **Resource naming**: Uses plural nouns for collections (`/users`, `/orders`)
- [ ] **URL structure**: RESTful hierarchy (`/users/{id}/orders` for user's orders)
- [ ] **No verbs in URLs**: Avoids `/getUser` or `/createOrder` patterns

### Status Codes
- [ ] **Appropriate status codes**: 
  - 200 (OK), 201 (Created), 204 (No Content)
  - 400 (Bad Request), 401 (Unauthorized), 403 (Forbidden), 404 (Not Found)
  - 422 (Unprocessable Entity), 500 (Internal Server Error)
- [ ] **Consistent error responses**: Uses standardized error format across all endpoints

### Request/Response Format
- [ ] **Content-Type headers**: Properly set (`application/json`)
- [ ] **JSON naming**: Uses camelCase in JSON responses (with struct tags)
- [ ] **Response structure**: Consistent format (data, meta, errors)
- [ ] **Pagination**: Implements for list endpoints (limit, offset, cursor-based)

## 🔒 **Security & Authentication**

### API Key Authentication
- [ ] **API key validation**: Proper validation and error handling for invalid keys
- [ ] **Rate limiting**: Implements per-key rate limiting if applicable
- [ ] **Key exposure**: API keys not logged or exposed in responses
- [ ] **Middleware placement**: Auth middleware applied at appropriate router level

### Input Validation & Sanitization
- [ ] **Input validation**: All user inputs validated (required fields, formats, ranges)
- [ ] **SQL injection prevention**: Uses parameterized queries or prepared statements
- [ ] **XSS prevention**: Input sanitization for any string fields
- [ ] **Request size limits**: Implements reasonable payload size limits

### Headers & CORS
- [ ] **Security headers**: Includes appropriate security headers
- [ ] **CORS configuration**: Properly configured for your frontend domains
- [ ] **Sensitive data**: No sensitive information in URLs or logs

## 💻 **Go Idiomatic Code**

### Error Handling
- [ ] **Error wrapping**: Uses `fmt.Errorf("message: %w", err)` for context
- [ ] **Error types**: Custom error types when appropriate
- [ ] **Error propagation**: Errors properly bubbled up through call stack
- [ ] **No panic in handlers**: Recovers from panics, doesn't propagate to users

### Naming Conventions
- [ ] **Exported names**: Start with capital letter, unexported with lowercase
- [ ] **Interface naming**: Single-method interfaces end with `-er` (e.g., `Reader`, `Writer`)
- [ ] **Receiver names**: Short, consistent (1-2 letters), not `this` or `self`
- [ ] **Variable naming**: Descriptive names, avoid abbreviations unless common

### Go Patterns
- [ ] **Context usage**: Proper context.Context threading through handlers
- [ ] **Struct composition**: Prefers composition over inheritance
- [ ] **Interface satisfaction**: Implements interfaces implicitly
- [ ] **Zero values**: Leverages useful zero values where appropriate

### Memory & Performance
- [ ] **Avoid memory leaks**: Proper cleanup of resources, channels, goroutines
- [ ] **Efficient JSON marshaling**: Uses appropriate struct tags
- [ ] **String building**: Uses `strings.Builder` for concatenation in loops
- [ ] **Slice preallocation**: Pre-allocates slices when size is known

## 🧪 **Testing (>85% Coverage)**

### Unit Tests
- [ ] **Test coverage**: Achieves >85% coverage for new/modified code
- [ ] **Test naming**: `TestFunctionName_Scenario_ExpectedBehavior`
- [ ] **Table-driven tests**: Uses table-driven tests for multiple scenarios
- [ ] **Test isolation**: Tests don't depend on each other or external state

### HTTP Testing
- [ ] **Handler testing**: Uses `httptest.NewRecorder()` for HTTP handler tests
- [ ] **Status code validation**: Tests verify correct HTTP status codes
- [ ] **Response body validation**: Tests check response content and structure
- [ ] **Error scenarios**: Tests cover error cases (400, 404, 500, etc.)

### Mocking & Dependencies
- [ ] **External dependencies**: Mocked or stubbed appropriately
- [ ] **Database interactions**: Uses test database or proper mocks
- [ ] **API key validation**: Tests both valid and invalid API keys
- [ ] **Test data**: Uses meaningful, realistic test data

## 🗄️ **Database & NoSQL**

### Query Patterns
- [ ] **Query efficiency**: Efficient queries, proper indexing considerations
- [ ] **Connection management**: Proper connection pooling and cleanup
- [ ] **Transaction handling**: Appropriate use of transactions where needed
- [ ] **Error handling**: Database errors properly handled and logged

### Data Validation
- [ ] **Schema validation**: Data validates against expected schema
- [ ] **Data types**: Appropriate data types for NoSQL documents
- [ ] **Null handling**: Proper handling of missing/null fields
- [ ] **Data consistency**: Maintains data consistency rules

## 📊 **Observability & Monitoring**

### Logging
- [ ] **Structured logging**: Uses structured logs (JSON format preferred)
- [ ] **Log levels**: Appropriate log levels (DEBUG, INFO, WARN, ERROR)
- [ ] **No sensitive data**: Doesn't log API keys, passwords, or PII
- [ ] **Request tracing**: Includes request IDs for traceability

### Metrics (DataDog)
- [ ] **Custom metrics**: Adds relevant business metrics where needed
- [ ] **Performance metrics**: Tracks response times, error rates
- [ ] **Resource metrics**: Monitors memory, CPU usage for serverless
- [ ] **Metric naming**: Follows consistent naming conventions

### Health Checks
- [ ] **Health endpoint**: Implements `/health` or `/healthz` endpoint
- [ ] **Dependency checks**: Verifies database and external service connectivity
- [ ] **Graceful shutdown**: Handles shutdown signals properly

## 🚀 **Serverless Considerations**

### Cold Start Optimization
- [ ] **Initialization**: Minimizes cold start time
- [ ] **Global variables**: Proper use of global variables for connection reuse
- [ ] **Dependency loading**: Lazy loading of heavy dependencies when possible
- [ ] **Memory allocation**: Appropriate memory configuration

### Function Structure
- [ ] **Single responsibility**: Each function has clear, focused purpose
- [ ] **Timeout handling**: Respects function timeout limits
- [ ] **Error handling**: Proper error responses for serverless environment
- [ ] **Environment variables**: Uses environment variables for configuration

## 📝 **Documentation & Comments**

### Code Documentation
- [ ] **Package documentation**: Package comment explains purpose
- [ ] **Public API documentation**: Exported functions/types have godoc comments
- [ ] **Complex logic**: Non-obvious code sections have explanatory comments
- [ ] **TODO/FIXME**: Temporary comments include context and timeline

### API Documentation
- [ ] **Endpoint documentation**: Clear description of endpoints, parameters
- [ ] **Request/response examples**: Includes example payloads
- [ ] **Error documentation**: Documents possible error responses
- [ ] **Authentication**: Documents API key requirements

## 🔄 **Code Review Process**

### General Review Points
- [ ] **Business logic**: Code correctly implements requirements
- [ ] **Edge cases**: Handles edge cases and error conditions
- [ ] **Performance impact**: No obvious performance regressions
- [ ] **Backward compatibility**: Changes don't break existing clients

### Team Collaboration
- [ ] **PR description**: Clear description of changes and reasoning
- [ ] **Breaking changes**: Clearly documented and justified
- [ ] **Dependencies**: New dependencies are necessary and well-maintained
- [ ] **Configuration**: New config options properly documented

---

## 🎯 **Quick Validation Commands**

```bash
# Run tests with coverage
go test -v -race -coverprofile=coverage.out ./...
go tool cover -html=coverage.out

# Format and vet
go fmt ./...
go vet ./...

# Check for common issues
golangci-lint run

# Build check
go build ./...
```

---

**Review Priority**: Focus on Security → API Design → Testing → Go Idioms → Performance
