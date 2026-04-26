erDiagram
    USER {
        string id PK
        string email
        string display_name
        string role
        boolean is_verified
    }

    COMMISSION {
        string id PK
        string customer_id FK
        string artist_id FK
        string status
        decimal agreed_price
        decimal platform_fee
        boolean is_collab
    }

    ARTWORK {
        string id PK
        string artist_id FK
        string title
        string auth_status
        boolean is_public
    }

    MESSAGE {
        string id PK
        string commission_id FK
        string sender_id FK
        string content
        boolean is_read
    }

    TRANSACTION {
        string id PK
        string commission_id FK
        string type
        decimal amount
        string status
    }

    FORUM_POST {
        string id PK
        string author_id FK
        string parent_id FK
        string content
    }

    WALLET {
        string id PK
        string user_id FK
        decimal balance
    }

    USER ||--o{ COMMISSION : "requests / fulfills"
    USER ||--o{ ARTWORK : "creates"
    USER ||--o{ MESSAGE : "sends"
    USER ||--o{ FORUM_POST : "authors"
    USER ||--o| WALLET : "owns"
    COMMISSION ||--o{ MESSAGE : "has"
    COMMISSION ||--o{ TRANSACTION : "generates"
    COMMISSION ||--o{ ARTWORK : "produces"
    FORUM_POST ||--o{ FORUM_POST : "replies to"
