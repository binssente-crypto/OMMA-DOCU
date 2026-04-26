```mermaid
erDiagram
    USER {
        string id PK
        string email
        string role "CUSTOMER | ARTIST | ADMIN"
        string displayName
        string avatarUrl
        boolean isVerified
        string paymentGatewayAccountId
        datetime createdAt
    }

    ARTWORK {
        string id PK
        string artistId FK
        string title
        string imageUrl
        string pHash
        string authStatus "PENDING | VERIFIED | FLAGGED"
        string tags
        boolean isPublic
        string commissionId FK
        datetime createdAt
    }

    COMMISSION {
        string id PK
        string customerId FK
        string artistId FK
        string status "PENDING | IN_PROGRESS | COMPLETED"
        json brief
        decimal price
        decimal platformFee
        int revisionsAllowed
        string paymentGatewayPaymentId
        datetime deadline
        datetime createdAt
    }

    MESSAGE {
        string id PK
        string commissionId FK
        string senderId FK
        string content
        string attachmentUrl
        boolean isRead
        datetime sentAt
    }

    TRANSACTION {
        string id PK
        string commissionId FK
        string type "DEPOSIT | RELEASE | REFUND"
        decimal amount
        string status "PENDING | COMPLETED | FAILED"
        string paymentGatewayEventId
        datetime createdAt
    }

    FORUM_POST {
        string id PK
        string authorId FK
        string content
        string mediaUrl
        string parentId FK
        int likeCount
        int repostCount
        datetime createdAt
    }

    USER ||--o{ ARTWORK : "creates"
    USER ||--o{ COMMISSION : "requests as customer"
    USER ||--o{ COMMISSION : "fulfills as artist"
    USER ||--o{ MESSAGE : "sends"
    USER ||--o{ FORUM_POST : "authors"
    COMMISSION ||--o{ MESSAGE : "contains"
    COMMISSION ||--o{ TRANSACTION : "generates"
    COMMISSION ||--o| ARTWORK : "produces"
    FORUM_POST ||--o{ FORUM_POST : "has replies"
