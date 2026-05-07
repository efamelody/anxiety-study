# README: Project Locus (The Certainty Map)

## Overview
**Locus** is a high-performance, AI-driven study platform designed to transform academic chaos into a structured Certainty Map through deterministic data structures and probabilistic machine learning. ." Built for students facing high-stakes exams, it replaces overwhelming to-do lists with a **Triage System** that adapts to the user's real-time energy levels and psychological state. Unlike standard planners, Locus implements **Semantic Triage**, **Forgetting Curve Heuristics**, and **Grounded RAG** to provide students with a scientifically optimized study path.


The mission is to solve **Exam Anxiety** by providing verifiable proof of progress and curated autonomy.

---

##Tech Stack
*   **Framework:** Next.js 15 (App Router)
*   **Auth:** Better Auth (Social OAuth + Persistent Sessions)
*   **Database:** PostgreSQL (via Supabase)
*   **ORM:** Prisma
*   **AI Engine:** Gemini 1.5 Flash (Vision & Structured JSON)
*   **Embeddings:** Google `text-embedding-004`
*   **Vector Database:** `pgvector` on Supabase (PostgreSQL)
*   **Data Integrity:** Zod (Schema enforcement for AI outputs)
*   **State Management:** Zustand (Session persistence & Timer state)
*   **Styling/UI:** Tailwind CSS, Shadcn UI, Framer Motion

---

## Full Feature Requirements

| Category | Feature | Technical Implementation |
| :--- | :--- | :--- |
| **Onboarding** | **Photo-to-Plan** | Gemini Vision API parses syllabus photos into structured Prisma tasks. |
| **Dashboard (Prioritisation)** | **The Triage Cards** | Calculates "Heat Scores" by performing a **Cosine Similarity Search** between a user's current "Intent" (Embedding) and the `Requirement` table. |
| **Grounding** | **RAG Micro-Quests** | Generates study sub-tasks by retrieving specific context chunks from uploaded notes to eliminate hallucinations. |
| **Flexibility** | **The "Pivot" Permission** | A button to instantly swap tasks mid-session without "failure" penalty. |
| **AI Partner** | **Micro-Quest Generator** | Gemini breaks big assignments into 15-minute actionable sub-tasks. |
| **Execution** | **Mood-Aware Timer** | Pomodoro durations (15m, 25m, 50m) that adapt to selected Energy Levels. |
| **Validation** | **LLM-as-a-Judge** | A background pipeline where a secondary AI process grades generated tasks on "Atomicity" and "Relevance" to ensure quality. |
| **Memory** | **Confidence Decay Model** | A heuristic feature predicting memory decay based on `SessionDuration` and `Difficulty`, triggering proactive review alerts. |

---

## The "Math" of Triage
Locus moves beyond simple sorting. The dashboard is powered by a **Weighted Multi-Vector Algorithm**:
1.  **Temporal Weight:** Exponentially increases as the deadline approaches.
2.  **Semantic Weight:** Vector distance between user "Intent" and task description.
3.  **Confidence Weight:** Inverse relationship to the user's recorded understanding.

---

## System Architecture & Layout

### 1. The Dashboard Zones (The Home View)
The dashboard is designed as an "Anti-Anxiety" hub divided into three zones:

*   **Zone A: The Compass (Top):** 3 Path Cards representing the top-K vector results. One card glows (High Priority), one is "Momentum" (Sequential), and one is "Quick Win" (Low Energy).
*   **Zone B: The Focus Lab (Middle):** A distraction-free workspace with a circular Pomodoro ring, grounded **Micro-Quests**, and a **Pivot Button** to recalculate the vector space if the user hits a mental block.
*   **Zone C: The Momentum Feed (Sidebar):** A vertical log of `FocusSessions` that feeds the Confidence Decay model and provides "What I did today" proof.

### 2. Page Architecture

| Page | URL | Purpose |
| :--- | :--- | :--- |
| **Triage Home** | `/` | Intent vectorization, energy check-in, and active session control. |
| **The Vault** | `/modules` | Grid of subjects with aggregate "Confidence" and "Heat" visualizations. |
| **The Lab** | `/onboarding` | The RAG setup: syllabus parsing and note embedding pipeline. |
| **History** | `/history` | A visualization of Confidence Delta ($\Delta C$) and Daily Receipts. |

---

## The User Flow (The Daily Journey)

1.  **Phase 1: The Check-In:** App asks: "How's your energy?" (Low/Med/High). UI filters tasks; Low energy triggers 15m "Emergency Sprints."
2.  **Phase 2: The Choice:** User selects one of three cards (Triage-High Heat, Momentum, or Quick Win).
3.  **Phase 3: The Deep Work:** UI enters "Dark Mode." Gemini breaks the task into 5 micro-quests. User follows the checklist as the timer runs.
4.  **Phase 4: The Validation:** Post-session reflection: "How confident do you feel?" (1-5). Dashboard sidebar updates with a "Win."
5.  **Phase 5: The Nightly Peace:** A 9:00 PM summary shows a "Structured List of Wins," proving the user is on track despite any chaos.

---

## Technical Logic & Implementation
*   **Triage Algorithm:** Sorts tasks by $(Weight \times Difficulty) / (DaysToDeadline + 1) + (5 - Confidence)$.
*   **Routing:** Uses **Next.js Parallel Routes** to keep the Focus Timer visible even while browsing other pages.
*   **Persistence:** Zustand + LocalStorage ensures the timer survives page refreshes.
*   **Animations:** Framer Motion used for "Path Cards" to make energy-filtering feel tactile and responsive.

