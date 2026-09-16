```markdown
# Software Architecture Design: AIVES Platform

```mermaid
flowchart TB
    subgraph ClientLayer["1. Presentation Tier (Client)"]
        WebStudent["Next.js WebApp\n(Examinee - WebRTC/WebSocket)"]
        WebLecturer["Next.js WebApp\n(Lecturer & Admin Dashboard)"]
    end

    subgraph GatewayLayer["2. Gateway & Proxy Tier"]
        APIGateway["API Gateway\n(Authentication, Rate Limit, Reverse Proxy)"]
        WSServer["WebSocket / WebRTC Signaling Server"]
    end

    subgraph CoreBusinessLayer["3. Core Business Services"]
        AuthService["Auth & Identity Service\n(RBAC, JWT Session)"]
        ExamService["Exam Session Management Service\n(Session Lifecycle & State Machine)"]
        GradingService["Grading & Audit Service\n(Human-in-the-Loop Review)"]
    end

    subgraph AIEngineLayer["4. AI & Media Processing Subsystem"]
        STTService["Streaming STT Service\n(Real-time Speech Recognition)"]
        VivaOrchestrator["Viva Orchestration Engine\n(Adaptive Probing State Machine)"]
        TTSService["Low-latency TTS Service\n(Voice Synthesis)"]
        RAGModule["RAG & Bloom Evaluator\n(Question Generation & Rubric Mapping)"]
    end

    subgraph StorageLayer["5. Persistence & Storage Tier"]
        MainDB[(Primary Relational DB\nUsers, Questions, Turns, Grades)]
        VectorDB[(Vector Database\nCourse Material Embeddings)]
        CacheQueue[(Redis & Message Broker\nSession State, Stream Buffers, Event Queue)]
        ObjectStore[(Object Storage - S3/MinIO\nAudio Recordings & Video Evidence)]
    end

    ClientLayer --> APIGateway
    WebStudent <--> WSServer
    
    APIGateway --> AuthService
    APIGateway --> ExamService
    APIGateway --> GradingService

    WSServer <--> VivaOrchestrator
    VivaOrchestrator --> STTService
    VivaOrchestrator --> TTSService
    VivaOrchestrator --> RAGModule
    
    ExamService --> MainDB
    GradingService --> MainDB
    RAGModule --> VectorDB
    VivaOrchestrator --> CacheQueue
    WSServer --> ObjectStore
