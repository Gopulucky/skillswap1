# Entity Relationship Diagram - SkillSwap (RTRP)

Based on the current database schema defined in `db/mysql_init.sql` and `skillswap_report.tex`.

```mermaid
erDiagram
    %% Entities
    USERS {
        int id PK "Auto Increment"
        varchar username UK "Unique"
        varchar email UK "Unique"
        varchar password "Bcrypt Hash"
        text avatar "URL"
        int timeCredits "Default: 10"
        text bio
        varchar role
        varchar location
        varchar website
        boolean is_online "Status"
        timestamp last_seen
        timestamp created_at
    }

    SKILLS {
        int id PK "Auto Increment"
        varchar title
        text description
        varchar category
        decimal hours "Credit Value"
        int user_id FK "References USERS"
        timestamp created_at
    }

    MESSAGES {
        int id PK "Auto Increment"
        int sender_id FK "References USERS"
        int receiver_id FK "References USERS"
        text text "Content"
        timestamp created_at
    }

    %% Relationships
    USERS ||--o{ SKILLS : "creates (One-to-Many)"
    USERS ||--o{ MESSAGES : "sends (One-to-Many)"
    USERS ||--o{ MESSAGES : "receives (One-to-Many)"
```

## Schema Details

### 1. USERS Table
- **Primary Key:** `id`
- **Unique Constraints:** `username`, `email`
- **Description:** Stores user profile information, authentication details, and time credits.

### 2. SKILLS Table
- **Primary Key:** `id`
- **Foreign Key:** `user_id` -> `users.id` (ON DELETE CASCADE)
- **Description:** Represents skills offered by users for exchange.

### 3. MESSAGES Table
- **Primary Key:** `id`
- **Foreign Keys:** 
  - `sender_id` -> `users.id` (ON DELETE CASCADE)
  - `receiver_id` -> `users.id` (ON DELETE CASCADE)
- **Description:** Stores chat history between users.
