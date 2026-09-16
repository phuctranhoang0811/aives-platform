# Software Architecture Design: AIVES Platform

```mermaid
flowchart TB
    subgraph ClientLayer["1. Presentation Tier (Client)"]
        WebStudent["Next.js WebApp<br/>(Examinee - WebRTC/WebSocket)"]
        WebLecturer["Next.js WebApp<br/>(Lecturer & Admin Dashboard)"]
    end

    subgraph GatewayLayer["2. Gateway & Proxy Tier"]
        APIGateway["API Gateway<br/>(Authentication, Rate Limit, Reverse Proxy)"]
        WSServer["WebSocket / WebRTC Signaling Server"]
    end

    subgraph CoreBusinessLayer["3. Core Business Services"]
        AuthService["Auth & Identity Service<br/>(RBAC, JWT Session)"]
        ExamService["Exam Session Management Service<br/>(Session Lifecycle & State Machine)"]
        GradingService["Grading & Audit Service<br/>(Human-in-the-Loop Review)"]
    end

    subgraph AIEngineLayer["4. AI & Media Processing Subsystem"]
        STTService["Streaming STT Service<br/>(Real-time Speech Recognition)"]
        VivaOrchestrator["Viva Orchestration Engine<br/>(Adaptive Probing State Machine)"]
        TTSService["Low-latency TTS Service<br/>(Voice Synthesis)"]
        RAGModule["RAG & Bloom Evaluator<br/>(Question Generation & Rubric Mapping)"]
    end

    subgraph StorageLayer["5. Persistence & Storage Tier"]
        MainDB[("Primary Relational DB<br/>Users, Questions, Turns, Grades")]
        VectorDB[("Vector Database<br/>Course Material Embeddings")]
        CacheQueue[("Redis & Message Broker<br/>Session State, Stream Buffers, Event Queue")]
        ObjectStore[("Object Storage - S3/MinIO<br/>Audio Recordings & Video Evidence")]
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
    VivaOrchestrator --> CacheQueu
