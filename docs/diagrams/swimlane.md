# Swimlane Diagram: Phiên thi vấn đáp AI thích ứng

```mermaid
flowchart TD
    subgraph Lane_Student["Làn 1: Sinh viên (Examinee)"]
        S1([Bắt đầu phòng thi])
        S2[Lắng nghe AI đọc câu hỏi]
        S3[Trả lời câu hỏi qua Micro]
        S4([Kết thúc bài thi])
    end

    subgraph Lane_Client["Làn 2: Web Client (Frontend / WebRTC)"]
        C1[Kích hoạt Micro / Camera]
        C2[Phát âm thanh câu hỏi ra loa]
        C3[Stream Audio Chunk thời gian thực]
    end

    subgraph Lane_Backend["Làn 3: Core Backend"]
        B1[Xác thực & Tạo Session ID]
        B2[Truy xuất câu hỏi chính từ DB]
        B3[Ghi nhận Transcript & Audit Log]
        B4{Cần hỏi xoáy VÀ\nLượt xoáy < Max?}
        B5{Còn câu hỏi\ntrong ca thi?}
        B6[Đóng phiên & Lưu bản ghi âm toàn bài]
    end

    subgraph Lane_AI["Làn 4: AI Viva Engine (STT / LLM / TTS)"]
        A1[TTS: Chuyển văn bản thành giọng nói]
        A2[STT: Chuyển giọng nói sang Text]
        A3[LLM: Đánh giá ý & So khớp Rubric]
        A4[LLM: Sinh câu hỏi xoáy đào sâu]
    end

    S1 --> C1 --> B1 --> B2 --> A1 --> C2 --> S2 --> S3 --> C3 --> A2 --> A3 --> B3 --> B4
    B4 -- Đúng: Cần làm rõ --> A4 --> A1
    B4 -- Sai: Đủ ý hoặc hết lượt --> B5
    B5 -- Còn câu tiếp theo --> B2
    B5 -- Hết câu hỏi --> B6 --> S4
