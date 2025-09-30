# Infrastructure Diagrams

## System Architecture Overview

```mermaid
graph TB
    subgraph "GitHub Infrastructure"
        GHA[GitHub Actions Runner]
        GCR[GitHub Container Registry]
        GP[GitHub Pages]
        GR[GitHub Repository]
    end
    
    subgraph "External Services"
        TOR[Tor Network]
        RS[Ransomware Sites]
        DC[Discord Webhooks]
        TW[Twitter API]
        MS[MS Teams]
    end
    
    subgraph "Data Flow"
        JSON1[groups.json]
        JSON2[posts.json]
        HTML[HTML Sources]
        MD[Markdown Docs]
        GRAPHS[PNG Graphs]
    end
    
    GHA --> GCR
    GHA --> TOR
    TOR --> RS
    GHA --> JSON1
    GHA --> JSON2
    JSON1 --> HTML
    HTML --> JSON2
    JSON2 --> MD
    JSON2 --> GRAPHS
    MD --> GP
    GRAPHS --> GP
    JSON2 --> DC
    JSON2 --> TW
    JSON2 --> MS
    GHA --> GR
```

## Data Flow Architecture

```mermaid
sequenceDiagram
    participant GA as GitHub Actions
    participant TC as Tor Container
    participant RS as Ransomware Sites
    participant FS as File System
    participant API as JSON APIs
    participant WEB as GitHub Pages
    participant EXT as External Services
    
    GA->>TC: Start Tor Proxy
    GA->>FS: Load groups.json
    loop For Each Group
        GA->>TC: Route through Tor
        TC->>RS: Fetch Site Content
        RS-->>TC: HTML Response
        TC-->>GA: HTML Content
        GA->>FS: Store HTML in /source/
    end
    
    GA->>FS: Run Parsers on HTML
    FS-->>GA: Extracted Victim Data
    GA->>FS: Update posts.json
    
    alt New Posts Found
        GA->>EXT: Send Notifications
    end
    
    GA->>FS: Generate Markdown
    GA->>FS: Create Graphs
    GA->>WEB: Deploy to GitHub Pages
    GA->>API: Expose JSON Endpoints
```

## Component Interaction Diagram

```mermaid
graph LR
    subgraph "Input Layer"
        CRON[Cron Schedule]
        MANUAL[Manual Trigger]
    end
    
    subgraph "Processing Layer"
        SCRAPER[Web Scraper]
        PARSER[Content Parser]
        GENERATOR[Doc Generator]
    end
    
    subgraph "Storage Layer"
        GROUPS[groups.json]
        POSTS[posts.json]
        SOURCE[HTML Files]
    end
    
    subgraph "Output Layer"
        WEBSITE[Static Website]
        JSONAPI[JSON APIs]
        SOCIAL[Social Notifications]
    end
    
    CRON --> SCRAPER
    MANUAL --> SCRAPER
    SCRAPER --> SOURCE
    GROUPS --> SCRAPER
    SOURCE --> PARSER
    PARSER --> POSTS
    POSTS --> GENERATOR
    GENERATOR --> WEBSITE
    POSTS --> JSONAPI
    POSTS --> SOCIAL
```

## Network Architecture

```mermaid
graph TB
    subgraph "GitHub Actions Environment"
        RUNNER[Ubuntu Runner]
        CONTAINER[Python Container]
        TORPROXY[Tor Proxy Container]
    end
    
    subgraph "Tor Network"
        ENTRY[Entry Node]
        MIDDLE[Middle Node]
        EXIT[Exit Node]
    end
    
    subgraph "Dark Web"
        ONION1[Group1.onion]
        ONION2[Group2.onion]
        ONION3[GroupN.onion]
    end
    
    subgraph "Clear Web"
        CLEAR1[Group Sites]
        CDN[Content Delivery]
    end
    
    CONTAINER --> TORPROXY
    TORPROXY --> ENTRY
    ENTRY --> MIDDLE
    MIDDLE --> EXIT
    EXIT --> ONION1
    EXIT --> ONION2
    EXIT --> ONION3
    
    CONTAINER --> CLEAR1
    CLEAR1 --> CDN
```

## Deployment Pipeline

```mermaid
graph LR
    subgraph "Source Control"
        CODE[Source Code]
        CONFIG[Configuration]
    end
    
    subgraph "CI/CD Pipeline"
        BUILD[Docker Build]
        TEST[Code Quality]
        DEPLOY[Auto Deploy]
    end
    
    subgraph "Runtime Environment"
        ACTIONS[GitHub Actions]
        SECRETS[Environment Secrets]
    end
    
    subgraph "Outputs"
        PAGES[GitHub Pages]
        REGISTRY[Container Registry]
        DATA[JSON Data]
    end
    
    CODE --> BUILD
    CONFIG --> BUILD
    BUILD --> TEST
    TEST --> DEPLOY
    DEPLOY --> ACTIONS
    SECRETS --> ACTIONS
    ACTIONS --> PAGES
    ACTIONS --> REGISTRY
    ACTIONS --> DATA
```

## Data Storage Architecture

```mermaid
erDiagram
    GROUPS ||--o{ LOCATIONS : contains
    GROUPS {
        string name
        boolean captcha
        boolean parser
        boolean javascript_render
        string meta
        array profile
    }
    
    LOCATIONS {
        string fqdn
        string title
        int version
        string slug
        boolean available
        datetime updated
        datetime lastscrape
        boolean enabled
    }
    
    POSTS {
        string post_title
        string group_name
        datetime discovered
    }
    
    GROUPS ||--o{ POSTS : generates
```

## Monitoring and Alerting Flow

```mermaid
graph TD
    SCRAPE[Scraping Process] --> PARSE[Parsing Process]
    PARSE --> CHECK{New Posts?}
    CHECK -->|Yes| NOTIFY[Send Notifications]
    CHECK -->|No| END[End Process]
    
    NOTIFY --> DISCORD[Discord Webhook]
    NOTIFY --> TWITTER[Twitter API]
    NOTIFY --> TEAMS[MS Teams Webhook]
    
    DISCORD --> LOG[Activity Log]
    TWITTER --> LOG
    TEAMS --> LOG
    
    LOG --> HEARTBEAT[Uptime Monitor]
    HEARTBEAT --> ALERT{Service Health}
    ALERT -->|Healthy| CONTINUE[Continue Monitoring]
    ALERT -->|Unhealthy| ESCALATE[Alert Administrators]
```

## Security Architecture

```mermaid
graph TB
    subgraph "Security Layers"
        AUTH[GitHub Authentication]
        SECRETS[Encrypted Secrets]
        NETWORK[Network Isolation]
        CONTAINER[Container Isolation]
    end
    
    subgraph "Data Protection"
        ENCRYPT[Data Encryption]
        SANITIZE[Input Sanitization]
        VALIDATE[Data Validation]
    end
    
    subgraph "Access Control"
        REPO[Repository Access]
        ACTIONS[Actions Permissions]
        PAGES[Pages Deployment]
    end
    
    AUTH --> SECRETS
    SECRETS --> NETWORK
    NETWORK --> CONTAINER
    
    CONTAINER --> ENCRYPT
    ENCRYPT --> SANITIZE
    SANITIZE --> VALIDATE
    
    VALIDATE --> REPO
    REPO --> ACTIONS
    ACTIONS --> PAGES
```