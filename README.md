# Third-Party API Documentation

## Authentication

All API requests require authentication using an API key. Include your API key in the request headers:

```
X-API-KEY: your_api_key_here
```

API keys are associated with an organisation and can be managed through the admin interface. Contact your administrator to obtain an API key.

## Encrypted Identifiers

For security reasons, all IDs used in API requests and responses are encrypted. To get the encrypted identifiers for your organisation's LinkedIn users and campaign routines, use the following endpoint:

```
GET /social_app/api/v1/third_party/user-campaign-filters
```

This endpoint returns encrypted IDs that should be used in subsequent API calls. The system automatically decrypts these IDs when processing your requests.

## Endpoints

### Create API Key

```
POST /social_app/api/v1/third_party/create-api-key
```

Creates a new API key for an organization. This endpoint requires admin authentication.

#### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| organisation_id | Integer | No | ID of the organization to create the API key for. If not provided, uses the current user's organization. |
| description | String | No | Description for the API key. If not provided, a default description with the current timestamp will be used. |

#### Response

```json
{
  "code": 200,
  "status": true,
  "message": "API key created successfully",
  "data": {
    "api_key": "a1b2c3d4e5f6...",
    "created_at": "2025-05-09T10:15:30Z",
    "description": "API key created on 2025-05-09 10:15"
  }
}
```

### Get Dashboard Information

```
GET /social_app/api/v1/third_party/dashboard-info
POST /social_app/api/v1/third_party/dashboard-info
```

Returns dashboard analytics data including campaign statistics, connection graphs, and more. Supports both GET and POST methods.

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| linkedin_user_information_ids | Array | Optional. Array of LinkedIn user information IDs to filter by |
| start_date | Date | Optional. Start date for data range (format: YYYY-MM-DD) |
| end_date | Date | Optional. End date for data range (format: YYYY-MM-DD) |

#### Example Response

```json
{
  "code": 200,
  "status": true,
  "message": "Dashboard details shared successfully",
  "data": {
    "campaign_member_info": {
      "data": [
        {
          "date": "2025-05-01",
          "count_data": 25
        }
      ],
      "total_member_count": 25
    },
    "campaign_count_info": {
      "completed": 5,
      "active": 10,
      "inactive": 2,
      "total": 17
    },
    "connection_graph": {
      "request": [
        {
          "date": "2025-05-01",
          "count_data": 50
        }
      ],
      "accept": [
        {
          "date": "2025-05-01",
          "count_data": 30
        }
      ],
      "pending_reject_count": 20,
      "total_friend_accept_count": 30,
      "total_friend_request_count": 50,
      "connection_percentage": 60.0
    }
  }
}
```

### Get Campaign Information

```
GET /social_app/api/v1/third_party/campaign-info
POST /social_app/api/v1/third_party/campaign-info
```

Returns detailed information about campaigns. Supports both GET and POST methods.

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| linkedin_user_information_ids | Array | Optional. Array of encrypted LinkedIn user information IDs to filter by |
| start_date | Date | Optional. Start date for data range (format: YYYY-MM-DD) |
| end_date | Date | Optional. End date for data range (format: YYYY-MM-DD) |
| limit | Integer | Optional. Number of results to return (default: 50) |
| offset | Integer | Optional. Offset for pagination (default: 0) |
| term | String | Optional. Search term to filter campaign names |

#### Example Response

```json
{
  "code": 200,
  "status": true,
  "message": "Campaign info shared successfully",
  "data": {
    "campaign_info": [
      {
        "linkedin_campaign_routine": {
          "id": 123,
          "campaign_name": "Sample Campaign",
          "campaign_status": "active"
        },
        "member_count": 150,
        "friend_request_count": 100,
        "friend_accept_count": 75,
        "connection_percentage": 75.0,
        "message_sent_count": 80,
        "message_accept_count": 40,
        "message_percentage": 50.0
      }
    ],
    "total_campaign_routines_count": 5
  }
}
```

## Error Handling

All endpoints return a standard error format:

```json
{
  "code": 400,
  "status": false,
  "message": "Error occurred",
  "data": {
    "error": "Error message details"
  }
}
```

Common error codes:

- 400: Bad Request - The request was invalid
- 401: Unauthorized - Invalid or missing API key
- 404: Not Found - The requested resource was not found
- 500: Internal Server Error - Something went wrong on the server

### Get Optimized Campaign Routine Data

```
GET /social_app/api/v1/third_party/optimized_routine_data
```

Returns detailed analytics and information for a specific campaign routine.

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| linkedin_campaign_routine_id | String | Required. Encrypted ID of the LinkedIn campaign routine |
| start_date | Date | Optional. Start date for data range (format: YYYY-MM-DD) |
| end_date | Date | Optional. End date for data range (format: YYYY-MM-DD) |

### Get Encrypted Identifiers

```
GET /social_app/api/v1/third_party/user-campaign-filters
```

Returns encrypted identifiers for all LinkedIn users and campaign routines in your organisation.

#### Example Response

```json
{
  "code": 200,
  "status": true,
  "message": "Encrypted identifiers retrieved successfully",
  "data": {
    "linkedin_users": [
      {
        "linkedin_user_information_id": "encrypted_id_string",
        "name": "User Name"
      }
    ],
    "campaign_routines": [
      {
        "linkedin_campaign_routine_id": "encrypted_id_string",
        "name": "Campaign Name"
      }
    ]
  }
}
```

## Rate Limiting

API requests are subject to rate limiting to prevent abuse. Current limits:

- 100 requests per minute per API key and IP address
- 5,000 requests per day per API key and IP address

Exceeding these limits will result in a 429 Too Many Requests response with the following format:

```json
{
  "code": 429,
  "status": false,
  "message": "Rate limit exceeded. Maximum 100 requests per minute allowed.",
  "data": {
    "error": "Too many requests",
    "retry_after": 45
  }
}
```

### Rate Limit Headers

Each API response includes headers that provide information about your current rate limit status:

- `X-RateLimit-Limit-Minute`: Maximum number of requests allowed per minute (100)
- `X-RateLimit-Remaining-Minute`: Number of requests remaining in the current minute window
- `X-RateLimit-Limit-Day`: Maximum number of requests allowed per day (5,000)
- `X-RateLimit-Remaining-Day`: Number of requests remaining in the current day window

When a rate limit is exceeded, the `retry_after` field in the response indicates the number of seconds (for minute limits) or hours (for daily limits) to wait before making another request.
