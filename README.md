# LinkedIn API Documentation

![LinkedIn API](https://content.pstmn.io/8b056aeb-b6c2-4110-85ab-ae759e519e21/bG9nby1zeW1ib2wucG5n)

## Introduction

Welcome to the LinkedIn API. Let's get started with a quick setup on how to use the API!

**Version:** 1.0.0  
**Last Updated:** May 12, 2025  
**Base URL:** `https://dev-api.konnector.ai/social_app/api/v1/third_party`

This API provides access to LinkedIn campaign data, user information, and analytics for third-party integrations. Use this API to build custom dashboards, integrate with CRM systems, and automate reporting workflows.

## Finding your API key

The first step when using the LinkedIn API is authenticating your requests with an API key.  
The API key is used to authenticate the incoming requests and map them to your organization.  
API keys never expire, however they can be deleted/deactivated.

## Authentication

After you have your API key, you will need to provide it in every request that you make to the LinkedIn API.  
You will need to add the `Authorization` request header to every request and set your API key as the value.

```
Authorization: your_api_key_here
```

### Test your API key

Once you have your API key, you can check if it's working by sending the following request.  
If everything is working properly, you should get a `200` HTTP status code.

```bash
curl --location 'https://dev-api.konnector.ai/social_app/api/v1/third_party/user-campaign-filters' \
--header 'Authorization: your_api_key_here'
```

---

## Encrypted Identifiers

For security reasons, all IDs used in API requests and responses are encrypted. To get the encrypted identifiers for your organisation's LinkedIn users and campaign routines, use the following endpoint.

### Get Encrypted Identifiers

Retrieves encrypted identifiers for all LinkedIn users and campaign routines in your organisation.

```http
GET /user-campaign-filters
```

#### Headers

| Key | Value | Description |
|-----|-------|-------------|
| Authorization | your_api_key_here | Your API key |
| Content-Type | application/json | The content type |

#### Responses

##### 200: Success

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
      }
    ],
    "campaign_routines": [
      {
        "linkedin_campaign_routine_id": "encrypted_id_string",
        "name": "Q2 Sales Outreach",
        "campaign_type": "smart_campaign",
        "campaign_status": "active",
        "created_at": "2025-05-01T10:15:30Z"
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

##### 401: Unauthorized

```json
{
  "code": 401,
  "status": false,
  "message": "Unauthorized access",
  "data": {
    "error": "Invalid API key",
    "error_code": "INVALID_API_KEY",
    "request_id": "req_12345abcde",
    "timestamp": "2025-05-11T14:30:45+05:30"
  }
}
```

---

## API Key Management

### Create API Key

Creates a new API key for an organization. This endpoint requires admin authentication.

```http
POST /create-api-key
```

#### Headers

| Key | Value | Description |
|-----|-------|-------------|
| Authorization | your_api_key_here | Your API key |
| Content-Type | application/json | The content type |

#### Body Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| organisation_id | Integer | No | ID of the organization to create the API key for. If not provided, uses the current user's organization. |
| description | String | No | Description for the API key. If not provided, a default description with the current timestamp will be used. |

```json
{
  "organisation_id": 123,
  "description": "API key for third-party integration with CRM system"
}
```

#### Responses

##### 200: Success

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

##### 401: Unauthorized

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

---

## Dashboard Analytics

### Get Dashboard Information

Returns dashboard analytics data including campaign statistics, connection graphs, and more.

```http
GET /dashboard-info
POST /dashboard-info
```

#### Headers

| Key | Value | Description |
|-----|-------|-------------|
| Authorization | your_api_key_here | Your API key |
| Content-Type | application/json | The content type |

#### Query Parameters (GET) / Body Parameters (POST)

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| linkedin_user_information_ids | Array | No | Array of LinkedIn user information IDs to filter by |
| start_date | Date | No | Start date for data range (format: YYYY-MM-DD) |
| end_date | Date | No | End date for data range (format: YYYY-MM-DD) |

```json
{
  "linkedin_user_information_ids": ["encrypted_id_string_1", "encrypted_id_string_2"],
  "start_date": "2025-05-01",
  "end_date": "2025-05-10"
}
```

#### Response

<details>
<summary>Example Response (200 OK)</summary>

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
        }
        // Additional data points...
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
        // Connection request data...
      ],
      "accept": [
        // Connection accept data...
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
        // Daily message statistics...
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
      // Additional user performance data...
    ],
    "timestamp": "2025-05-11T14:59:52+05:30",
    "request_id": "req_dashboard_12345"
  }
}
```
</details>

<details>
<summary>Error Response (400 Bad Request)</summary>

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
</details>

---

## Campaign Management

### Get Campaign Information

> Returns detailed information about campaigns.

```
GET /campaign-info
POST /campaign-info
```

#### Request Headers

| Header | Value |
|--------|-------|
| Authorization | your_api_key_here |
| Content-Type | application/json |

#### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| linkedin_user_information_ids | Array | No | Array of encrypted LinkedIn user information IDs to filter by |
| start_date | Date | No | Start date for data range (format: YYYY-MM-DD) |
| end_date | Date | No | End date for data range (format: YYYY-MM-DD) |
| limit | Integer | No | Number of results to return (default: 50) |
| offset | Integer | No | Offset for pagination (default: 0) |
| term | String | No | Search term to filter campaign names |
| campaign_status | String | No | Filter by campaign status (active, inactive, completed, draft) |
| campaign_type | String | No | Filter by campaign type (smart_campaign, friend_campaign, etc.) |
| sort_by | String | No | Field to sort results by (created_at, campaign_name, etc.) |
| sort_order | String | No | Sort order (asc, desc) |

#### Request Body (POST)

<details>
<summary>Example Request</summary>

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
</details>

#### Response

<details>
<summary>Example Response (200 OK)</summary>

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
          // Daily campaign statistics...
        ],
        "linkedin_user": {
          "name": "John Doe",
          "linkedin_user_information_id": "encrypted_id_string_1",
          "email": "john.doe@example.com",
          "status": "active"
        },
        "campaign_steps": [
          // Campaign steps information...
        ]
      },
      // Additional campaign information...
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
</details>

<details>
<summary>Error Response (400 Bad Request)</summary>

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
</details>

### Get Optimized Campaign Routine Data

> Returns detailed analytics and information for a specific campaign routine.

```
GET /optimized_routine_data
```

#### Request Headers

| Header | Value |
|--------|-------|
| Authorization | your_api_key_here |
| Content-Type | application/json |

#### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| linkedin_campaign_routine_id | String | Yes | Encrypted ID of the LinkedIn campaign routine |
| start_date | Date | No | Start date for data range (format: YYYY-MM-DD) |
| end_date | Date | No | End date for data range (format: YYYY-MM-DD) |

#### Response

<details>
<summary>Example Response (200 OK)</summary>

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
          // Daily connection metrics...
        ]
      },
      "message_metrics": {
        "message_sent_count": 80,
        "message_replied_count": 40,
        "response_rate": 50.0,
        "daily_metrics": [
          // Daily message metrics...
        ]
      }
    },
    "campaign_steps": [
      // Campaign steps with metrics...
    ],
    "linkedin_user": {
      "name": "John Doe",
      "linkedin_user_information_id": "encrypted_user_id",
      "email": "john.doe@example.com"
    }
  }
}
```
</details>

---

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

### Common Error Codes

| Code | Description |
|------|-------------|
| 400 | Bad Request - The request was invalid |
| 401 | Unauthorized - Invalid or missing API key |
| 404 | Not Found - The requested resource was not found |
| 500 | Internal Server Error - Something went wrong on the server |

---

## Rate Limits

LinkedIn API allows a maximum of `100` requests per minute and `5,000` requests per day. All requests count towards these limits, regardless of whether they succeed or fail.

If you exceed the rate limits, you will receive a `429 Too Many Requests` response with details about when you can retry.

### Rate Limit Response

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

| Header | Description |
|--------|-------------|
| X-RateLimit-Limit-Minute | Maximum number of requests allowed per minute (100) |
| X-RateLimit-Remaining-Minute | Number of requests remaining in the current minute window |
| X-RateLimit-Limit-Day | Maximum number of requests allowed per day (5,000) |
| X-RateLimit-Remaining-Day | Number of requests remaining in the current day window |

---

## Code Examples

### Get Dashboard Information

#### cURL

```bash
curl --location 'https://dev-api.konnector.ai/social_app/api/v1/third_party/dashboard-info' \
--header 'Authorization: your_api_key_here' \
--header 'Content-Type: application/json'
```

#### JavaScript (Fetch)

```javascript
var requestOptions = {
  method: 'GET',
  headers: {
    'Authorization': 'your_api_key_here',
    'Content-Type': 'application/json'
  },
  redirect: 'follow'
};

fetch("https://dev-api.konnector.ai/social_app/api/v1/third_party/dashboard-info", requestOptions)
  .then(response => response.json())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

#### NodeJs (Axios)

```javascript
var axios = require('axios');

var config = {
  method: 'get',
  url: 'https://dev-api.konnector.ai/social_app/api/v1/third_party/dashboard-info',
  headers: { 
    'Authorization': 'your_api_key_here', 
    'Content-Type': 'application/json'
  }
};

axios(config)
.then(function (response) {
  console.log(JSON.stringify(response.data));
})
.catch(function (error) {
  console.log(error);
});
```

#### Python (Requests)

```python
import requests

url = "https://dev-api.konnector.ai/social_app/api/v1/third_party/dashboard-info"

headers = {
  'Authorization': 'your_api_key_here',
  'Content-Type': 'application/json'
}

response = requests.request("GET", url, headers=headers)

print(response.text)
```

#### PHP (cURL)

```php
<?php
$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://dev-api.konnector.ai/social_app/api/v1/third_party/dashboard-info',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'GET',
  CURLOPT_HTTPHEADER => array(
    'Authorization: your_api_key_here',
    'Content-Type: application/json'
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
```

---

## Enjoy building 🛠️

If you encounter any issues or have questions about the API, please contact our support team at api-support@example.com or visit our developer portal at https://developers.example.com.

---

<div style="text-align: center; margin-top: 50px; color: #075e54;">
  <p>© 2025 Example Organization. All rights reserved.</p>
</div>
