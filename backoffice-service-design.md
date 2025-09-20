# Backoffice Service Design

## 6.2 - Contract Documentation

### Authentication & Authorization

### LOGIN

**method:** POST  
**path:** `/v1/auth/login`

User authentication endpoint for identifying current user session and fetching user data with tenant context. Response must return the userId, token, tenantId and refresh token.

1. username, password and tenantId are required fields

**request:**

```json
{
  "username": "string",
  "password": "string",
  "tenantId": "string"
}
```

**response:**

```json
{
  "userId": "string",
  "token": "string",
  "tenantId": "string",
  "refreshToken": "string",
  "expiresIn": "integer"
}
```

---

### LOGOUT

**method:** POST  
**path:** `/v1/auth/logout`

Invalidate user session and revoke authentication tokens. Must clean up session data from Cassandra.

1. userId and token are required fields

**request:**

```json
{
  "userId": "string",
  "token": "string"
}
```

**response:**

```json
{
  "success": "boolean",
  "message": "string"
}
```

---

### REFRESH TOKEN

**method:** POST  
**path:** `/v1/auth/refresh`

Refresh authentication token using valid refresh token. Returns new access token and optionally new refresh token.

1. refreshToken and userId are required fields

**request:**

```json
{
  "refreshToken": "string",
  "userId": "string"
}
```

**response:**

```json
{
  "token": "string",
  "refreshToken": "string",
  "expiresIn": "integer"
}
```

---

## User Management

### REGISTER USER

**method:** POST  
**path:** `/v1/users/register`

Register new user with tenant association. Credentials encrypted and stored in AWS KMS Secrets. User profile stored in Keyspace.

1. username, password, email, fullName, tenantId and role are required fields
2. Password must meet security requirements (min 8 chars, uppercase, lowercase, number, special char)
3. Email must be unique within tenant

**request:**

```json
{
  "username": "string",
  "password": "string",
  "email": "string",
  "fullName": "string",
  "tenantId": "string",
  "role": "string"
}
```

**response:**

```json
{
  "userId": "string",
  "username": "string",
  "email": "string",
  "tenantId": "string",
  "createdAt": "timestamp"
}
```

---

### GET USER PROFILE

**method:** GET  
**path:** `/v1/users/{userId}`

Retrieve user profile information from Keyspace. Must validate tenant access.

**response:**

```json
{
  "userId": "string",
  "username": "string",
  "email": "string",
  "fullName": "string",
  "tenantId": "string",
  "role": "string",
  "createdAt": "timestamp",
  "lastLogin": "timestamp"
}
```

---

## POC Management

### CREATE POC

**method:** POST  
**path:** `/v1/pocs`

Register new Proof of Concept in the system. Data stored in Keyspace with partition key as tenantId and clustering key as pocId.

1. title, description, language, tags, userId and tenantId are required fields
2. Tags must be an array with at least one tag
3. Language must be from supported languages list

**request:**

```json
{
  "title": "string",
  "description": "string",
  "language": "string",
  "tags": ["string"],
  "repositoryUrl": "string",
  "documentation": "string",
  "userId": "string",
  "tenantId": "string"
}
```

**response:**

```json
{
  "pocId": "string",
  "title": "string",
  "language": "string",
  "tags": ["string"],
  "createdAt": "timestamp",
  "status": "string"
}
```

---

### SEARCH POCS

**method:** GET  
**path:** `/v1/pocs/search`

Search POCs by name, language, or tags with pagination. Uses Cassandra secondary indexes for efficient querying.

**Query Parameters:**

- `name`: string (optional) - Partial match on POC title
- `language`: string (optional) - Exact match on programming language
- `tags`: string array (optional) - Match any of the provided tags
- `tenantId`: string (required) - Tenant identifier for data isolation
- `page`: integer (default: 1) - Page number for pagination
- `limit`: integer (default: 20, max: 100) - Number of results per page

**response:**

```json
{
  "pocs": [
    {
      "pocId": "string",
      "title": "string",
      "description": "string",
      "language": "string",
      "tags": ["string"],
      "userId": "string",
      "createdAt": "timestamp"
    }
  ],
  "totalCount": "integer",
  "page": "integer",
  "totalPages": "integer"
}
```

---

### GET POC DETAILS

**method:** GET  
**path:** `/v1/pocs/{pocId}`

Retrieve detailed information about a specific POC including metrics. Increments view counter.

**response:**

```json
{
  "pocId": "string",
  "title": "string",
  "description": "string",
  "language": "string",
  "tags": ["string"],
  "repositoryUrl": "string",
  "documentation": "string",
  "userId": "string",
  "tenantId": "string",
  "createdAt": "timestamp",
  "updatedAt": "timestamp",
  "metrics": {
    "views": "integer",
    "favorites": "integer"
  }
}
```

---

### UPDATE POC

**method:** PUT  
**path:** `/v1/pocs/{pocId}`

Update existing POC information. Only POC owner or admin can update.

1. At least one field must be provided for update

**request:**

```json
{
  "title": "string",
  "description": "string",
  "language": "string",
  "tags": ["string"],
  "repositoryUrl": "string",
  "documentation": "string"
}
```

**response:**

```json
{
  "pocId": "string",
  "title": "string",
  "updatedAt": "timestamp",
  "success": "boolean"
}
```

---

### DELETE POC

**method:** DELETE  
**path:** `/v1/pocs/{pocId}`

Remove POC from the system. Performs soft delete by updating status. Only POC owner or admin can delete.

**response:**

```json
{
  "success": "boolean",
  "message": "string"
}
```

---

## Favorites Management

### ADD TO FAVORITES

**method:** POST  
**path:** `/v1/favorites`

Add POC to user's favorites list. Creates entry in favorites table with composite key.

1. pocId and userId are required fields
2. Cannot add same POC to favorites twice

**request:**

```json
{
  "pocId": "string",
  "userId": "string"
}
```

**response:**

```json
{
  "favoriteId": "string",
  "pocId": "string",
  "userId": "string",
  "createdAt": "timestamp"
}
```

---

### GET USER FAVORITES

**method:** GET  
**path:** `/v1/favorites/user/{userId}`

Retrieve all favorite POCs for a user. Joins with POC table to return full POC details.

**response:**

```json
{
  "favorites": [
    {
      "favoriteId": "string",
      "pocId": "string",
      "pocTitle": "string",
      "language": "string",
      "tags": ["string"],
      "addedAt": "timestamp"
    }
  ],
  "totalCount": "integer"
}
```

---

### REMOVE FROM FAVORITES

**method:** DELETE  
**path:** `/v1/favorites/{favoriteId}`

Remove POC from user's favorites. User can only remove their own favorites.

**response:**

```json
{
  "success": "boolean",
  "message": "string"
}
```

---

## Reports Generation

### GENERATE POC REPORT

**method:** POST  
**path:** `/v1/reports/generate`

Generate analytical report for POCs. Publishes event to AWS MSK (Kafka) for async processing.

1. reportType, dateRange and tenantId are required fields
2. Valid reportTypes: "summary", "detailed", "analytics", "usage"
3. Date range cannot exceed 1 year

**request:**

```json
{
  "reportType": "string",
  "dateRange": {
    "startDate": "date",
    "endDate": "date"
  },
  "filters": {
    "language": "string",
    "tags": ["string"],
    "userId": "string"
  },
  "tenantId": "string",
  "format": "string"
}
```

**response:**

```json
{
  "reportId": "string",
  "status": "pending",
  "estimatedTime": "integer",
  "message": "string"
}
```

---

### GET REPORT STATUS

**method:** GET  
**path:** `/v1/reports/{reportId}/status`

Check report generation status. Polls Kafka consumer group for processing status.

**response:**

```json
{
  "reportId": "string",
  "status": "string",
  "progress": "integer",
  "downloadUrl": "string",
  "completedAt": "timestamp"
}
```

---

### DOWNLOAD REPORT

**method:** GET  
**path:** `/v1/reports/{reportId}/download`

Download generated report file. Returns S3 presigned URL or streams file directly.

**response:**

- Binary file stream with appropriate Content-Type header
- OR redirect (302) to S3 presigned URL

---

## Video Generation

### GENERATE YEARLY VIDEO

**method:** POST  
**path:** `/v1/videos/generate-yearly`

Generate video compilation of user's POCs for the specified year. Triggers async processing via Kafka.

1. userId, year and tenantId are required fields
2. Year must be between 2020 and current year
3. User must have at least 1 POC in the specified year

**request:**

```json
{
  "userId": "string",
  "year": "integer",
  "includeMetrics": "boolean",
  "template": "string",
  "tenantId": "string"
}
```

**response:**

```json
{
  "videoId": "string",
  "status": "queued",
  "estimatedDuration": "integer",
  "message": "string"
}
```

---

### GET VIDEO STATUS

**method:** GET  
**path:** `/v1/videos/{videoId}/status`

Check video generation progress. Returns current processing status and download URL when complete.

**response:**

```json
{
  "videoId": "string",
  "status": "string",
  "progress": "integer",
  "downloadUrl": "string",
  "thumbnailUrl": "string",
  "completedAt": "timestamp"
}
```

---

## Real-time Dojo Support

### CREATE DOJO SESSION

**method:** POST  
**path:** `/v1/dojos/sessions`

Initialize real-time collaborative coding session. Creates dedicated Kafka topic for real-time event streaming.

1. title, pocId, hostUserId and tenantId are required fields
2. maxParticipants must be between 2 and 50
3. scheduledAt must be future timestamp

**request:**

```json
{
  "title": "string",
  "description": "string",
  "pocId": "string",
  "hostUserId": "string",
  "maxParticipants": "integer",
  "scheduledAt": "timestamp",
  "tenantId": "string"
}
```

**response:**

```json
{
  "sessionId": "string",
  "kafkaTopic": "string",
  "websocketUrl": "string",
  "joinCode": "string",
  "status": "created"
}
```

---

### JOIN DOJO SESSION

**method:** POST  
**path:** `/v1/dojos/sessions/{sessionId}/join`

Join existing dojo session. Validates join code and session capacity.

1. userId and joinCode are required fields
2. Session must be active and not full

**request:**

```json
{
  "userId": "string",
  "joinCode": "string"
}
```

**response:**

```json
{
  "sessionId": "string",
  "websocketUrl": "string",
  "kafkaTopic": "string",
  "participantToken": "string"
}
```

---

### GET DOJO SESSION STATUS

**method:** GET  
**path:** `/v1/dojos/sessions/{sessionId}`

Retrieve current dojo session information including participant list.

**response:**

```json
{
  "sessionId": "string",
  "title": "string",
  "status": "string",
  "participants": [
    {
      "userId": "string",
      "username": "string",
      "role": "string",
      "joinedAt": "timestamp"
    }
  ],
  "startedAt": "timestamp",
  "pocId": "string"
}
```

---

### END DOJO SESSION

**method:** POST  
**path:** `/v1/dojos/sessions/{sessionId}/end`

Terminate dojo session and cleanup resources. Only host can end session.

1. hostUserId is required field
2. Must match session creator

**request:**

```json
{
  "hostUserId": "string"
}
```

**response:**

```json
{
  "success": "boolean",
  "sessionSummary": {
    "duration": "integer",
    "participantCount": "integer",
    "recordingUrl": "string"
  }
}
```

---

## Tenant Management

### CREATE TENANT

**method:** POST  
**path:** `/v1/tenants`

Register new tenant in the multi-tenant system. Creates isolated data partition in Keyspace.

1. name, domain, contactEmail and plan are required fields
2. Domain must be unique across system
3. Valid plans: "standard", "premium", "enterprise"

**request:**

```json
{
  "name": "string",
  "domain": "string",
  "contactEmail": "string",
  "plan": "string",
  "configuration": {
    "maxUsers": "integer",
    "maxPocs": "integer",
    "features": ["string"]
  }
}
```

**response:**

```json
{
  "tenantId": "string",
  "name": "string",
  "domain": "string",
  "status": "active",
  "createdAt": "timestamp"
}
```

---

### GET TENANT INFO

**method:** GET  
**path:** `/v1/tenants/{tenantId}`

Retrieve tenant information, configuration and current usage metrics.

**response:**

```json
{
  "tenantId": "string",
  "name": "string",
  "domain": "string",
  "plan": "string",
  "status": "string",
  "configuration": {
    "maxUsers": "integer",
    "maxPocs": "integer",
    "features": ["string"]
  },
  "usage": {
    "userCount": "integer",
    "pocCount": "integer",
    "storageUsed": "integer"
  }
}
```

---

## Notification Service

### SEND EMAIL NOTIFICATION

**method:** POST  
**path:** `/v1/notifications/email`

Send email notifications via AWS SES. Uses predefined templates stored in SES.

1. recipients, templateId and tenantId are required fields
2. Recipients array cannot exceed 50 emails
3. Template must exist in SES

**request:**

```json
{
  "recipients": ["string"],
  "templateId": "string",
  "subject": "string",
  "data": {
    "key": "value"
  },
  "tenantId": "string"
}
```

**response:**

```json
{
  "notificationId": "string",
  "status": "sent",
  "messageId": "string"
}
```

## 6.3 - Persistence Model

### User

|   NAME    |   TYPE    | SIZE | NOT NULL |      DEFAULT      |              DESCRIPTION              |
| :-------: | :-------: | :--: | :------: | :---------------: | :-----------------------------------: |
|    id     |   uuid    |      |    NO    |                   |           uuid primary key            |
| username  |  varchar  |  50  |    NO    |                   | username must be unique within tenant |
| password  |  varchar  | 255  |    NO    |                   |   encrypted password stored in KMS    |
|   email   |  varchar  | 100  |    NO    |                   |  email must be unique within tenant   |
| fullName  |  varchar  | 100  |   YES    |                   |           user's full name            |
| tenantId  |   uuid    |      |    NO    |                   |  tenant identifier for multi-tenancy  |
|   role    |  varchar  |  20  |    NO    |                   |    user role (admin, user, viewer)    |
| isActive  |  boolean  |      |    NO    |       true        |         account active status         |
| lastLogin | timestamp |      |   YES    |                   |         last login timestamp          |
|  created  | timestamp |      |    NO    | current_timestamp |                                       |
|  updated  | timestamp |      |    NO    | current_timestamp |                                       |

### Tenant

|     NAME     |   TYPE    | SIZE | NOT NULL |      DEFAULT      |                    DESCRIPTION                    |
| :----------: | :-------: | :--: | :------: | :---------------: | :-----------------------------------------------: |
|      id      |   uuid    |      |    NO    |                   |                 uuid primary key                  |
|     name     |  varchar  | 100  |    NO    |                   |             tenant organization name              |
|    domain    |  varchar  | 100  |    NO    |                   |             unique domain identifier              |
| contactEmail |  varchar  | 100  |    NO    |                   |               primary contact email               |
|     plan     |  varchar  |  20  |    NO    |                   | subscription plan (standard, premium, enterprise) |
|   maxUsers   |    int    |      |    NO    |        10         |               maximum allowed users               |
|   maxPocs    |    int    |      |    NO    |        100        |               maximum allowed POCs                |
|   features   |   text    |      |   YES    |                   |          JSON array of enabled features           |
|    status    |  varchar  |  20  |    NO    |      active       |   tenant status (active, suspended, cancelled)    |
| storageQuota |  bigint   |      |    NO    |    10737418240    |       storage quota in bytes (default 10GB)       |
|   created    | timestamp |      |    NO    | current_timestamp |                                                   |
|   updated    | timestamp |      |    NO    | current_timestamp |                                                   |

### POC

|     NAME      |   TYPE    | SIZE | NOT NULL |      DEFAULT      |              DESCRIPTION               |
| :-----------: | :-------: | :--: | :------: | :---------------: | :------------------------------------: |
|      id       |   uuid    |      |    NO    |                   |            uuid primary key            |
|     title     |  varchar  | 200  |    NO    |                   |               POC title                |
|  description  |   text    |      |   YES    |                   |            POC description             |
|   language    |  varchar  |  50  |    NO    |                   |          programming language          |
|     tags      |   text    |      |    NO    |                   |           JSON array of tags           |
| repositoryUrl |  varchar  | 500  |   YES    |                   |           git repository URL           |
| documentation |   text    |      |   YES    |                   |         markdown documentation         |
|    userId     |   uuid    |      |    NO    |                   |       FK to User.id (POC owner)        |
|   tenantId    |   uuid    |      |    NO    |                   |            FK to Tenant.id             |
|    status     |  varchar  |  20  |    NO    |      active       | POC status (active, archived, deleted) |
|   viewCount   |    int    |      |    NO    |         0         |            number of views             |
| favoriteCount |    int    |      |    NO    |         0         |          number of favorites           |
|    created    | timestamp |      |    NO    | current_timestamp |                                        |
|    updated    | timestamp |      |    NO    | current_timestamp |                                        |

### Favorite

|   NAME   |   TYPE    | SIZE | NOT NULL |      DEFAULT      |   DESCRIPTION    |
| :------: | :-------: | :--: | :------: | :---------------: | :--------------: |
|    id    |   uuid    |      |    NO    |                   | uuid primary key |
|  pocId   |   uuid    |      |    NO    |                   |   FK to POC.id   |
|  userId  |   uuid    |      |    NO    |                   |  FK to User.id   |
| tenantId |   uuid    |      |    NO    |                   | FK to Tenant.id  |
| created  | timestamp |      |    NO    | current_timestamp |                  |

### Report

|     NAME      |   TYPE    | SIZE | NOT NULL |      DEFAULT      |                    DESCRIPTION                    |
| :-----------: | :-------: | :--: | :------: | :---------------: | :-----------------------------------------------: |
|      id       |   uuid    |      |    NO    |                   |                 uuid primary key                  |
|  reportType   |  varchar  |  20  |    NO    |                   | report type (summary, detailed, analytics, usage) |
|   startDate   |   date    |      |    NO    |                   |                report period start                |
|    endDate    |   date    |      |    NO    |                   |                 report period end                 |
|    filters    |   text    |      |   YES    |                   |         JSON object with filter criteria          |
|    format     |  varchar  |  10  |    NO    |        pdf        |          output format (pdf, xlsx, csv)           |
|    status     |  varchar  |  20  |    NO    |      pending      |                 generation status                 |
|   progress    |    int    |      |    NO    |         0         |          generation progress percentage           |
|    fileUrl    |  varchar  | 500  |   YES    |                   |            S3 URL for completed report            |
|    userId     |   uuid    |      |    NO    |                   |             FK to User.id (requester)             |
|   tenantId    |   uuid    |      |    NO    |                   |                  FK to Tenant.id                  |
| estimatedTime |    int    |      |   YES    |                   |       estimated completion time in seconds        |
|  completedAt  | timestamp |      |   YES    |                   |               completion timestamp                |
|    created    | timestamp |      |    NO    | current_timestamp |                                                   |

### Video

|       NAME        |   TYPE    | SIZE | NOT NULL |      DEFAULT      |          DESCRIPTION           |
| :---------------: | :-------: | :--: | :------: | :---------------: | :----------------------------: |
|        id         |   uuid    |      |    NO    |                   |        uuid primary key        |
|      userId       |   uuid    |      |    NO    |                   |         FK to User.id          |
|     tenantId      |   uuid    |      |    NO    |                   |        FK to Tenant.id         |
|       year        |    int    |      |    NO    |                   |   year for video compilation   |
|  includeMetrics   |  boolean  |      |    NO    |       false       |  include POC metrics in video  |
|     template      |  varchar  |  50  |    NO    |      default      |      video template style      |
|      status       |  varchar  |  20  |    NO    |      queued       |       processing status        |
|     progress      |    int    |      |    NO    |         0         | generation progress percentage |
|     duration      |    int    |      |   YES    |                   |   video duration in seconds    |
|      fileUrl      |  varchar  | 500  |   YES    |                   |   S3 URL for completed video   |
|   thumbnailUrl    |  varchar  | 500  |   YES    |                   |   S3 URL for video thumbnail   |
| estimatedDuration |    int    |      |   YES    |                   |   estimated processing time    |
|    completedAt    | timestamp |      |   YES    |                   |      completion timestamp      |
|      created      | timestamp |      |    NO    | current_timestamp |                                |

### DojoSession

|      NAME       |   TYPE    | SIZE | NOT NULL |      DEFAULT      |               DESCRIPTION               |
| :-------------: | :-------: | :--: | :------: | :---------------: | :-------------------------------------: |
|       id        |   uuid    |      |    NO    |                   |            uuid primary key             |
|      title      |  varchar  | 200  |    NO    |                   |              session title              |
|   description   |   text    |      |   YES    |                   |           session description           |
|      pocId      |   uuid    |      |    NO    |                   |              FK to POC.id               |
|   hostUserId    |   uuid    |      |    NO    |                   |      FK to User.id (session host)       |
|    tenantId     |   uuid    |      |    NO    |                   |             FK to Tenant.id             |
| maxParticipants |    int    |      |    NO    |        10         |      maximum allowed participants       |
|   kafkaTopic    |  varchar  | 100  |    NO    |                   |    Kafka topic for real-time events     |
|  websocketUrl   |  varchar  | 500  |    NO    |                   |        WebSocket connection URL         |
|    joinCode     |  varchar  |  10  |    NO    |                   |      unique join code for session       |
|     status      |  varchar  |  20  |    NO    |      created      | session status (created, active, ended) |
|   scheduledAt   | timestamp |      |   YES    |                   |          scheduled start time           |
|    startedAt    | timestamp |      |   YES    |                   |            actual start time            |
|     endedAt     | timestamp |      |   YES    |                   |            session end time             |
|  recordingUrl   |  varchar  | 500  |   YES    |                   |      S3 URL for session recording       |
|     created     | timestamp |      |    NO    | current_timestamp |                                         |

### DojoParticipant

|       NAME       |   TYPE    | SIZE | NOT NULL |      DEFAULT      |                   DESCRIPTION                   |
| :--------------: | :-------: | :--: | :------: | :---------------: | :---------------------------------------------: |
|        id        |   uuid    |      |    NO    |                   |                uuid primary key                 |
|    sessionId     |   uuid    |      |    NO    |                   |              FK to DojoSession.id               |
|      userId      |   uuid    |      |    NO    |                   |                  FK to User.id                  |
|       role       |  varchar  |  20  |    NO    |    participant    | participant role (host, moderator, participant) |
| participantToken |  varchar  | 255  |    NO    |                   |            unique participant token             |
|     joinedAt     | timestamp |      |    NO    | current_timestamp |                                                 |
|      leftAt      | timestamp |      |   YES    |                   |             participant leave time              |

### AuditLog

|    NAME    |   TYPE    | SIZE | NOT NULL |      DEFAULT      |              DESCRIPTION               |
| :--------: | :-------: | :--: | :------: | :---------------: | :------------------------------------: |
|     id     |   uuid    |      |    NO    |                   |            uuid primary key            |
|   userId   |   uuid    |      |   YES    |                   | FK to User.id (null for system events) |
|  tenantId  |   uuid    |      |    NO    |                   |            FK to Tenant.id             |
|   action   |  varchar  |  50  |    NO    |                   |            action performed            |
| entityType |  varchar  |  50  |    NO    |                   | entity type (user, poc, report, etc.)  |
|  entityId  |   uuid    |      |   YES    |                   |           entity identifier            |
| ipAddress  |  varchar  |  45  |   YES    |                   |           client IP address            |
| userAgent  |  varchar  | 500  |   YES    |                   |           client user agent            |
| requestId  |  varchar  |  50  |    NO    |                   |          request tracking ID           |
|  metadata  |   text    |      |   YES    |                   |    JSON object with additional data    |
|  created   | timestamp |      |    NO    | current_timestamp |                                        |

### Notification

|     NAME     |   TYPE    | SIZE | NOT NULL |      DEFAULT      |            DESCRIPTION             |
| :----------: | :-------: | :--: | :------: | :---------------: | :--------------------------------: |
|      id      |   uuid    |      |    NO    |                   |          uuid primary key          |
|   tenantId   |   uuid    |      |    NO    |                   |          FK to Tenant.id           |
|     type     |  varchar  |  20  |    NO    |                   | notification type (email, webhook) |
|  recipients  |   text    |      |    NO    |                   |      JSON array of recipients      |
|  templateId  |  varchar  |  50  |   YES    |                   |      SES template identifier       |
|   subject    |  varchar  | 200  |   YES    |                   |           email subject            |
|     data     |   text    |      |   YES    |                   |   JSON object with template data   |
|    status    |  varchar  |  20  |    NO    |      pending      |          delivery status           |
|  messageId   |  varchar  | 100  |   YES    |                   |           SES message ID           |
| errorMessage |   text    |      |   YES    |                   |      error details if failed       |
|    sentAt    | timestamp |      |   YES    |                   |       actual send timestamp        |
|   created    | timestamp |      |    NO    | current_timestamp |                                    |

### Session

|     NAME     |   TYPE    | SIZE | NOT NULL |      DEFAULT      |      DESCRIPTION      |
| :----------: | :-------: | :--: | :------: | :---------------: | :-------------------: |
|      id      |   uuid    |      |    NO    |                   |   uuid primary key    |
|    userId    |   uuid    |      |    NO    |                   |     FK to User.id     |
|    token     |  varchar  | 500  |    NO    |                   |   JWT access token    |
| refreshToken |  varchar  | 500  |    NO    |                   |   JWT refresh token   |
|  ipAddress   |  varchar  |  45  |   YES    |                   |   client IP addres    |
|  userAgent   |  varchar  | 500  |   YES    |                   |   client user agent   |
|  expiresAt   | timestamp |      |    NO    |                   | token expiration time |
|   isActive   |  boolean  |      |    NO    |       true        | session active status |
|   created    | timestamp |      |    NO    | current_timestamp |                       |
|   updated    | timestamp |      |    NO    | current_timestamp |                       |

## 6.4 - Algorithms/Data Structures

### Message Queues

- **Queue for Report Generation** - FIFO queue using Kafka for async report processing with retry mechanism and dead letter queue
- **Queue for Video Generation** - Priority queue for video compilation jobs, prioritized by tenant tier and estimated duration
- **Queue for Real-time Messages** - Pub/Sub queue for Dojo session collaboration using Kafka with WebSocket broadcasting
- **Queue for Email Notifications** - Standard queue for batch email processing via AWS SES with rate limiting per tenant
- **Queue for POC CRUD Events** - Event stream with log compaction for create/update/delete operations and cache invalidation

### Caching Structures

- **LRU Cache** - Least Recently Used cache for frequently accessed POC data (10K entries, 5-minute TTL)
- **Bloom Filter** - Probabilistic structure for duplicate POC title detection (0.01% false positive rate)
- **Distributed Cache** - Redis-based cache layer for session data and user preferences

### Search & Indexing

- **Trie (Prefix Tree)** - Autocomplete suggestions for POC search by title
- **Inverted Index** - Full-text search on POC descriptions and documentation
- **Secondary Indexes** - Cassandra indexes on language, tags, and tenant for efficient filtering

### Background Processing

- **Priority Queue** - Min-heap for job scheduling based on tenant tier and wait time
- **Batch Processor** - Aggregate operations for reports (100 items), emails (25 items), and DB writes (50 items)
- **Circuit Breaker** - Fault tolerance for external services (SES, S3) with exponential backoff
