# Conceptual ERD: AI-powered Viva Exam System

```mermaid
erDiagram
    USERS ||--o{ EXAM_SESSION : conducts
    USERS ||--o{ STUDENT_EXAM : attends
    COURSE ||--o{ QUESTION : contains
    QUESTION ||--o{ RUBRIC : defines
    QUESTION ||--o{ EXAM_TURN : instantiated_in
    EXAM_SESSION ||--o{ STUDENT_EXAM : includes
    STUDENT_EXAM ||--o{ EXAM_TURN : records_dialogue
    STUDENT_EXAM ||--o| AUDIT_EVIDENCE : generates

    USERS {
        uuid id PK
        string full_name
        string role "Admin | Lecturer | Student"
    }
    STUDENT_EXAM {
        uuid id PK
        uuid session_id FK
        uuid student_id FK
        float ai_suggested_score
        float lecturer_final_score
        string status 
    }
