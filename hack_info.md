# FahMai (ฟ้าใหม่) Electronics Store — RAG Hackathon Info

## Overview

**FahMai (ฟ้าใหม่)** is a fictional Thai electronics store.
The task is to build a **Retrieval-Augmented Generation (RAG)** system that answers **100 multiple-choice questions** about the store using a provided Thai-language knowledge base.

---

## Dataset Description

### Knowledge Base (`knowledge_base/`)

Markdown files organized into **three folders**. All documents are written in **Thai**.

| Folder | Contents | Approx. Files |
|---|---|---|
| `products/` | Product spec sheets (specs, pricing, features) | ~30 files |
| `policies/` | Store policies (returns, warranty, shipping, membership) | ~5 files |
| `store_info/` | Branch locations, contact info, promotions | ~5 files |

---

### Questions File (`questions.csv`)

| Column | Description |
|---|---|
| `id` | Question ID (1–100) |
| `question` | The question in Thai |
| `choice_1` … `choice_10` | Answer choices in Thai |

---

### Sample Submission File (`sample_submission.csv`)

| Column | Description |
|---|---|
| `id` | Question ID (1–100) |
| `answer` | Your predicted answer (integer 1–10) |

---

## Answer Choice Structure (All 100 Questions)

Every question has exactly **10 choices**:

| Choice Number | Meaning |
|---|---|
| 1–8 | Content-specific answers |
| 9 | `ไม่มีข้อมูลนี้ในฐานข้อมูล` — No data available in the knowledge base |
| 10 | `คำถามนี้ไม่เกี่ยวข้องกับร้านฟ้าใหม่` — This question is not related to FahMai store |

> **Important:** Choice 9 and 10 are special sentinel answers. Use them only when the knowledge base truly lacks the info, or the question is entirely off-topic.

---

## Allowed Models (ThaiLLMs Only)

You **MUST** use only the following Thai LLMs:

| Model Name |
|---|
| `OpenThaiGPT-ThaiLLM-8B-instruct-v7.2` |
| `Pathumma-ThaiLLM-qwen3-8b-think-3.0.0` |
| `Typhoon-S-ThaiLLM-8B-Instruct` |
| `THaLLE-0.2-ThaiLLM-8b-fa` |

**API Access:** https://playground.thaillm.or.th/chat/

---

## Evaluation Metric

- **Metric:** Accuracy (percentage of correct answers out of 100)
- **Public Leaderboard:** Scored on **60%** of questions — visible during the competition, use for model development
- **Private Leaderboard:** Scored on **40%** of questions — revealed at end of competition, used for final ranking

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

## Competition Rules

- **Individual competition** (รายบุคคล)
- Team name format: `รหัสประจำตัวผู้อบรม-ชื่อจริงภาษาไทย`
  - Example: `600000-นงนุช`
- **Submission limit:** 8 submissions per day per team
- **Day cutoff time:** 07:00 AM Thailand time (ICT, UTC+7)

---

## File Structure Summary

```
data/
├── knowledge_base/
│   ├── products/       # ~30 Thai markdown files — product specs, pricing, features
│   ├── policies/       # ~5 Thai markdown files — returns, warranty, shipping, membership
│   └── store_info/     # ~5 Thai markdown files — branch locations, contact, promotions
├── questions.csv       # 100 questions with 10 choices each (Thai)
└── sample_submission.csv  # Template: id (1-100), answer (1-10)
```

---

## RAG System Design Tips

1. **Retrieve** relevant markdown documents from `knowledge_base/` based on the question.
2. **Augment** the prompt with retrieved context.
3. **Generate** the answer using one of the allowed ThaiLLMs.
4. **Post-process:** extract integer 1–10 from the model output.
5. Handle edge cases:
   - If no relevant document found → lean toward **choice 9**
   - If question is clearly off-topic → lean toward **choice 10**

---

*Last updated: 2026-03-28*
