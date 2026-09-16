---

### 2. File `docs/diagrams/use-case.md` (Use Case Diagram)



```markdown
# Use Case Diagram: AI-powered Viva Exam System (AIVES)

```plantuml
@startuml
left to right direction
skinparam packageStyle rectangle

actor "Student" as Student
actor "Lecturer" as Lecturer
actor "Administrator" as Admin
actor "AI Service Engine" as AIService <<System>>

rectangle "AIVES - AI-powered Viva Exam System" {
  package "Question & Rubric Management" {
    usecase "Ingest Documents & Auto-generate Questions (RAG)" as UC_GenQuestions
    usecase "Review & Refine Question Bank" as UC_ManageQuestions
    usecase "Configure Bloom Taxonomy & Rubric Criteria" as UC_SetRubric
  }

  package "Exam Session Administration" {
    usecase "Schedule Exam Session & Configure Question Sets" as UC_CreateExam
    usecase "Monitor Live Exam Sessions" as UC_MonitorExam
  }

  package "Assessment & Human-in-the-Loop Grading" {
    usecase "Review Transcript & Audio Playback" as UC_ReviewExam
    usecase "Adjust & Finalize Grade (Human-in-the-loop)" as UC_FinalizeGrade
    usecase "Export Performance Analytics & Reports" as UC_ExportReport
  }

  package "Examinee Portal" {
    usecase "Join Virtual Viva Room" as UC_TakeViva
    usecase "Conduct Real-time Voice Interaction" as UC_VoiceInteraction
    usecase "View Evaluation Report & Feedback" as UC_ViewResult
  }

  package "System & Infrastructure Administration" {
    usecase "Manage User Accounts & RBAC" as UC_ManageUsers
    usecase "Configure STT / TTS / LLM Model Endpoints" as UC_ConfigSystem
    usecase "Inspect Audit Trails & Session Logs" as UC_AuditLog
  }
}

Lecturer --> UC_GenQuestions
Lecturer --> UC_ManageQuestions
Lecturer --> UC_SetRubric
Lecturer --> UC_CreateExam
Lecturer --> UC_MonitorExam
Lecturer --> UC_ReviewExam
Lecturer --> UC_FinalizeGrade
Lecturer --> UC_ExportReport

Student --> UC_TakeViva
Student --> UC_VoiceInteraction
Student --> UC_ViewResult

Admin --> UC_ManageUsers
Admin --> UC_ConfigSystem
Admin --> UC_AuditLog

UC_GenQuestions ..> AIService : <<include>>
UC_VoiceInteraction ..> AIService : <<include>>
UC_ReviewExam ..> AIService : <<include>>
@enduml
