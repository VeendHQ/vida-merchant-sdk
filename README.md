# BNPL Webhook

This README provides documentation for the Buy Now Pay Later (BNPL) webhook integration.

## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
- [Webhook Format](#webhook-format)
- [Status Types](#status-types)
- [API Endpoints](#api-endpoints)
- [Implementation Notes](#implementation-notes)
- [Best Practices](#best-practices)

## Overview
The BNPL webhook system allows you to receive real-time notifications about Buy Now Pay Later transaction status changes. This enables your application to react promptly to transaction updates without polling our API.

## Installation
No additional installation is required. Webhooks are automatically available for all clients with a configured webhook URL.

## Webhook Format
All webhook notifications follow this standard format:

```json
{
  "requestId": "<bnpl_request_id>",
  "status": "<current_status>",
  "reference": "<transaction_reference>",
  "service": "bnpl",
  "metadata": "<additional_data_or_null>"
}
```

## Status Types
The webhook will notify you when a BNPL transaction transitions to any of the following statuses:

| Status | Description |
|--------|-------------|
| `initiated` | The BNPL request has been successfully created and is awaiting further processing. |
| `pending` | The BNPL request is being processed but has not yet been approved or rejected. |
| `claimed` | The customer has claimed the BNPL offer and has agreed to the terms. |
| `initiated-collection` | The collection process for the BNPL payments has begun. |
| `settled-merchant` | Funds have been settled with the merchant. |
| `failed` | The BNPL transaction has failed for some reason (rejection, timeout, etc.). |
| `completed` | The BNPL transaction has been successfully completed, with all payments made. |

## API Endpoints

### Status Query Endpoint
You can query the current status of a BNPL request using the following endpoint:

```
GET {{base_url}}/bnplrequests/:reference/status
```

#### Parameters
- `:reference` - The unique reference of the BNPL request

#### Response
```json
{
  "status": "initiated",
  "reference": "BNPL-123456789",
  "requestId": "5f8d0e1b2c3d4e5f6a7b8c9d",
  "timestamp": "2025-04-09T12:34:56.789Z"
}
```

#### Status Codes
- `200 OK` - Request successful
- `404 Not Found` - BNPL request not found
- `401 Unauthorized` - Invalid or missing authentication
- `500 Internal Server Error` - Server error

## Implementation Notes
- Webhooks are processed asynchronously through a queue system
- Each webhook notification receives a unique reference in the format: `Vida-bnpl-{transaction_reference}-{shortid}`
- If the `metadata` field was included in the original BNPL request, it will be included in the webhook notification; otherwise, it will be `null`

## Best Practices
1. Implement idempotency in your webhook consumer to handle potential duplicate notifications
2. Acknowledge webhook receipt with a 2xx HTTP response
3. Process webhooks asynchronously on your server
4. Set up proper error handling and retries
5. Validate the webhook data before processing
