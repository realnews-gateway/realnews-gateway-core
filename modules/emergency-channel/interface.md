# Emergency Channel Interface

This document defines the interfaces exposed by the Emergency Publishing Channel.

## 1. Submission API

### `POST /submit`
Submit encrypted content for processing.

**Headers**
- `Authorization: Bearer <token>`  
- `X-Anonymous-ID: <randomized-id>`  

**Body**
```json
{
  "payload": "<encrypted-bytes>",
  "type": "text | image | video | document",
  "timestamp": "<client-timestamp>"
}
