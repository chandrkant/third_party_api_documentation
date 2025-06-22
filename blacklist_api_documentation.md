# Blacklist API Documentation

This document provides comprehensive documentation for the Blacklist API endpoints. These endpoints allow you to manage blacklisted LinkedIn profiles within your organization.

## Base URL

All API endpoints are prefixed with:

```
/social_app/api/v1/blacklist
```

## Authentication

All requests require authentication using an API token. Include the token in the request header:

```
Authorization: your_api_key_here
```

## Response Format

All API responses follow a standard format:

```json
{
  "code": 200,          // HTTP status code
  "status": true,       // Boolean indicating success or failure
  "message": "Success", // Human-readable message
  "data": {             // Response data (structure varies by endpoint)
    // Endpoint-specific data
  }
}
```

## Error Handling

Errors follow the same format, but with appropriate status codes and error messages:

```json
{
  "code": 400,                   // HTTP status code
  "status": false,               // Always false for errors
  "message": "Error message",    // Human-readable error message
  "data": {}                     // Empty object or additional error details
}
```

## Blacklist Integration with Campaigns

Blacklists can be integrated with LinkedIn campaigns to exclude specific blacklist campaigns. This allows you to run campaigns while ignoring certain blacklisted profiles.

### Excluded Blacklist IDs

When updating a campaign routine, you can specify which blacklist campaigns should be excluded from consideration. This is done by including the `excluded_blacklist_ids` setting in the campaign settings.

## Endpoints

### 1. List Blacklist Campaigns

Retrieves a paginated list of blacklist campaigns for the organization.

- **URL**: `/campaigns`
- **Method**: `GET`
- **Authentication**: Required

#### Example Request

```bash
curl -X GET "https://dev-api.konnector.ai/social_app/api/v1/blacklist/campaigns?page=1&per_page=20&search=Global" \
  -H "Authorization: 869d05d20e57f119a228de87f65a7c709fb6d794408a17c15829f5cb58cbf28f" \
  -H "Content-Type: application/json"
```

#### Query Parameters

| Parameter | Type   | Required | Default | Description                                |
|-----------|--------|----------|---------|--------------------------------------------|
| page      | Integer| No       | 1       | Page number for pagination                 |
| per_page  | Integer| No       | 20      | Items per page (max: 100)                  |
| search    | String | No       | null    | Search term to filter by campaign name     |

#### Success Response

```json
{
  "code": 200,
  "status": true,
  "message": "Blacklist campaigns retrieved successfully",
  "data": {
    "campaigns": [
      {
        "id": 123,
        "campaign_name": "Global Blacklist",
        "created_at": "2025-06-01T10:30:00Z"
      },
      {
        "id": 124,
        "campaign_name": "Sales Team Blacklist",
        "created_at": "2025-05-28T14:15:00Z"
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_count": 92,
      "per_page": 20
    }
  }
}
```

### 2. Add Members to Blacklist

Adds LinkedIn members to a blacklist campaign.

- **URL**: `/`
- **Method**: `POST`
- **Authentication**: Required

#### Example Request

```bash
curl -X POST "https://dev-api.konnector.ai/social_app/api/v1/blacklist/add_to_blacklist" \
  -H "Authorization: 869d05d20e57f119a228de87f65a7c709fb6d794408a17c15829f5cb58cbf28f" \
  -H "Content-Type: application/json" \
  -d '{
    "organisation_member_id": 16,
    "blacklist": {
      "linkedin_campaign_member_ids": [789, 790, 791],
      "linkedin_user_campaign_id": 123,
      "linkedin_campaign_routine_id": 2442,
      "excluded_campaign_member_ids": [792, 793],
      "linkedin_user_campaign_ids": [456, 457],
      "campaign_name": "Custom Blacklist"
    },
    "linkedin_user_information_id": 863
  }'
```

#### Request Body Parameters

| Parameter                                  | Type    | Required | Description                                       |
|--------------------------------------------|---------|----------|---------------------------------------------------|
| organisation_member_id                     | Integer | Yes      | ID of the organization member making the request  |
| blacklist[linkedin_campaign_member_ids]    | Array   | Yes*     | Array of LinkedIn campaign member IDs to blacklist|
| blacklist[linkedin_user_campaign_ids]      | Array   | Yes*     | Array of LinkedIn user campaign IDs to blacklist  |
| blacklist[linkedin_user_campaign_id]       | Integer | No       | ID of existing blacklist campaign (if updating)   |
| blacklist[linkedin_campaign_routine_id]    | Integer | No       | ID of the campaign routine to associate with the blacklist |
| blacklist[excluded_campaign_member_ids]    | Array   | No       | Array of campaign member IDs to exclude from blacklisting |
| blacklist[campaign_name]                   | String  | No       | Optional name for the blacklist campaign          |
| linkedin_user_information_id               | Integer | No       | ID of LinkedIn user information (optional)        |

*Either `linkedin_campaign_member_ids` or `linkedin_user_campaign_ids` must be provided

#### Example Request

```json
{
  "organisation_member_id": 456,
  "blacklist": {
    "linkedin_campaign_member_ids": [789, 790, 791],
    "linkedin_user_campaign_id": 123,
    "linkedin_campaign_routine_id": 2442,
    "excluded_campaign_member_ids": [792, 793],
    "linkedin_user_campaign_ids": [456, 457],
    "campaign_name": "Custom Blacklist"
  },
  "linkedin_user_information_id": 345
}
```

#### Success Response

```json
{
  "code": 200,
  "status": true,
  "message": "Members added to blacklist successfully",
  "data": {
    "campaign_id": 123,
    "campaign_name": "Global Blacklist",
    "members_added": 3
  }
}
```

#### Error Response

```json
{
  "code": 400,
  "status": false,
  "message": "organisation_member_id is required",
  "data": {}
}
```

### 3. Get Blacklist Campaign Details

Retrieves details of a specific blacklist campaign including its members.

- **URL**: `/campaigns/:id`
- **Method**: `GET`
- **Authentication**: Required

#### Example Request

```bash
curl -X GET "https://dev-api.konnector.ai/social_app/api/v1/blacklist/campaigns/123?page=1&per_page=20" \
  -H "Authorization: 869d05d20e57f119a228de87f65a7c709fb6d794408a17c15829f5cb58cbf28f" \
  -H "Content-Type: application/json"
```

#### URL Parameters

| Parameter | Type    | Required | Description                   |
|-----------|---------|----------|-------------------------------|
| id        | Integer | Yes      | ID of the blacklist campaign  |

#### Query Parameters

| Parameter | Type    | Required | Default | Description                |
|-----------|---------|----------|---------|----------------------------|
| page      | Integer | No       | 1       | Page number for pagination |
| per_page  | Integer | No       | 20      | Items per page (max: 100)  |

#### Success Response

```json
{
  "code": 200,
  "status": true,
  "message": "Blacklist campaign details retrieved successfully",
  "data": {
    "campaign": {
      "id": 123,
      "campaign_name": "Global Blacklist",
      "created_at": "2025-06-01T10:30:00Z"
    },
    "members": [
      {
        "id": 789,
        "profile_id": "linkedin-profile-id-1",
        "name": "John Doe",
        "company_name": "Acme Inc",
        "created_at": "2025-06-01T10:35:00Z"
      },
      {
        "id": 790,
        "profile_id": "linkedin-profile-id-2",
        "name": "Jane Smith",
        "company_name": "XYZ Corp",
        "created_at": "2025-06-01T10:35:00Z"
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 3,
      "total_count": 45,
      "per_page": 20
    }
  }
}
```

#### Error Response

```json
{
  "code": 404,
  "status": false,
  "message": "Blacklist campaign not found",
  "data": {}
}
```

### 4. Check Blacklist Campaign

Checks if a blacklist campaign exists and retrieves its details.

- **URL**: `/campaigns/check`
- **Method**: `GET`
- **Authentication**: Required

#### Example Request

```bash
curl -X GET "https://dev-api.konnector.ai/social_app/api/v1/blacklist/campaigns/check?linkedin_user_campaign_id=123" \
  -H "Authorization: 869d05d20e57f119a228de87f65a7c709fb6d794408a17c15829f5cb58cbf28f" \
  -H "Content-Type: application/json"
```

#### Query Parameters

| Parameter                | Type    | Required | Description                  |
|-------------------------|---------|----------|------------------------------|
| linkedin_user_campaign_id| Integer | Yes      | ID of the campaign to check  |

#### Success Response (Campaign Found)

```json
{
  "code": 200,
  "status": true,
  "message": "Blacklist campaign found",
  "data": {
    "exists": true,
    "campaign": {
      "id": 123,
      "campaign_name": "Global Blacklist",
      "created_at": "2025-06-01T10:30:00Z",
      "member_count": 45
    }
  }
}
```

#### Success Response (Campaign Not Found)

```json
{
  "code": 404,
  "status": false,
  "message": "Blacklist campaign not found",
  "data": {
    "exists": false
  }
}
```

## Implementation Notes

1. **Organization Context**: All endpoints operate within the context of the authenticated organization.

2. **LinkedIn User Information**: The `linkedin_user_information_id` parameter is optional for blacklist operations. When provided, it associates the blacklist campaign with a specific LinkedIn user. When absent, the blacklist campaign is created at the organization level only.

3. **Pagination**: List endpoints support pagination with `page` and `per_page` parameters. The maximum items per page is 100.

4. **Search**: The list campaigns endpoint supports searching by campaign name using the `search` parameter.

5. **Error Handling**: All endpoints include appropriate error messages and status codes for common error scenarios.

6. **Excluded Blacklists**: Campaigns can exclude specific blacklist campaigns by using the `excluded_blacklist_ids` setting in the campaign settings. This allows campaigns to ignore certain blacklisted profiles.

## Campaign Integration

### Update Routine with Excluded Blacklist IDs

To update a campaign routine with excluded blacklist IDs, use the update_routine endpoint:

- **URL**: `/social_app/api/v1/social-api/update_routine`
- **Method**: `POST`
- **Authentication**: Required

#### Example Request

```bash
curl --location --request POST 'https://dev-api.konnector.ai/social_app/api/v1/social-api/update_routine?organisation_member_id=16&time_zone=Asia%2FCalcutta' \
--header 'Accept: application/json, text/plain, */*' \
--header 'Accept-Language: en-US,en;q=0.9' \
--header 'Authorization: 869d05d20e57f119a228de87f65a7c709fb6d794408a17c15829f5cb58cbf28f' \
--header 'Connection: keep-alive' \
--header 'Origin: https://dev-next.konnector.ai' \
--header 'Referer: https://dev-next.konnector.ai/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "linkedin_user_information_id": 863,
    "change_after_live": false,
    "linkedin_campaign_routine_id": "2442",
    "stage": 3,
    "campaign_status": "inactive",
    "is_draft": false,
    "campaign_name": "New AI Test 34",
    "settings": [
        {
            "email_source": [
                {
                    "id": "email",
                    "title": "Lead\'s LinkedIn profile",
                    "priority": "P1",
                    "enabled": false
                },
                {
                    "id": "find_email",
                    "title": "Konnector Email Finder",
                    "priority": null,
                    "enabled": false
                },
                {
                    "id": "csv_email",
                    "title": "Your CSV Upload",
                    "priority": null,
                    "enabled": false
                }
            ]
        },
        {
            "comment_settings": [
                {
                    "auto_comment_enabled": false,
                    "comment_strategy": "wait_approval_continue"
                }
            ]
        },{
            "excluded_blacklist_ids": [5224]
        }
    ],
    "linkedin_user_information_ids": [
        863
    ]
}'
```

#### Request Body Parameters for Excluded Blacklist IDs

| Parameter                     | Type    | Description                                       |
|------------------------------|---------|--------------------------------------------------|
| settings                     | Array   | Array of setting objects                          |
| settings[].excluded_blacklist_ids | Array   | **NEW SETTING**: Array of blacklist campaign IDs to exclude from this campaign |

#### Success Response

```json
{
  "code": 200,
  "status": true,
  "message": "Campaign routine updated successfully",
  "data": {
    "campaign_id": 2442,
    "campaign_name": "New AI Test 34",
    "settings": {
      "excluded_blacklist_ids": [5224]
    }
  }
}
```

## UI Implementation Recommendations

1. **Blacklist Management Page**:
   - List view of all blacklist campaigns with search and pagination
   - Button to create a new blacklist campaign
   - Click on a campaign to view its details and members

2. **Add to Blacklist Modal**:
   - Form to select members to blacklist
   - Option to add to an existing blacklist campaign or create a new one
   - Confirmation message after successful addition

3. **Campaign Details Page**:
   - Display campaign information (name, creation date)
   - Paginated list of blacklisted members
   - Option to remove members from the blacklist

4. **Integration with Other Features**:
   - Add "Blacklist" action button in member lists throughout the application
   - Show blacklist status indicator on member profiles
   - Prevent blacklisted members from being added to active campaigns
   - Option to exclude specific blacklist campaigns when configuring LinkedIn campaigns
