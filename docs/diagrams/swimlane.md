# Swimlane Diagram: Real-Time Adaptive Viva Examination

```mermaid
flowchart TD
    subgraph Lane_Student["Student (Examinee)"]
        S1([Join Exam Session])
        S2[Listen to AI Question]
        S3[Provide Verbal Answer via Mic]
        S4([Exam Completed])
    end

    subgraph Lane_Client["Web Client (Frontend / WebRTC)"]
        C1[Initialize Media Devices]
        C2[Playback Question Audio]
        C3[Stream Real-time Audio Chunks]
    end

    subgraph Lane_Backend["Core Backend (API Gateway & Session Service)"]
        B1[Authenticate & Initialize Session]
        B2[Fetch Main Question from DB]
        B3[Log Dialogue Transcript & Turn Events]
        B4{Probing Required AND\nTurn Count < Max Limit?}
        B5{Remaining Questions\nin Exam Session?}
        B6[Close Session & Archive Full Audio/Video]
    end

    subgraph Lane_AI["AI Viva Engine (STT / LLM / TTS)"]
        A1[TTS: Synthesize Speech from Text]
        A2[STT: Convert Audio Stream to Text]
        A3[LLM: Analyze Content against Rubric]
        A4[LLM: Generate Adaptive Probing Question]
    end

    S1 --> C1
    C1 --> B1
    B1 --> B2
    B2 --> A1
    A1 --> C2
    C2 --> S2
    S2 --> S3
    S3 --> C3
    C3 --> A2
    A2 --> A3
    A3 --> B3
    B3 --> B4

    %% Probing condition
    B4 -- Yes: Clarification Needed --> A4
    A4 --> A1

    %% Next Question or Finish
    B4 -- No: Satisfied / Limit Reached --> B5
    B5 -- Next Question Exists --> B2
    B5 -- All Questions Answered --> B6
    B6 --> S4
