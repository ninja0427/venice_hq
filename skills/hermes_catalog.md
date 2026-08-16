# Hermes スキル カタログ

Hermes 同梱の手順書（SKILL.md）を、本体を起動せずに参照するための目次。
本文はこのリポジトリに複製していない。下表の絶対パスを READ_FILE で読む。

- 全 79 件 / venice_cli でそのまま使えるもの ○ = 56 件
- `—` は Hermes 固有ツール（ブラウザ操作・MCP・AppleScript 等）が前提。
  手順の考え方だけ参考にしてよいが、書かれたツールは venice_cli には存在しない。

## 使い方

1. 詰まった作業に近い行を下表から選ぶ
2. `[READ_FILE path="<パス>"]` で本文を読む
3. 本文の手順に従う。ツール名が venice_cli に無い場合は、
   `run_command_alternatives.md` の対応表で読み替える

## 一覧

| 可 | 分野 | 名前 | 要約 | パス |
|---|---|---|---|---|
| ○ | apple | apple-notes | Manage Apple Notes via memo CLI: create, search, edit. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\apple\apple-notes\SKILL.md |
| ○ | apple | apple-reminders | Apple Reminders via remindctl: add, list, complete. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\apple\apple-reminders\SKILL.md |
| ○ | autonomous-ai-agents | codex | Delegate coding to OpenAI Codex CLI (features, PRs). | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\autonomous-ai-agents\codex\SKILL.md |
| ○ | autonomous-ai-agents | merge-reconciler | Neutral third-party resolution of agent merge conflicts. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\autonomous-ai-agents\merge-reconciler\SKILL.md |
| ○ | autonomous-ai-agents | opencode | Delegate coding to OpenCode CLI (features, PR review). | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\autonomous-ai-agents\opencode\SKILL.md |
| ○ | creative | architecture-diagram | Dark-themed SVG architecture/cloud/infra diagrams as HTML. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\architecture-diagram\SKILL.md |
| ○ | creative | ascii-art | ASCII art: pyfiglet, cowsay, boxes, image-to-ascii. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\ascii-art\SKILL.md |
| ○ | creative | ascii-video | ASCII video: convert video/audio to colored ASCII MP4/GIF. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\ascii-video\SKILL.md |
| ○ | creative | baoyu-infographic | Infographics: 21 layouts x 21 styles (信息图, 可视化). | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\baoyu-infographic\SKILL.md |
| ○ | creative | claude-design | Design one-off HTML artifacts (landing, deck, prototype). | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\claude-design\SKILL.md |
| ○ | creative | comfyui | Generate images, video, and audio via diffusion workflows. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\comfyui\SKILL.md |
| ○ | creative | design-md | Author/validate/export Google's DESIGN.md token spec files. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\design-md\SKILL.md |
| ○ | creative | excalidraw | Hand-drawn Excalidraw JSON diagrams (arch, flow, seq). | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\excalidraw\SKILL.md |
| ○ | creative | humanizer | Humanize text: strip AI-isms and add real voice. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\humanizer\SKILL.md |
| ○ | creative | manim-video | Manim CE animations: 3Blue1Brown math/algo videos. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\manim-video\SKILL.md |
| ○ | creative | p5js | p5.js sketches: gen art, shaders, interactive, 3D. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\p5js\SKILL.md |
| ○ | creative | pretext | Build creative browser demos with DOM-free text layout. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\pretext\SKILL.md |
| ○ | creative | songwriting-and-ai-music | Songwriting craft and Suno AI music prompts. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\songwriting-and-ai-music\SKILL.md |
| ○ | creative | touchdesigner-mcp | Control TouchDesigner via twozero MCP. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\touchdesigner-mcp\SKILL.md |
| ○ | email | email-inbox-triage | Triage an inbox: prioritize threads, draft replies safely. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\email\email-inbox-triage\SKILL.md |
| ○ | email | himalaya | Himalaya CLI: IMAP/SMTP email from terminal. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\email\himalaya\SKILL.md |
| ○ | github | codebase-inspection | Inspect codebases w/ pygount: LOC, languages, ratios. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\github\codebase-inspection\SKILL.md |
| ○ | github | github-auth | GitHub auth setup: HTTPS tokens, SSH keys, gh CLI login. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\github\github-auth\SKILL.md |
| ○ | github | github-code-review | Review PRs: diffs, inline comments via gh or REST. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\github\github-code-review\SKILL.md |
| ○ | github | github-issue-to-pr | Carry a GitHub issue to a verified PR with honest CI state. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\github\github-issue-to-pr\SKILL.md |
| ○ | github | github-issues | Create, triage, label, assign GitHub issues via gh or REST. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\github\github-issues\SKILL.md |
| ○ | github | github-pr-workflow | GitHub PR lifecycle: branch, commit, open, CI, merge. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\github\github-pr-workflow\SKILL.md |
| ○ | github | github-repo-management | Clone/create/fork repos; manage remotes, releases. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\github\github-repo-management\SKILL.md |
| ○ | media | gif-search | Search/download GIFs from Tenor via curl + jq. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\media\gif-search\SKILL.md |
| ○ | media | songsee | Audio spectrograms/features (mel, chroma, MFCC) via CLI. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\media\songsee\SKILL.md |
| ○ | media | youtube-content | YouTube transcripts to summaries, threads, blogs. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\media\youtube-content\SKILL.md |
| ○ | mlops | evaluating-llms-harness | lm-eval-harness: benchmark LLMs (MMLU, GSM8K, etc.). | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\mlops\evaluation\evaluating-llms-harness\SKILL.md |
| ○ | mlops | huggingface-hub | HuggingFace hf CLI: search/download/upload models, datasets. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\mlops\huggingface-hub\SKILL.md |
| ○ | mlops | llama-cpp | llama.cpp local GGUF inference + HF Hub model discovery. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\mlops\inference\llama-cpp\SKILL.md |
| ○ | mlops | serving-llms-vllm | vLLM: high-throughput LLM serving, OpenAI API, quantization. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\mlops\inference\serving-llms-vllm\SKILL.md |
| ○ | mlops | weights-and-biases | W&B: log ML experiments, sweeps, model registry, dashboards. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\mlops\evaluation\weights-and-biases\SKILL.md |
| ○ | note-taking | obsidian | Read, search, create, and edit notes in the Obsidian vault. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\note-taking\obsidian\SKILL.md |
| ○ | productivity | document-to-action-items | Extract cited obligations, deadlines, tasks from documents. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\document-to-action-items\SKILL.md |
| ○ | productivity | docx | Create, read, edit, template, and review Word .docx files. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\docx\SKILL.md |
| ○ | productivity | google-workspace | Gmail, Calendar, Drive, Docs, Sheets via gws CLI or Python. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\google-workspace\SKILL.md |
| ○ | productivity | meeting-action-items | Turn meeting notes into cited decisions, owners, tickets. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\meeting-action-items\SKILL.md |
| ○ | productivity | nano-pdf | Edit text in existing PDFs via natural-language prompts. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\nano-pdf\SKILL.md |
| ○ | productivity | notion | Notion API + ntn CLI: pages, databases, markdown, Workers. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\notion\SKILL.md |
| ○ | productivity | ocr-and-documents | Extract text from PDFs/scans (pymupdf, marker-pdf). | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\ocr-and-documents\SKILL.md |
| ○ | productivity | pdf | Create, read, merge, fill, and secure PDF files. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\pdf\SKILL.md |
| ○ | productivity | powerpoint | Create, read, edit .pptx decks with python-pptx. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\powerpoint\SKILL.md |
| ○ | productivity | teams-meeting-pipeline | Teams meeting summaries, job replay, Graph subscriptions. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\teams-meeting-pipeline\SKILL.md |
| ○ | productivity | weekly-review-planning | Weekly reset: commitments, stalled work, next-week plan. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\weekly-review-planning\SKILL.md |
| ○ | productivity | xlsx | Create, read, edit Excel .xlsx workbooks and CSVs. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\xlsx\SKILL.md |
| ○ | research | arxiv | Search arXiv papers by keyword, author, category, or ID. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\research\arxiv\SKILL.md |
| ○ | research | blogwatcher | Monitor blogs and RSS/Atom feeds via blogwatcher-cli tool. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\research\blogwatcher\SKILL.md |
| ○ | research | competitor-news-monitor | Watch named companies for material news; cited digests. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\research\competitor-news-monitor\SKILL.md |
| ○ | research | llm-wiki | Karpathy's LLM Wiki: build/query interlinked markdown KB. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\research\llm-wiki\SKILL.md |
| ○ | social-media | xurl | X/Twitter via xurl CLI: raw post search, posting, DM, media. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\social-media\xurl\SKILL.md |
| ○ | software-development | node-inspect-debugger | Debug Node.js via --inspect + Chrome DevTools Protocol CLI. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\node-inspect-debugger\SKILL.md |
| ○ | software-development | python-debugpy | Debug Python: pdb REPL + debugpy remote (DAP). | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\python-debugpy\SKILL.md |
| — | apple | findmy | Track Apple devices/AirTags via FindMy.app on macOS. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\apple\findmy\SKILL.md |
| — | apple | imessage | Send and receive iMessages/SMS via the imsg CLI on macOS. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\apple\imessage\SKILL.md |
| — | autonomous-ai-agents | claude-code | Delegate coding to Claude Code CLI (features, PRs). | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\autonomous-ai-agents\claude-code\SKILL.md |
| — | autonomous-ai-agents | computer-use | Drive the desktop in the background without stealing focus. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\autonomous-ai-agents\computer-use\SKILL.md |
| — | autonomous-ai-agents | hermes-agent | Use, configure, theme, extend, and orchestrate Hermes Agent. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\autonomous-ai-agents\hermes-agent\SKILL.md |
| — | creative | popular-web-designs | 54 real design systems (Stripe, Linear, Vercel) as HTML/CSS. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\popular-web-designs\SKILL.md |
| — | creative | sketch | Throwaway HTML mockups: 2-3 design variants to compare. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\sketch\SKILL.md |
| — | devops | sdlc-review | Review Kanban handoffs and route verified outcomes. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\devops\sdlc-review\SKILL.md |
| — | productivity | airtable | Airtable REST API via curl. Records CRUD, filters, upserts. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\airtable\SKILL.md |
| — | productivity | maps | Geocode, POIs, routes, timezones via OpenStreetMap/OSRM. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\maps\SKILL.md |
| — | productivity | product-price-monitor | Watch product, flight, or listing prices; alert on target. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\productivity\product-price-monitor\SKILL.md |
| — | research | grounded-citations | Ground answers and documents in cited, verifiable sources. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\research\grounded-citations\SKILL.md |
| — | research | research-paper-writing | Write ML papers for NeurIPS/ICML/ICLR: design→submit. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\research\research-paper-writing\SKILL.md |
| — | smart-home | openhue | Control Philips Hue lights, scenes, rooms via OpenHue CLI. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\smart-home\openhue\SKILL.md |
| — | software-development | dogfood | Exploratory QA of web apps: find bugs, evidence, reports. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\dogfood\SKILL.md |
| — | software-development | hermes-agent-skill-authoring | Author in-repo SKILL.md files: frontmatter and structure. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\hermes-agent-skill-authoring\SKILL.md |
| — | software-development | inspecting-hermes-desktop-dom | Read the live Hermes desktop DOM/CSS over CDP. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\inspecting-hermes-desktop-dom\SKILL.md |
| — | software-development | plan | Write a markdown plan to .hermes/plans/; no execution. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\plan\SKILL.md |
| — | software-development | requesting-code-review | Pre-commit review: security scan, quality gates, auto-fix. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\requesting-code-review\SKILL.md |
| — | software-development | simplify-code | Parallel 4-agent cleanup of recent code changes. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\simplify-code\SKILL.md |
| — | software-development | spike | Throwaway experiments to validate an idea before build. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\spike\SKILL.md |
| — | software-development | systematic-debugging | 4-phase root cause debugging: understand bugs before fixing. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\systematic-debugging\SKILL.md |
| — | software-development | test-driven-development | TDD: enforce RED-GREEN-REFACTOR, tests before code. | C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\software-development\test-driven-development\SKILL.md |
