# LangChain Social Post Generator

A lightweight social media post generator built with **LangChain**, **Google Gemini**, and **Pydantic**.

The project takes a short product description and generates:

* A punchy title
* An engaging caption (max 280 characters)
* 3–5 hashtags
* Short alt-text
* An Arabic translation of the caption

The final results are saved to `posts.csv`.

## Tech Stack

* Python 3.10+
* Google Colab
* LangChain
* Google Gemini API
* Pydantic
* Pandas

**Model:** `gemini-3.5-flash-lite`

## Installation

Run in Google Colab:

```bash
pip install -U langchain langchain-google-genai pydantic
```

The Gemini API key is stored securely in **Google Colab Secrets** as:

```text
GEMINI_API_KEY
```

It is loaded using:

```python
from google.colab import userdata

api_key = userdata.get("GEMINI_API_KEY")
```

The API key is never hard-coded in the notebook.

## How It Works

```text
Product Description
        ↓
ChatPromptTemplate
        ↓
Gemini
        ↓
Structured SocialPost
        ↓
Arabic Translation
        ↓
posts.csv
```

The project uses a Pydantic schema:

```python
class SocialPost(BaseModel):
    title: str
    caption: str
    hashtags: list[str]
    alt_text: str
```

Structured output is enforced using:

```python
llm.with_structured_output(SocialPost)
```

The complete workflow is wrapped in:

```python
generate_post(description)
```

which returns the generated post plus `arabic_caption`.

## Test Descriptions

The notebook was tested with these three product descriptions:

1. `new iced karak chai with cardamom, served cold in a tall glass`

2. `freshly baked chocolate croissant with a crispy outside and soft chocolate-filled center`

3. `creamy mango smoothie made with fresh mangoes, yogurt, and a touch of honey`

## Output

`posts.csv` contains:

| Column           | Description                  |
| ---------------- | ---------------------------- |
| `description`    | Original product description |
| `title`          | Generated title              |
| `caption`        | English caption              |
| `hashtags`       | 3–5 generated hashtags       |
| `alt_text`       | Accessibility description    |
| `arabic_caption` | Arabic caption               |

## Project Structure

```text
langchain-social-post/
├── README.md
├── langchain_social_post.ipynb
├── posts.csv
└── screenshots/
    ├── 01_notebook_run.png
    ├── 02_structured_output.png
    └── 03_csv_open.png
```

## Requirements

* Google Colab
* Python 3.10+
* Gemini API key
* No GPU required
* No paid services required

## Scope

This project focuses on:

* Prompt Templates
* Structured Outputs
* Pydantic
* Sequential LangChain chains
* Arabic translation
* CSV export

No agents, RAG, vector databases, or deployment are used.
