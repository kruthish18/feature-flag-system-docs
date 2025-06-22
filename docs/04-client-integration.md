# Client Integration

This section describes how clients can consume and evaluate feature flags either through SDKs or directly via HTTP API. The integration method depends on your application architecture and performance requirements.

---

## Option 1: Using the SDK

The SDK simplifies flag evaluation by wrapping the API calls and caching logic. You can embed the SDK in your frontend, backend, or mobile apps.

### Example (Node.js SDK)

```javascript
import FeatureFlagClient from '@yourorg/feature-flag-sdk';

const client = new FeatureFlagClient({
  apiKey: 'your-api-key',
  environment: 'production',
  user: {
    userId: 'user_123',
    email: 'user@example.com',
    device: 'mobile'
  }
});

const isEnabled = await client.evaluate('new_checkout_flow');

if (isEnabled) {
  // Show new checkout
} else {
  // Show old checkout
}
SDK Features
Local caching with TTL

Batched API calls

Supports rollout rules and targeting

Fallback to defaults if server unreachable

Option 2: Using Direct API Calls
For environments without SDK support (e.g., shell scripts, low-level microservices), you can use the /evaluate endpoint directly.

Request


POST /api/v1/evaluate
Content-Type: application/json
Authorization: Bearer <token>

{
  "flagKey": "new_checkout_flow",
  "environment": "production",
  "user": {
    "userId": "user_123",
    "email": "user@example.com",
    "device": "mobile"
  }
}

{
  "flagKey": "new_checkout_flow",
  "enabled": true,
  "reason": "Rollout matched for user_123"
}

Best Practices
Always provide consistent user context (userId, email, etc.) for consistent flag results.

Cache evaluation results locally for session-based access.

Retry API failures with exponential backoff if not using SDK.

Avoid hardcoding flag values in code — always fetch from the system.

Summary
Whether through the SDK or direct API integration, clients can dynamically control application behavior without code changes or redeployments. For details on backend setup and configuration, continue to the Deployment Guide.