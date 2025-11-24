# ✨ DailyJobMatch  
![Made with n8n](https://img.shields.io/badge/Made%20with-n8n-00e8a2?style=flat&logo=n8n)  
![OpenAI Powered](https://img.shields.io/badge/Powered%20by-OpenAI-412991?style=flat&logo=openai)  
![Docker](https://img.shields.io/badge/Run%20with-Docker-2496ED?style=flat&logo=docker)  
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat)

> **Automated AI-powered job-matching workflow built with n8n**

---

## 📑 Table of Contents
1. [Overview](#-overview)  
2. [Architecture](#-architecture-diagram)  
3. [How to Use](#-how-to-use-dailyjobmatch)  
   - [Install n8n](#1️⃣-installing--running-n8n-with-docker)  
   - [Set Up Credentials](#2️⃣-setting-up-required-credentials)  
   - [Import Workflow](#3️⃣-importing--configuring-the-workflow)  
4. [Output Format](#-output-format)  
5. [Example Email](#-example-email-output)  
6. [Contact](#-contact)  
7. [Roadmap](#-roadmap)

---

## 📚 Overview

### 🧠 What DailyJobMatch Does  
DailyJobMatch automates your job search every morning by collecting fresh job postings, comparing each role against your CV using a large language model, and emailing you a ranked shortlist of the most relevant opportunities.

### ⭐ Core Features
- 🔄 **Daily automation** — Runs automatically (e.g., 07:30).  
- 📝 **CV-aware matching** — Uses full CV content, not keywords.  
- 💯 **Multi-dimensional scoring** — Background, skills, experience, seniority, language, company.  
- 🧹 **Cleaning & deduplication** — Normalises fields and removes mismatches (student, intern, postdoc).  
- 📊 **Structured LLM output** — Enforces strict JSON.  
- 🥇 **Top-N selection** — Keeps only the most relevant jobs.  
- 💌 **Dark-theme email report** — Job cards, scores, keywords, and “View & Apply” buttons.

---

## 🏗️ Architecture Diagram

<p align="center">
  <img width="2268" src="https://github.com/user-attachments/assets/7ceafeef-1611-4131-9f8b-03a719967199" />
  <br/>
  <img width="2565" src="https://github.com/user-attachments/assets/5b406f69-ec06-4b07-aae2e604abda" />
</p>

---

## 📘 How to Use DailyJobMatch

> This is an early-stage project and still requires personalised setup.

---

### 1️⃣ Installing & Running n8n with Docker  
DailyJobMatch is built on **n8n**, a flexible automation tool.  
Installation guide:  
https://github.com/n8n-io/n8n

---

### 2️⃣ Setting Up Required Credentials  

DailyJobMatch relies on several external services:

#### 🔹 Google Drive & Gmail  
Used to fetch your CV and send daily job digest emails.  
Docs:  
https://docs.n8n.io/integrations/builtin/credentials/google/

#### 🔹 Apify (LinkedIn Job Scraper)  
Collects fresh LinkedIn job postings from the last 24h.  
I use:  
**Linkedin Jobs Scraper – PPR**  
https://apify.com/curious_coder/linkedin-jobs-scraper

#### 🔹 OpenAI (LLM scoring model)  
Reads job descriptions + your CV and outputs structured scores.  
Docs:  
https://docs.n8n.io/integrations/builtin/credentials/openai/

---

### 3️⃣ Importing & Configuring the Workflow  
After setting up Docker and credentials, import:

```
workflow/Job_Hunting_Agent.json
```

> ⚠️ Replace all my example credentials with your own.

Customise these nodes:

- **Config** — Add Apify API key, recipient email, number of jobs per day.  
- **RetrieveCV (Google Drive)** — Set your Google Drive OAuth2 + CV PDF file.  
- **LinkedIn** — Use your Apify endpoint + token.  
- **Filter & Deduplicate** — Update banned keywords (e.g., “student”, “temporary”).  
- **Score Job & Extract** — Customise the LLM prompt + scoring logic.  
- **Model Nodes** — Choose your LLM model/credentials.  
- **Send a Message (Gmail)** — Select Gmail OAuth2 + set the **To** field.

#### ✔️ Manual Test  
Disable schedule → click **Execute Workflow** → review step-by-step execution.

---

## 📤 Output Format

The scoring node requires the LLM to:

- Read your entire CV  
- Read the job description  
- Evaluate fit across six dimensions  
- Output strict JSON (no markdown or commentary)  
- Keep the structure consistent across all jobs  

### JSON Template
```json
{
  "score": {
    "overall": 0,
    "background_match": 0,
    "skills_overlap": 0,
    "experience_relevance": 0,
    "seniority": 0,
    "language_requirement": 0,
    "company_score": 0
  },
  "summary": "",
  "keywords": [],
  "fit_bullets": [],
  "connector": ""
}
```

### Scoring Rubric

| Category                  | Range | Description                                                        |
|---------------------------|-------|--------------------------------------------------------------------|
| **background_match**      | 0–10  | Fit with domain (bioinformatics, pharma, biotech)                  |
| **skills_overlap**        | 0–30  | Match between required skills and your technical stack              |
| **experience_relevance**  | 0–30  | Alignment with responsibilities & past projects                     |
| **seniority**             | 0–10  | Entry-level preference; penalises senior roles                      |
| **language_requirement**  | 0–10  | Penalises strict “Danish required” listings                         |
| **company_score**         | 0–10  | Bonus for biotech/pharma/AI companies                               |
| **overall**               | 0–100 | Sum of all categories                                               |

---

## 📧 Example Email Output

<p align="center">
  <img src="https://github.com/user-attachments/assets/30da81d5-ba15-43fc-8345-aebe2c4ac42c" width="500">
</p>

---

## 📬 Contact

- **Author:** Chunxu Han  
- **Email:** s220311@dtu.dk  
- **LinkedIn:** https://www.linkedin.com/in/chunxu-han  

Feel free to reach out with questions or feature ideas!

---

## 🌟 Roadmap

- JobIndex / StepStone / Indeed integrations  
- Notion / Supabase job-tracking storage  
- Auto-apply system (draft cover letters)  
- Multi-country job search  
- Weekly analytics dashboard (Grafana / Supabase)  
- AI-based CV improvement suggestions  

_Contributions welcome — open an issue or submit a PR!_

---
