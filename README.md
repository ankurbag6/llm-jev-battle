# llm-jev-battle

Tests [Jev](https://docs.typesafe.ai/introduction), TypeSafe's "System One" model, then compares it with OpenAI and Claude models on a classification task.

Jev doesn't generate text. You send it some content (the **state**) and typed questions, and it returns structured answers with probabilities:

- **noul**: yes/no, with a probability from 0 to 1
- **choice**: picks one option from a list
- **score**: rates the content on a rubric

## Notebooks

| Notebook | What it covers |
|---|---|
| [part1_raw_http.ipynb](part1_raw_http.ipynb) | Calls the Jev API directly with `requests`, so you can see the raw request and response |
| [part2_sdk.ipynb](part2_sdk.ipynb) | Makes the same calls with the official `typesafe-sdk` Python SDK |
| [part3_battle.ipynb](part3_battle.ipynb) | Runs Jev, GPT and Claude models on 16 labeled support messages and compares accuracy, cost and speed |

Each notebook runs on its own, so you don't need to run them in order.

## Setup

1. Create a virtual environment and install the libraries:

   ```bash
   uv venv --python 3.12 .venv
   uv pip install -r requirements.txt
   ```

2. Create a `.env` file in the repo folder with your API keys:

   ```
   TYPESAFE_API_KEY=...
   OPENAI_API_KEY=...      # Part 3 only
   ANTHROPIC_API_KEY=...   # Part 3 only
   ```

3. Open a notebook in VS Code or Jupyter and select the `.venv` kernel.

## The battle (Part 3)

Each model labels every message on 4 fields:

| Label | Options |
|---|---|
| `is_billing` | yes / no |
| `tone` | angry / neutral / happy |
| `team` | engineering / billing / sales / account |
| `urgency` | low / medium / high |

Contestants: `jev-latest`, `gpt-5-nano`, `gpt-4.1-mini`, `claude-haiku-5-5`, `claude-sonnet-5-5` and `claude-opus-5-5`. The LLMs use structured outputs, so formatting mistakes don't cost them points.

The notebook reports:

- accuracy overall and for each label
- cost per 1,000 messages, using the token counts each API returns
- average seconds per message
- which messages each model got wrong

Prices are hard-coded in the `PRICES` table in the notebook. Check them against each provider's pricing page before trusting the cost numbers.

## License

MIT, see [LICENSE](LICENSE).
