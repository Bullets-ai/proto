# gptprompt_final

**JD (Job Description) Based Resume ATS Optimization Pipeline**

A Python script that extracts keywords from job postings using GPT and embeddings, matches and rewrites resume bullets, and improves ATS (Applicant Tracking System) scores.

---

## Overview

This script performs the following tasks:

1. **JD Collection** — Collect job descriptions from text files or LinkedIn
2. **Keyword Extraction** — Extract category-based keywords from JD using GPT (technologies, hard/soft skills, knowledge, industry, education, experience)
3. **Resume Matching** — Match resume bullets with keywords using Fast Score, Jaccard similarity, and embedding-based methods
4. **Keyword Labeling** — Determine keyword usage per bullet (used / not used) via rule-based logic and GPT
5. **Bullet Rewriting** — Rewrite bullets by naturally incorporating unused keywords
6. **Top-K Selection by Company** — Select priority bullets based on company and experience
7. **Summary Generation** — Generate a Professional Summary reflecting JD-matched bullets
8. **Final Output** — Produce optimized resume in JSON/HTML format

---

## Features Summary

| Feature | Description |
|---------|-------------|
| JD Keyword Extraction | technologies and tools, hard skills, soft skills, knowledge, industry, education, experience |
| Resume Matching | Fast Score, Jaccard similarity, embedding cosine similarity |
| Keyword Labeling | Rule-based (tools, industry) + Embedding prefilter + GPT (hard/soft/knowledge) |
| Bullet Rewriting | GPT-based keyword insertion, Action Verb + Method + Result structure |
| Semantic Rewriting | Integrate 'not used' tech keywords into the closest bullet via embedding similarity |
| Cost Tracking | Token and cost logging (GPT-3.5-turbo, GPT-4-turbo, text-embedding-3-small) |

---

## Requirements

- **Python 3.10+**
- **spaCy** (`en_core_web_sm` model)
- **OpenAI API Key**

### Main Dependencies

```
openai
pandas
numpy
spacy
playwright
beautifulsoup4
jinja2
tiktoken
more-itertools
python-dateutil
python-dotenv
ftfy
```

---

## Installation

```bash
# Create and activate virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install spaCy English model
python -m spacy download en_core_web_sm

# Install dependencies
pip install -r requirements.txt
```

---

## Environment Variables

| Variable | Description |
|----------|-------------|
| `OPENAI_API_KEY` | OpenAI API key (required) |

Example `.env` file:

```
OPENAI_API_KEY=sk-...
```

---

## Usage

### Run

```bash
python gptprompt_final.py
```

### Runtime Input

1. **Select JD Input Method**
   - `1`: Manual input (text file `data/jd_text.txt`)
   - `2`: Automatic LinkedIn JD collection (Chrome CDP connection)

2. **For manual input**: Enter position title, company name, location, and job posting URL

### Input Files

The following files must be prepared before running the script:

| File | Description |
|------|-------------|
| `profile.json` | Resume profile (contact info, experience, education, certifications, skills, etc.) |
| `Resume_Table.csv` | Bullet-level data (Bullet, Tool, Skill, Goal, Impact, Organization, Start, End, etc.) |
| `keyword_alias_map.csv` | Keyword-alias mapping (keyword, alias columns) |
| `data/jd_text.txt` | JD text (for manual input mode) |

### Output Files

| File | Description |
|------|-------------|
| `bullet_keyword_gpt_results.csv` | Keyword labeling results |
| `Bullet_table.csv` | Bullet table (includes final_bullet) |
| `bullet_summary.csv` | Professional Summary |
| `selected_top_bullets_by_company.csv` | Top-K bullets per company |
| `final_resume_data.json` | Final resume JSON |
| `resume_output.html` | HTML rendering output |
| `gpt_usage_log.csv` | GPT usage and cost log |

---

## Project Structure (Reference)

Recommended structure when uploading to GitHub:

```
gptprompt_final/
├── gptprompt_final.py      # Main script
├── README.md
├── README_EN.md
├── requirements.txt
├── utils/
│   └── score_utils.py      # compute_resume_score, compute_total_experience_years
├── data/
│   ├── jd_text.txt
│   ├── profile.json
│   ├── Resume_Table.csv
│   └── keyword_alias_map.csv
└── templates/
    ├── resume_template.html
    └── final_resume_data.json  # output
```

> **Note**: `utils/score_utils` provides `compute_resume_score` and `compute_total_experience_years`. Include it with this script or merge these functions into `gptprompt_final.py`.

---

## Path Configuration

The following paths are hardcoded in the script. Update them for your environment:

- `load_dotenv()` — `.env` file path
- `alias_map_path` — `keyword_alias_map.csv` path
- `PROFILE_PATH`, `RESUME_CSV_PATH` — Profile and resume CSV paths
- `BULLET_TABLE_OUT`, `SUMMARY_PATH`, `CSV_OUT` — Output file paths

---

## Cost

- **Approximate run cost**: ~$0.05 USD depending on JD/resume size, ~80 seconds runtime (based on code comments)
- Models used: GPT-3.5-turbo (primary), GPT-4-turbo (summary, rare fallback), text-embedding-3-small (embeddings)

---

## License

MIT (or set according to your project)
