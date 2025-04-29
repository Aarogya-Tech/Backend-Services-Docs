# Streak Feature Implementation Guide for Frontend Developers

## Overview
The streak feature tracks user engagement by recording visits and calculating streaks based on configurable thresholds. This document provides implementation guidance for frontend developers to integrate with the streak API endpoints.

## API Endpoints

The streak feature provides two main endpoints for retrieving streak summary data:

### 1. JWT Token-Based Endpoint (For Production Use)

This endpoint should be used in production environments where users are authenticated with JWT tokens.

- **URL**: `https://dev.docseva.com/health/api/v1/streaks/summary/latest`
- **Method**: GET
- **Authentication**: Requires a valid JWT token in the Authorization header
- **Headers**:
  - `Authorization: Bearer <jwt_token>`
  - `accept: */*`
- **Response**: See [Response Format](#response-format) section

#### Example Request
```bash
curl --location 'https://dev.docseva.com/health/api/v1/streaks/summary/latest' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...' \
--header 'accept: */*'
```

### 2. User ID-Based Endpoint (For Testing)

This endpoint can be used for testing purposes or when you need to retrieve streak data for a specific user ID.

- **URL**: `https://dev.docseva.com/health/api/v1/streaks/summary/{userId}`
- **Method**: GET
- **Path Parameters**:
  - `userId`: The UUID of the user (e.g., `10000000-0000-0000-0000-000000000001`)
- **Headers**:
  - `accept: */*`
- **Response**: See [Response Format](#response-format) section

#### Example Request
```bash
curl --location 'https://dev.docseva.com/health/api/v1/streaks/summary/10000000-0000-0000-0000-000000000001' \
--header 'accept: */*'
```

## Response Format

Both endpoints return the same response format:

```json
{
  "status": "SUCCESS",
  "message": null,
  "data": {
    "userName": "John Doe",
    "days": [
      {
        "date": "2023-05-01",
        "day": "MON",
        "completed": true
      },
      {
        "date": "2023-05-02",
        "day": "TUE",
        "completed": true
      },
      {
        "date": "2023-05-03",
        "day": "WED",
        "completed": true
      },
      {
        "date": "2023-05-04",
        "day": "THU",
        "completed": false
      },
      {
        "date": "2023-05-05",
        "day": "FRI",
        "completed": false
      },
      {
        "date": "2023-05-06",
        "day": "SAT",
        "completed": false
      },
      {
        "date": "2023-05-07",
        "day": "SUN",
        "completed": false
      }
    ],
    "trophiesEarned": 3,
    "streakAchieved": true,
    "currentStreak": 1,
    "maxStreak": 1,
    "totalSessions": 3,
    "longestConsecutiveMeasurementStreakDays": 3
  }
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `userName` | String | The name of the user |
| `days` | Array | A list of calendar days with their completion status |
| `days[].date` | String | The date in ISO format (YYYY-MM-DD) |
| `days[].day` | String | The day of the week (MON, TUE, WED, etc.) |
| `days[].completed` | Boolean | Whether a visit occurred on that day |
| `trophiesEarned` | Integer | The number of trophies earned in the current period (up to the streak threshold) |
| `streakAchieved` | Boolean | Whether a streak has been achieved in the current period |
| `currentStreak` | Integer | The current streak count (number of consecutive periods with achieved streaks) |
| `maxStreak` | Integer | The maximum streak count achieved by the user |
| `totalSessions` | Integer | The total number of sessions (visits) recorded for the user |
| `longestConsecutiveMeasurementStreakDays` | Integer | The longest streak of consecutive days with measurements |


## Testing

For testing purposes, you can use the user ID-based endpoint with a test user ID:

```bash
curl --location 'https://dev.docseva.com/health/api/v1/streaks/summary/10000000-0000-0000-0000-000000000001' \
--header 'accept: */*'
```

This allows you to test the streak feature without needing to authenticate with a JWT token.

## Error Handling

The API may return error responses in the following format:

```json
{
  "status": "FAILURE",
  "message": "Error message describing the issue",
  "data": null
}
```

Your frontend implementation should handle these error cases gracefully and display appropriate messages to the user.

## Additional Resources

For more detailed information about the streak feature, including implementation details and example scenarios, refer to the comprehensive [Streak Feature Documentation](STREAK_DOCUMENTATION.md).
