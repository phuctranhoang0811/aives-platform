# Use Case Diagram: AI-powered Viva Exam System (AIVES)

![AIVES Use Case Diagram](https://www.plantuml.com/plantuml/svg/bLJDRnin4BtxLt3g3dYm3h58H45wL24-G7e6lD0G7o2L2A6eOca0k8Z9ZqK_p_pxzWfU4uS5a54b32JmXW_6U2YV7kI2aH6V3y7p1k2L4eP8U8K1g8I3w4f2o8M2aX8V3wK1g8P5aH6V3y7p1k2L4eP8U8K1g8I3w4f2o8M2aX8V3wK1g8P5)

### PlantUML Source Code

```plantuml
@startuml
left to right direction
skinparam packageStyle rectangle

actor "Sinh viên" as Student
actor "Giảng viên" as Lecturer
actor "Quản trị viên" as Admin
actor "AI Engine" as AIService <<System>>

rectangle "AIVES - AI Viva Exam System" {
  package "Quản lý Đề & Rubric" {
    usecase "Tải tài liệu & Sinh câu hỏi tự động" as UC_GenQuestions
    usecase "Duyệt / Tinh chỉnh ngân hàng câu hỏi" as UC_ManageQuestions
    usecase "Thiết lập Rubric & Thang Bloom" as UC_SetRubric
  }

  package "Quản lý Kỳ thi" {
    usecase "Thiết lập ca thi & Bộ câu hỏi" as UC_CreateExam
    usecase "Giám sát phiên thi trực tiếp" as UC_MonitorExam
  }

  package "Chấm thi & Đánh giá" {
    usecase "Xem Transcript & Nghe lại ghi âm" as UC_ReviewExam
    usecase "Phê duyệt & Chốt điểm cuối (Human-in-the-loop)" as UC_FinalizeGrade
    usecase "Xem thống kê & Xuất bảng điểm" as UC_ExportReport
  }

  package "Tham gia Thi" {
    usecase "Tham gia phòng vấn đáp ảo" as UC_TakeViva
    usecase "Tương tác thoại với AI Examiner" as UC_VoiceInteraction
    usecase "Xem báo cáo kết quả & Nhận xét" as UC_ViewResult
  }

  package "Hệ thống" {
    usecase "Quản lý tài khoản & Phân quyền" as UC_ManageUsers
    usecase "Cấu hình mô hình STT/TTS/LLM" as UC_ConfigSystem
    usecase "Truy vết nhật ký kiểm toán (Audit Logs)" as UC_AuditLog
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
