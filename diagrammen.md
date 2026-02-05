sequenceDiagram
    participant S as Speler (Frontend)
    participant API as REST API (Backend)
    participant GE as Game Engine
    participant DB as MongoDB
    participant WS as WebSocket Server

    S->>API: Plaatst inzet (POST /play)
    API->>GE: Valideer inzet + bereken resultaat
    GE->>DB: Sla spelresultaat op
    GE-->>API: Resultaat + nieuw saldo
    API-->>S: Response met resultaat
    API->>WS: Push saldo-update
    WS-->>S: Real-time update saldo + melding


    flowchart LR

    subgraph Client ["Frontend (Browser)"]
        UI[Vue.js UI]
        WSClient[WebSocket Client]
    end

    subgraph Backend ["Backend (Node.js / Express)"]
        API[REST API]
        GameEngine[Game Engine (RNG + logica)]
        WS[WebSocket Server]
    end

    subgraph Database ["Database Layer"]
        Mongo[(MongoDB)]
        Redis[(Redis Cache)]
    end

    UI --> API
    WSClient <--> WS

    API --> Mongo
    API --> Redis

    API --> GameEngine
    GameEngine --> Mongo

    WS --> UI