# FahMai RAG Hackathon — Full Info

**SuperAI Level 1 Hackathon 2026 | Organized by AiAT (10th Anniversary)**

---

## The Story

**FahMai (ฟ้าใหม่)** is a fictional Thai electronics store.
Mission: Build an AI Customer Service Agent that reads a Thai knowledge base and answers customer questions correctly via a RAG system.

Customers ask questions in **casual Thai** about products, warranties, returns, shipping, and membership points.

---

## The FahMai Universe

### 5 House Brands (98 products total)

| Brand          | Category            | Products    |
| -------------- | ------------------- | ----------- |
| **SaiFah**     | Phones & Tablets    | 23 products |
| **DaoNuea**    | Computers & Laptops | 23 products |
| **KluenSiang** | Audio               | 22 products |
| **WongKhoJon** | Wearables           | 12 products |
| **JudChuem**   | Accessories         | 18 products |

**Total including partner brands: ~110 products**

### 4 Partner Brands

| Brand     | Category    |
| --------- | ----------- |
| ZenByte   | Storage     |
| NovaTech  | Peripherals |
| PulseGear | Power       |
| ArcWave   | Displays    |

### 5 Branches

Siam · Lad Phrao · Rama 9 · Chiang Mai · Phuket

---

## Dataset Description

### Knowledge Base (`knowledge_base/`)

**Total: 135 markdown documents** (127 products + 5 policies + 3 store info)

> Note: starter kit ships with **118 files**

All documents are written in **Thai**.

| Folder        | Contents                                       | Approx. Files |
| ------------- | ---------------------------------------------- | ------------- |
| `products/`   | Product spec sheets (specs, pricing, features) | ~127 files    |
| `policies/`   | Returns, warranty, shipping, membership        | ~5 files      |
| `store_info/` | Branch locations, contact info, promotions     | ~3 files      |

---

### Questions File (`questions.csv`)

| Column                   | Description                            |
| ------------------------ | -------------------------------------- |
| `id`                     | Question ID (1–100)                    |
| `question`               | The question in Thai (casual language) |
| `choice_1` … `choice_10` | Answer choices in Thai                 |

**No answers are provided** — you must predict them.

---

### Sample Submission File (`sample_submission.csv`)

| Column   | Description                          |
| -------- | ------------------------------------ |
| `id`     | Question ID (1–100)                  |
| `answer` | Your predicted answer (integer 1–10) |

---

## Answer Choice Structure (All 100 Questions)

Every question has exactly **10 choices**:

| Choice | Meaning                                                                      |
| ------ | ---------------------------------------------------------------------------- |
| 1–8    | Content-specific answers                                                     |
| 9      | `ไม่มีข้อมูลนี้ในฐานข้อมูล` — Information not in knowledge base              |
| 10     | `คำถามนี้ไม่เกี่ยวข้องกับร้านฟ้าใหม่` — Question not related to FahMai store |

### Example Question

> "Watch S3 Ultra กันน้ำได้กี่ ATM ครับ?"
>
> 1. 3 ATM 2) 5 ATM 3) 10 ATM 4) 20 ATM 5) 50 ATM ... 9) ไม่มีข้อมูล 10) ไม่เกี่ยวข้อง

---

## How RAG Works (Pipeline)

```
1. CHUNK     →  Split 135 docs into small passages
2. EMBED     →  Convert chunks to vector representations
3. RETRIEVE  →  Find most relevant chunks for the question
4. GENERATE  →  LLM reads chunks and picks the answer (1–10)
```

The starter kit notebook walks through **Dense**, **BM25**, and **Hybrid (RRF)** retrieval.

---

## Allowed LLM Models (ThaiLLMs ONLY)

All models accessed via: **https://playground.thaillm.or.th/**

| #   | Model Name                                                | Provider |
| --- | --------------------------------------------------------- | -------- |
| 1   | `OpenThaiGPT-ThaiLLM-8B-Instruct-v7.2` (Research Preview) | AIEAT    |
| 2   | `Pathumma-ThaiLLM-qwen3-8b-think-3.0.0`                   | NECTEC   |
| 3   | `Typhoon-S-ThaiLLM-8B-Instruct` (Research Preview)        | SCB 10X  |
| 4   | `THaLLE-0.2-ThaiLLM-8B-fa`                                | KBTG     |

> ⚠️ Submissions **must use one of the above ThaiLLMs**. No other LLMs allowed for generation.

---

## Embedding Models

You may use **ANY embedding model** that fits the task well — no restriction.

- Recommended direction: strong multilingual models (e.g., multilingual-e5, BGE-M3, etc.)
- Pick what gives the best retrieval quality for Thai text

---

## Evaluation

- **Metric:** Accuracy (% of 100 questions answered correctly)
- **Public Leaderboard:** 60% of questions — visible during competition, use for iterating
- **Private Leaderboard:** 40% of questions — revealed at end, used for **final ranking**
- Higher accuracy = better

---

## Competition Rules

- **Individual competition** (รายบุคคล)
- Team name format: `รหัสประจำตัวผู้อบรม-ชื่อจริงภาษาไทย`
  - Example: `600000-นงนุช`
- **Submission limit:** 8 submissions per day per team
- **Day cutoff time:** 07:00 AM Thailand time (ICT, UTC+7)
- ⚠️ **Submissions must be fully automated — no manual answers allowed**

---

## Submission Format

Submit a CSV file with **exactly 100 rows**:

```
id,answer
1,5
2,3
3,7
...
100,2
```

- `id`: integer 1–100
- `answer`: integer 1–10

---

## Important Links

| Resource                     | Link                                                                                  |
| ---------------------------- | ------------------------------------------------------------------------------------- |
| Starter Kit Notebook (Colab) | https://colab.research.google.com/drive/1GMWk8NuUh1SCv3kK7zLEdns9avc9qnb3?usp=sharing |
| Kaggle Competition           | https://www.kaggle.com/t/e5771bca3ab441e4a1e101839b9098ec                             |
| ThaiLLM API Playground       | https://playground.thaillm.or.th/                                                     |

---

## Deadline

**Sunday March 29, 2026 @ 8:00 AM (Thailand time)**

---

## Ideas to Explore (from official slides)

| Idea                   | Description                                                          |
| ---------------------- | -------------------------------------------------------------------- |
| **Better Embeddings**  | Try stronger multilingual embedding models                           |
| **Chunk Size Tuning**  | Split by structure, adjust chunk size, enrich chunks with metadata   |
| **Prompt Engineering** | Adjust system prompt, add few-shot examples, change output format    |
| **Try Other ThaiLLMs** | Benchmark across all 4 allowed models                                |
| **Hybrid Retrieval**   | Dense + BM25 via Reciprocal Rank Fusion (RRF) — keywords + semantics |
| **Reranking**          | Cross-encoder or LLM-based reranking for better top-K selection      |

---

## File Structure

```
data/
├── hack_info.md            ← this file
├── knowledge_base/
│   ├── products/           # ~127 Thai markdown files — product specs, pricing, features
│   ├── policies/           # ~5 Thai markdown files — returns, warranty, shipping, membership
│   └── store_info/         # ~3 Thai markdown files — branch locations, contact, promotions
├── questions.csv           # 100 questions x 10 choices (Thai), no answers
└── sample_submission.csv   # Template: id (1-100), answer (1-10)
```

---

## Fun Fact

> Season 5 originally had no RAG hackathon — **this one was added especially!** 🎉

---

_Last updated: 2026-03-28_
_By PoomPhasinDev_
