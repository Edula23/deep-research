# Deep Research

A multi-agent research assistant that takes a topic, plans a set of searches, gathers information from the web, and writes up a structured report — streamed live to a simple chat-style UI.

## How it works

Deep Research is built around a `ResearchManager` that orchestrates a small pipeline of specialized agents:

1. **Planner Agent** (`planner_agent.py`) — breaks the user's query down into a set of targeted search queries.
2. **Search Agent** (`search_agent.py`) — executes those searches and pulls back relevant results.
3. **Writer Agent** (`writer_agent.py`) — synthesizes the search results into a coherent, structured report.
4. **Email Agent** (`email_agent.py`) — optionally formats and sends the final report by email.

The `ResearchManager` (`research_manager.py`) ties these together and yields incremental output, so the UI can stream progress and the final report as it's generated.

## UI

The app uses [Gradio](https://www.gradio.app/) for the interface (`app.py`):

- A text box for the research topic/query
- A "Run" button (and Enter-to-submit) to kick off the pipeline
- A live-updating Markdown panel showing the streamed report

## Tech stack

- **Python**
- **Gradio** — UI
- **python-dotenv** — environment variable management
- **uv** — dependency management (see `pyproject.toml` / `uv.lock`)
- **Railway** — deployment (see `railway.json`)

## Project structure

```
.
├── app.py                # Gradio UI entrypoint
├── research_manager.py   # Orchestrates the agent pipeline
├── planner_agent.py      # Breaks query into search tasks
├── search_agent.py       # Performs web searches
├── writer_agent.py       # Synthesizes final report
├── email_agent.py        # Sends report via email (optional)
├── pyproject.toml        # Project dependencies
├── uv.lock                # Locked dependency versions
├── railway.json          # Railway deployment config
├── requirements.txt
└── .env.example           # Example environment variables
```

## Setup

1. **Clone the repo**
   ```bash
   git clone <your-repo-url>
   cd deep-research
   ```

2. **Install dependencies with `uv`**
   ```bash
   uv sync
   ```

3. **Configure environment variables**
   ```bash
   cp .env.example .env
   ```
   Fill in the required API keys/secrets in `.env` (e.g. LLM provider keys, email provider credentials if using the email agent).

4. **Run locally**
   ```bash
   uv run app.py
   ```
   The app launches a Gradio interface (in-browser) on `PORT` (default `7860`).

## Usage

1. Open the app.
2. Enter a research topic in the text box.
3. Click **Run** (or press Enter).
4. Watch the report stream in as the planner, search, and writer agents do their work.

## Deployment

This project is set up for deployment on [Railway](https://railway.app/) via `railway.json`. Push to your connected repo/branch and Railway will build and run the app using the `PORT` environment variable it provides.

## Roadmap / Ideas

- [ ] Add source citations to the generated report
- [ ] Support exporting the report as PDF/Markdown
- [ ] Add configurable depth (number of searches) for the planner
- [ ] Add tests for each agent

## License

MIT
