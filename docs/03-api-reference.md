# Feature Flag System API Reference

This section describes the RESTful API endpoints available in the Feature Flag System. These APIs allow clients, SDKs, and internal services to create, retrieve, update, and evaluate feature flags and environments.

All endpoints are prefixed with `/api/v1/`.

## GET /flags
Returns a list of all feature flags for a given environment.

### Request HTTP
```
GET /api/v1/flags?env=production
Authorization: Bearer
```

### Response JSON
```json
[
  {
    "id": "flag_001",
    "key": "new_checkout_flow",
    "enabled": true,
    "environments": {
      "production": {
        "enabled": true,
        "rollout": 50
      },
      "staging": {
        "enabled": true,
        "rollout": 100
      }
    },
    "createdAt": "2024-06-01T12:34:56Z"
  },
  {
    "id": "flag_002",
    "key": "beta_search",
    "enabled": false,
    "environments": {
      "production": {
        "enabled": false
      }
    },
    "createdAt": "2024-06-10T09:12:00Z"
  }
]
```

## GET /flags/{id}
Fetches the full configuration for a specific flag by ID.

### Request HTTP
```
GET /api/v1/flags/flag_001
Authorization: Bearer
```

### Response JSON
```json
{
  "id": "flag_001",
  "key": "new_checkout_flow",
  "description": "Enables the redesigned checkout experience",
  "enabled": true,
  "createdBy": "kruthi.hegde@example.com",
  "createdAt": "2024-06-01T12:34:56Z",
  "environments": {
    "production": {
      "enabled": true,
      "rollout": 50,
      "targetingRules": [
        {
          "attribute": "email",
          "operator": "contains",
          "value": "@internal.com"
        }
      ]
    }
  }
}
```

## POST /evaluate
Evaluates a feature flag for a specific user context and returns a boolean or variant result.

### Request HTTP
```
POST /api/v1/evaluate
Content-Type: application/json
Authorization: Bearer
```

### JSON
```json
{
  "flagKey": "new_checkout_flow",
  "environment": "production",
  "user": {
    "userId": "user_123",
    "email": "user@example.com",
    "location": "US",
    "device": "mobile"
  }
}
```

### Response JSON
```json
{
  "flagKey": "new_checkout_flow",
  "enabled": true,
  "reason": "Matched targeting rule: email contains '@example.com'"
}
```

## GET /environments
Returns the list of available environments and their metadata.

### Request HTTP
```
GET /api/v1/environments
Authorization: Bearer
```

### Response JSON
```json
[
  {
    "id": "env_prod",
    "name": "production",
    "description": "Live environment used by real users",
    "createdAt": "2024-01-01T00:00:00Z"
  },
  {
    "id": "env_staging",
    "name": "staging",
    "description": "Internal environment for QA and pre-release testing",
    "createdAt": "2024-01-10T00:00:00Z"
  }
]
```

## Summary
These endpoints form the core of the Feature Flag System's server-side API. Clients can integrate with the system via direct API calls or SDKs that wrap these endpoints. Authentication is typically managed via API tokens or OAuth2-based access control.

To understand how to integrate these APIs on the client side, refer to the *Client Integration* documentation.
