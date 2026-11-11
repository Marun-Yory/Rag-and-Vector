## Rag-and-Vector

- Python (3.14.6 or later)
- An [OpenAI account](https://openai.com/) (or an OpenAI-compatible
  provider like Groq, Gemini, or Ollama)
- Basic familiarity with Python and the command line

## Adding dependencies
Now add the dependencies we'll need:

```bash
uv add sqlitesearch minsearch openai jupyter python-dotenv
```

This installs:

- `sqlitesearch` - to search documents in a SQLite database
- `minsearch` - a simple in-memory search engine for indexing and
  searching text
- `openai` - the OpenAI API client for calling the LLM
- `jupyter` - the notebook environment where we'll write and run code
- `python-dotenv` - to load API keys from a `.env` file

## Setting up API keys

The safest way to store the key is in a `.env` file that never gets
committed to git.

Create a `.env` file in your project folder and put your API key in
it:

```bash
OPENAI_API_KEY=sk-YOUR_KEY_HERE
```

Now add `.env` to `.gitignore` to make sure you never accidentally
commit your key:

```bash
.env
```

Never commit `.env` to git. Treat the API key like a password. If it
leaks, someone else can run up charges on your account.

## Starting Jupyter

Start Jupyter:

```bash
uv run jupyter notebook
```

Create a new notebook. Throughout the course, you'll copy code from
the section notes into notebook cells.

Check that the OpenAI client works:

```python
from dotenv import load_dotenv
load_dotenv()

from openai import OpenAI
openai_client = OpenAI()
```

If you see an error, make sure the key in your `.env` file is
correct.

For Groq or other OpenAI-compatible providers, add the key to
`.env`:

```bash
GROQ_API_KEY=your_key_here
```

And configure the client:

```python
from openai import OpenAI
import os

openai_client = OpenAI(
    api_key=os.getenv("GROQ_API_KEY"),
    base_url="https://api.groq.com/openai/v1"
)
```

## (Optional) Auto-loading .env with dirdotenv

If you don't want to call `load_dotenv()` in every notebook, use
[dirdotenv].

It loads `.env` files automatically when you `cd` into a directory:

```bash
uv tool install dirdotenv
echo 'eval "$(dirdotenv hook bash)"' >> ~/.bashrc
```

Restart your terminal, and now whenever you enter the project
directory, the variables from `.env` are loaded automatically. No
`load_dotenv()` needed.
# Vector Search

We extend the RAG pipeline vector search. Vector search
matches documents by semantic meaning instead of exact keyword
overlap. We start from embeddings and end with persistent vector
indexes (sqlitesearch, PGVector) and ONNX-based embedders for
lightweight deployments.
