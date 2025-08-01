# Carton Caps Referral Mock API

## NOTE:

Would have used swagger/openAPI for the API documentation, but due to time constraints, I did not have the time to implement it.

## Development

To run the project, use `npm start`.

## Cloud Run

### Local Development

To run this project locally with the Cloud Run emulator, run `npm run local`.

### Deployment

To deploy to Cloud Run, use `npm run deploy`.

## Features

- **5 Individual Cloud Functions** - Each endpoint as a separate function
- **User Management** - Get all users with pagination support
- **Abuse Prevention** - Rate limiting, duplicate prevention, max referrals
- **Referral Expiration** - expiration enforcement
- **TypeScript with Type-fest** - Enhanced type safety and utilities
- **Comprehensive Tests** - Jest with simplified test data access
- **Docker Support** - Ready for containerized deployment

## Cloud Functions

1. **getAllUsers** - `GET` with query params `limit`, `offset` - Get paginated list of all users
2. **getUserReferrals** - `GET` with query params `userId`, `limit`, `offset` - Get user's referrals
3. **getUserReferralInfo** - `GET` with query param `userId` - Get user's referral statistics
4. **validateReferralCode** - `GET` with query param `referralCode` - Validate a referral code
5. **createReferral** - `POST` with JSON body - Create a new referral

# Endpoints

### Get all users (default 8 limit)

`/users`

### Get users with pagination

`/users?limit=10&offset=10`

#### Get User Referrals

`/users/:id/referrals`

#### Get User Referral Info

`/users/:id/referral-stats`

#### Validate Referral Code

`/referrals/:id`

#### Create Referral

```
POST /referrals
 Body {
"referrerUserId": "user-id",
"referredUserEmail": "friend@example.com",
"shareMethod": "email",
"customMessage": "Join me on Carton Caps!"
}
```

## API Response Examples

### Get All Users Response

```json
{
  "users": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440001",
      "email": "test1@example.com",
      "name": "Test User One",
      "referralCode": "TEST0001",
      "createdAt": "2024-01-15T10:00:00.000Z"
    }
  ],
  "total": 8,
  "limit": 50,
  "offset": 0
}
```

### Create Referral Response

```json
{
  "referral": {
    "id": "123e4567-e89b-12d3-a456-426614174000",
    "referrerUserId": "user-id",
    "referralCode": "ABC12345",
    "referredUserEmail": "friend@example.com",
    "status": "pending",
    "createdAt": "2024-01-15T10:30:00Z",
    "shareMethod": "email",
    "customMessage": "Join me on Carton Caps!",
    "expiresAt": "2024-02-15T10:30:00Z"
  },
  "shareUrl": "https://carton-caps.app/install?ref=ABC12345&email=friend%40example.com"
}
```

### Validate Referral Response

```json
{
  "isValid": true,
  "isExpired": false,
  "referral": {
    "id": "123e4567-e89b-12d3-a456-426614174000",
    "referrerUserId": "user-id",
    "referralCode": "ABC12345",
    "status": "pending",
    "createdAt": "2024-01-15T10:30:00Z",
    "expiresAt": "2024-02-15T10:30:00Z"
  },
  "referrer": {
    "id": "user-id",
    "name": "John Doe"
  }
}
```

## Testing

### Run All Tests

```bash
npm test
```

### Test Coverage

The test suite includes:

- **Mock Data Service Tests** - Data management and business logic
- **Cloud Functions Tests** - HTTP request/response handling
- **Abuse Prevention Tests** - Rate limiting and validation
- **Referral Expiration Tests** - Time-based logic
- **Error Handling Tests** - Edge cases and error responses

## Error Handling

All endpoints return consistent error responses:

```json
{
  "error": "ERROR_CODE",
  "message": "Human readable error message",
  "code": 400
}
```

Common error codes:

- `400` - Bad Request (invalid parameters)
- `404` - Not Found (user/referral not found)
- `405` - Method Not Allowed
- `429` - Too Many Requests (abuse prevention)
- `500` - Internal Server Error

## Technology Stack

- **Runtime**: Node.js 20
- **Language**: TypeScript
- **Testing**: Jest
- **Type Utilities**: Type-fest
- **Mock Data**: Faker.js
- **Cloud Platform**: Google Cloud Functions
- **Container**: Docker
- **CI/CD**: Google Cloud Build
