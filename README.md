# AI Blog Generator (CrewAI + LangGraph)

Generates an SEO-oriented blog post for a small-business AI consultancy from the code and outputs of a Jupyter notebook, using a team of LLM agents.

## What it does
It extracts code and outputs from a notebook (`data/demo_test.ipynb` by default), then runs a three-agent CrewAI workflow that analyses it, writes the post and optimises it for search. The result is saved as a Markdown file (`akshay_small_business_ai_blog.md` is a sample output).

## Features
- Three CrewAI agents: AI Solutions Research Specialist, Small Business AI Content Strategist, Small Business AI SEO Specialist (`src/agents/agent_definitions.py`), with three sequential tasks (`src/tasks/task_definitions.py`).
- Gemini 2.0 Flash (`gemini/gemini-2.0-flash`, temperature 0.5) via CrewAI `LLM`; web search with `SerperDevTool`.
- Notebook extractor utility (`src/utils/notebook_extractor.py`).
- Configurable business details and target keywords (`src/config/business_details.py`).
- Alternative implementation in `demo_using_langgraph/`: a LangGraph state machine (parse notebook, clean code, draft blog, feedback, accept/reject, final blog) using LangChain with Gemini or GitHub-hosted GPT-4o models.

## Flow
```mermaid
flowchart LR
  NB[Notebook] --> EX[extract code + output]
  EX --> A1[Research agent] --> A2[Content agent] --> A3[SEO agent] --> MD[Markdown blog]
```

## Tech stack
Python, CrewAI, crewai-tools (Serper), Gemini, LangChain / LangGraph, python-dotenv, Jupyter.

## Structure
```
blog_genaroter.py          # entry point -> src.main.generate_blog()
src/{agents,tasks,config,utils,main.py}
demo_using_langgraph/      # LangGraph variant + notebook
old_blog_gen.py            # earlier single-file version
data/raw/demo_test.ipynb   # (note: default path is data/demo_test.ipynb)
config/ notebooks/ tests/  # mostly placeholders
```

## Setup / run
```bash
pip install crewai crewai-tools python-dotenv   # requirement.txt only lists python-dotenv
# .env
SERPER_API_KEY=...
GEMINI_API_KEY=...
# LangGraph variant also uses GITHUB_TOKEN
python blog_genaroter.py
```
Note: the default `NOTEBOOK_PATH` is `data/demo_test.ipynb`, while the sample notebook lives at `data/raw/demo_test.ipynb`; adjust one of them.

## Limitations
Dependency file is incomplete; README references `blog_gen.py` and `requirements.txt`, which do not exist; tests are placeholders; business details are hard-coded to the author's consultancy.
