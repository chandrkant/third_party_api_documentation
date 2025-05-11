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
        "name": "User Name",
        "email": "user@example.com",
        "status": "active"
      }
    ],
    "campaign_routines": [
      {
        "linkedin_campaign_routine_id": "encrypted_id_string",
        "name": "Campaign Name",
        "campaign_type": "smart_campaign",
        "campaign_status": "active",
        "created_at": "2025-05-01T10:15:30Z"
      }
    ],
    "organisation": {
      "id": 123,
      "name": "Example Organization"
    }
  }
}
```

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

#### Example Request

```json
{
  "organisation_id": 123,
  "description": "API key for third-party integration with CRM system"
}
```

#### Example Response

```json
{
  "code": 200,
  "status": true,
  "message": "API key created successfully",
  "data": {
    "api_key": "a1b2c3d4e5f6...",
    "created_at": "2025-05-11T14:30:45Z",
    "description": "API key for third-party integration with CRM system",
    "organisation_id": 123,
    "organisation_name": "Example Organization",
    "expires_at": "2026-05-11T14:30:45Z",
    "permissions": ["read", "write"],
    "created_by": "admin@example.com"
  }
}
```

#### Error Response

```json
{
  "code": 401,
  "status": false,
  "message": "Unauthorized access",
  "data": {
    "error": "Only organization administrators can create API keys",
    "error_code": "UNAUTHORIZED_ACCESS",
    "request_id": "req_12345abcde",
    "timestamp": "2025-05-11T14:30:45+05:30"
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

#### Example Request (POST)

```json
{
  "linkedin_user_information_ids": ["encrypted_id_string_1", "encrypted_id_string_2"],
  "start_date": "2025-05-01",
  "end_date": "2025-05-10"
}
```

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
        },
        {
          "date": "2025-05-02",
          "count_data": 32
        },
        {
          "date": "2025-05-03",
          "count_data": 18
        },
        {
          "date": "2025-05-04",
          "count_data": 22
        },
        {
          "date": "2025-05-05",
          "count_data": 30
        },
        {
          "date": "2025-05-06",
          "count_data": 28
        },
        {
          "date": "2025-05-07",
          "count_data": 35
        },
        {
          "date": "2025-05-08",
          "count_data": 40
        },
        {
          "date": "2025-05-09",
          "count_data": 45
        },
        {
          "date": "2025-05-10",
          "count_data": 38
        }
      ],
      "total_member_count": 313
    },
    "campaign_count_info": {
      "completed": 5,
      "active": 10,
      "inactive": 2,
      "draft": 3,
      "total": 20
    },
    "connection_graph": {
      "request": [
        {
          "date": "2025-05-01",
          "count_data": 50
        },
        {
          "date": "2025-05-02",
          "count_data": 65
        },
        {
          "date": "2025-05-03",
          "count_data": 42
        },
        {
          "date": "2025-05-04",
          "count_data": 38
        },
        {
          "date": "2025-05-05",
          "count_data": 55
        },
        {
          "date": "2025-05-06",
          "count_data": 60
        },
        {
          "date": "2025-05-07",
          "count_data": 70
        },
        {
          "date": "2025-05-08",
          "count_data": 75
        },
        {
          "date": "2025-05-09",
          "count_data": 80
        },
        {
          "date": "2025-05-10",
          "count_data": 72
        }
      ],
      "accept": [
        {
          "date": "2025-05-01",
          "count_data": 30
        },
        {
          "date": "2025-05-02",
          "count_data": 40
        },
        {
          "date": "2025-05-03",
          "count_data": 25
        },
        {
          "date": "2025-05-04",
          "count_data": 22
        },
        {
          "date": "2025-05-05",
          "count_data": 35
        },
        {
          "date": "2025-05-06",
          "count_data": 38
        },
        {
          "date": "2025-05-07",
          "count_data": 45
        },
        {
          "date": "2025-05-08",
          "count_data": 48
        },
        {
          "date": "2025-05-09",
          "count_data": 52
        },
        {
          "date": "2025-05-10",
          "count_data": 47
        }
      ],
      "pending_reject_count": 157,
      "total_friend_accept_count": 382,
      "total_friend_request_count": 607,
      "connection_percentage": 62.9
    },
    "message_stats": {
      "sent": 520,
      "replied": 215,
      "response_rate": 41.3,
      "daily_stats": [
        {
          "date": "2025-05-01",
          "sent": 45,
          "replied": 18
        },
        {
          "date": "2025-05-02",
          "sent": 52,
          "replied": 22
        },
        {
          "date": "2025-05-03",
          "sent": 38,
          "replied": 15
        },
        {
          "date": "2025-05-04",
          "sent": 35,
          "replied": 14
        },
        {
          "date": "2025-05-05",
          "sent": 48,
          "replied": 20
        },
        {
          "date": "2025-05-06",
          "sent": 55,
          "replied": 23
        },
        {
          "date": "2025-05-07",
          "sent": 62,
          "replied": 26
        },
        {
          "date": "2025-05-08",
          "sent": 68,
          "replied": 28
        },
        {
          "date": "2025-05-09",
          "sent": 72,
          "replied": 30
        },
        {
          "date": "2025-05-10",
          "sent": 65,
          "replied": 27
        }
      ]
    },
    "campaign_types": {
      "friend_request_follow_message": 5,
      "connector_campaign": 3,
      "friend_campaign": 4,
      "smart_campaign": 6,
      "shared_campaign": 2
    },
    "user_performance": [
      {
        "linkedin_user_information_id": "encrypted_id_string_1",
        "name": "John Doe",
        "email": "john.doe@example.com",
        "connection_rate": 65.2,
        "response_rate": 42.8,
        "active_campaigns": 6
      },
      {
        "linkedin_user_information_id": "encrypted_id_string_2",
        "name": "Jane Smith",
        "email": "jane.smith@example.com",
        "connection_rate": 60.5,
        "response_rate": 39.7,
        "active_campaigns": 4
      }
    ],
    "timestamp": "2025-05-11T14:59:52+05:30",
    "request_id": "req_dashboard_12345"
  }
}
```

#### Error Response

```json
{
  "code": 400,
  "status": false,
  "message": "Invalid date range",
  "data": {
    "error": "End date cannot be before start date",
    "error_code": "INVALID_DATE_RANGE",
    "request_id": "req_12345abcde",
    "timestamp": "2025-05-11T14:59:52+05:30"
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
| campaign_status | String | Optional. Filter by campaign status (active, inactive, completed, draft) |
| campaign_type | String | Optional. Filter by campaign type (smart_campaign, friend_campaign, etc.) |
| sort_by | String | Optional. Field to sort results by (created_at, campaign_name, etc.) |
| sort_order | String | Optional. Sort order (asc, desc) |

#### Example Request (POST)

```json
{
  "linkedin_user_information_ids": ["encrypted_id_string_1"],
  "start_date": "2025-05-01",
  "end_date": "2025-05-10",
  "limit": 10,
  "offset": 0,
  "campaign_status": "active",
  "sort_by": "created_at",
  "sort_order": "desc"
}
```

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
          "id": "encrypted_id_string_1",
          "campaign_name": "Q2 Sales Outreach",
          "campaign_status": "active",
          "campaign_type": "smart_campaign",
          "created_at": "2025-05-08T10:15:30Z",
          "last_activate_time": "2025-05-09T08:30:15Z",
          "initial_activate_time": "2025-05-08T14:22:10Z",
          "max_campaign_days": 7,
          "source": "manual",
          "metadata": {
            "target_industry": "Technology",
            "target_positions": ["CTO", "VP Engineering", "Technical Director"],
            "campaign_goals": "Generate leads for enterprise software"
          }
        },
        "member_count": 150,
        "friend_request_count": 100,
        "friend_accept_count": 75,
        "connection_percentage": 75.0,
        "message_sent_count": 80,
        "message_replied_count": 40,
        "response_rate": 50.0,
        "campaign_duration_days": 3,
        "daily_stats": [
          {
            "date": "2025-05-08",
            "connections": {
              "sent": 40,
              "accepted": 28
            },
            "messages": {
              "sent": 30,
              "replied": 15
            }
          },
          {
            "date": "2025-05-09",
            "connections": {
              "sent": 35,
              "accepted": 26
            },
            "messages": {
              "sent": 28,
              "replied": 14
            }
          },
          {
            "date": "2025-05-10",
            "connections": {
              "sent": 25,
              "accepted": 21
            },
            "messages": {
              "sent": 22,
              "replied": 11
            }
          }
        ],
        "linkedin_user": {
          "name": "John Doe",
          "linkedin_user_information_id": "encrypted_id_string_1",
          "email": "john.doe@example.com",
          "status": "active"
        },
        "campaign_steps": [
          {
            "step_id": "encrypted_step_id_1",
            "step_name": "Initial Connection",
            "step_type": "connection_request",
            "completion_rate": 85.0
          },
          {
            "step_id": "encrypted_step_id_2",
            "step_name": "Follow-up Message",
            "step_type": "message",
            "completion_rate": 75.0
          }
        ]
      },
      {
        "linkedin_campaign_routine": {
          "id": "encrypted_id_string_2",
          "campaign_name": "Marketing Directors Network",
          "campaign_status": "active",
          "campaign_type": "friend_campaign",
          "created_at": "2025-05-07T14:30:45Z",
          "last_activate_time": "2025-05-08T09:15:20Z",
          "initial_activate_time": "2025-05-07T16:45:30Z",
          "max_campaign_days": 7,
          "source": "manual",
          "metadata": {
            "target_industry": "Marketing",
            "target_positions": ["CMO", "Marketing Director", "Head of Marketing"],
            "campaign_goals": "Build marketing professional network"
          }
        },
        "member_count": 120,
        "friend_request_count": 85,
        "friend_accept_count": 60,
        "connection_percentage": 70.6,
        "message_sent_count": 65,
        "message_replied_count": 32,
        "response_rate": 49.2,
        "campaign_duration_days": 4,
        "daily_stats": [
          {
            "date": "2025-05-07",
            "connections": {
              "sent": 25,
              "accepted": 16
            },
            "messages": {
              "sent": 18,
              "replied": 8
            }
          },
          {
            "date": "2025-05-08",
            "connections": {
              "sent": 22,
              "accepted": 15
            },
            "messages": {
              "sent": 16,
              "replied": 8
            }
          },
          {
            "date": "2025-05-09",
            "connections": {
              "sent": 20,
              "accepted": 16
            },
            "messages": {
              "sent": 17,
              "replied": 9
            }
          },
          {
            "date": "2025-05-10",
            "connections": {
              "sent": 18,
              "accepted": 13
            },
            "messages": {
              "sent": 14,
              "replied": 7
            }
          }
        ],
        "linkedin_user": {
          "name": "Jane Smith",
          "linkedin_user_information_id": "encrypted_id_string_2",
          "email": "jane.smith@example.com",
          "status": "active"
        },
        "campaign_steps": [
          {
            "step_id": "encrypted_step_id_3",
            "step_name": "Initial Connection",
            "step_type": "connection_request",
            "completion_rate": 82.5
          },
          {
            "step_id": "encrypted_step_id_4",
            "step_name": "Welcome Message",
            "step_type": "message",
            "completion_rate": 70.2
          },
          {
            "step_id": "encrypted_step_id_5",
            "step_name": "Follow-up Message",
            "step_type": "message",
            "completion_rate": 58.6
          }
        ]
      }
    ],
    "total_campaign_routines_count": 8,
    "pagination": {
      "current_page": 1,
      "total_pages": 4,
      "limit": 10,
      "offset": 0,
      "next_page_url": "/social_app/api/v1/third_party/campaign-info?offset=10&limit=10",
      "prev_page_url": null
    },
    "filters_applied": {
      "linkedin_user_information_ids": ["encrypted_id_string_1"],
      "start_date": "2025-05-01",
      "end_date": "2025-05-10",
      "campaign_status": "active",
      "sort_by": "created_at",
      "sort_order": "desc"
    },
    "timestamp": "2025-05-11T14:59:52+05:30",
    "request_id": "req_campaign_67890"
  }
}
```

#### Error Response

```json
{
  "code": 400,
  "status": false,
  "message": "Invalid parameters",
  "data": {
    "error": "One or more LinkedIn user information IDs are invalid",
    "error_code": "INVALID_USER_IDS",
    "request_id": "req_98765abcde",
    "timestamp": "2025-05-11T14:59:52+05:30"
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
    "error": "Error message details",
    "error_code": "INVALID_REQUEST",
    "request_id": "req_12345abcde",
    "timestamp": "2025-05-09T22:36:05+05:30"
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

#### Example Response

```json
{
  "code": 200,
  "status": true,
  "message": "Campaign routine data retrieved successfully",
  "data": {
    "campaign_details": {
      "id": "encrypted_id_string",
      "name": "Q2 Sales Outreach",
      "campaign_type": "smart_campaign",
      "campaign_status": "active",
      "created_at": "2025-05-01T10:15:30Z",
      "last_activate_time": "2025-05-02T08:30:15Z",
      "campaign_duration_days": 7,
      "max_campaign_days": 7
    },
    "performance_metrics": {
      "member_count": 150,
      "connection_metrics": {
        "friend_request_count": 100,
        "friend_accept_count": 75,
        "connection_percentage": 75.0,
        "pending_count": 25,
        "daily_metrics": [
          {
            "date": "2025-05-01",
            "requests_sent": 20,
            "requests_accepted": 15
          },
          {
            "date": "2025-05-02",
            "requests_sent": 25,
            "requests_accepted": 18
          }
        ]
      },
      "message_metrics": {
        "message_sent_count": 80,
        "message_replied_count": 40,
        "response_rate": 50.0,
        "daily_metrics": [
          {
            "date": "2025-05-01",
            "messages_sent": 15,
            "messages_replied": 8
          },
          {
            "date": "2025-05-02",
            "messages_sent": 20,
            "messages_replied": 10
          }
        ]
      }
    },
    "campaign_steps": [
      {
        "step_id": "encrypted_step_id_1",
        "step_name": "Initial Connection",
        "step_type": "connection_request",
        "completion_rate": 85.0,
        "metrics": {
          "total_attempts": 100,
          "successful": 85,
          "failed": 15
        }
      },
      {
        "step_id": "encrypted_step_id_2",
        "step_name": "Follow-up Message",
        "step_type": "message",
        "completion_rate": 75.0,
        "metrics": {
          "total_attempts": 85,
          "successful": 75,
          "failed": 10
        }
      }
    ],
    "linkedin_user": {
      "name": "John Doe",
      "linkedin_user_information_id": "encrypted_user_id",
      "email": "john.doe@example.com"
    }
  }
}
```

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
        "name": "User Name",
        "email": "user@example.com",
        "status": "active"
      },
      {
        "linkedin_user_information_id": "encrypted_id_string_2",
        "name": "Jane Smith",
        "email": "jane.smith@example.com",
        "status": "active"
      },
      {
        "linkedin_user_information_id": "encrypted_id_string_3",
        "name": "Robert Johnson",
        "email": "robert.johnson@example.com",
        "status": "inactive"
      }
    ],
    "campaign_routines": [
      {
        "linkedin_campaign_routine_id": "encrypted_id_string",
        "name": "Q2 Sales Outreach",
        "campaign_type": "smart_campaign",
        "campaign_status": "active",
        "created_at": "2025-05-01T10:15:30Z"
      },
      {
        "linkedin_campaign_routine_id": "encrypted_id_string_2",
        "name": "Marketing Professionals Network",
        "campaign_type": "friend_campaign",
        "campaign_status": "active",
        "created_at": "2025-04-15T14:22:10Z"
      },
      {
        "linkedin_campaign_routine_id": "encrypted_id_string_3",
        "name": "Tech Conference Follow-up",
        "campaign_type": "connector_campaign",
        "campaign_status": "completed",
        "created_at": "2025-03-20T09:45:00Z"
      }
    ],
    "organisation": {
      "id": 123,
      "name": "Example Organization",
      "created_at": "2024-01-15T08:30:00Z",
      "subscription_status": "active"
    }
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
    "error_code": "RATE_LIMIT_EXCEEDED",
    "retry_after": 45,
    "request_id": "req_12345abcde",
    "timestamp": "2025-05-09T22:36:05+05:30",
    "rate_limit_info": {
      "limit_minute": 100,
      "remaining_minute": 0,
      "limit_day": 5000,
      "remaining_day": 3250
    }
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
