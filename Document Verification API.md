# Document Verification API

## Overview

The Document Verification API allows applications to submit identity documents for validation and receive verification results.

This API can be integrated into onboarding workflows where identity verification is required for compliance and security purposes.

Supported document types include:

- Passport
- Driver License
- National ID

---

## Base URL

```
https://api.exampleplatform.com/v1
```

---

## Authentication

All API requests require authentication using an access token.

Include the token in the request header:

```
Authorization: Bearer <access_token>
```

---

## Endpoint

### Submit Document for Verification

```
POST /documents/verify
```

This endpoint submits a document for identity verification.

---

## Request Parameters

| Parameter | Type | Required | Description |
|----------|------|----------|-------------|
| user_id | string | Yes | Unique identifier for the user |
| document_type | string | Yes | Type of document (passport, driver_license, national_id) |
| country_code | string | Yes | ISO country code of the document |
| document_image | file | Yes | Image of the document |

---

## Request Example

```
POST /v1/documents/verify
Content-Type: application/json
Authorization: Bearer <access_token>
```

```json
{
  "user_id": "U102938",
  "document_type": "passport",
  "country_code": "US"
}
```

---

## Response Example

```json
{
  "verification_id": "VER-983274",
  "status": "pending",
  "submitted_at": "2026-03-17T10:30:00Z"
}
```

---

## Check Verification Status

### Endpoint

```
GET /documents/verify/{verification_id}
```

---

## Status Response Example

```json
{
  "verification_id": "VER-983274",
  "status": "verified",
  "verified_at": "2026-03-17T10:32:10Z"
}
```

---

## Status Values

| Status | Description |
|------|-------------|
| pending | Verification is in progress |
| verified | Document successfully verified |
| rejected | Document verification failed |

---

## Error Responses

| HTTP Code | Message | Description |
|----------|---------|-------------|
| 400 | Invalid request | Missing or incorrect parameters |
| 401 | Unauthorized | Invalid or expired token |
| 415 | Unsupported media type | Unsupported file format |
| 429 | Too many requests | Rate limit exceeded |
| 500 | Internal server error | Unexpected processing error |

---

## Rate Limits

The API supports **100 requests per minute per client**.

Requests exceeding this limit will return:

```
429 Too Many Requests
```

---

## Supported File Formats

- JPG
- PNG
- PDF

Maximum file size: **10 MB**

---

## Typical Workflow

1. Client submits a document for verification.
2. The API generates a verification ID.
3. The verification service processes the document.
4. The client checks the verification status using the verification ID.
5. The API returns the final verification result.

---

## Notes

- Images should be clear and readable.
- Blurry or cropped documents may result in verification failure.
- Verification results are typically available within a few seconds.
