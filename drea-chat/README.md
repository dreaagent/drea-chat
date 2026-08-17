# Drea Chat

> A minimal, local-first LLM console for any OpenAI-compatible endpoint. Streamed, complete, never truncated.

Drea Chat is a client-side web application built with vanilla JavaScript. It provides a sleek, distraction-free interface for interacting with Large Language Models (LLMs) directly from your browser. It connects to any OpenAI-compatible API (local or remote) and offers advanced features like conversation branching, secure API key encryption, and robust streaming.

## 🌌 Philosophy

Drea Chat was built on a few core principles:

1. **Local-First & Private**: Your conversations never leave your browser. There is no backend, no server, and no telemetry. All chats and settings are stored locally in your browser's IndexedDB. 
2. **Uncompromised Streaming**: LLM endpoints can be flaky. Drea Chat uses a robust, line-buffered Server-Sent Events (SSE) parser that ensures no token is ever dropped, even on chunky connections.
3. **Exploratory Chatting**: Conversations aren't just linear logs. Drea Chat uses a conversation tree structure, allowing you to edit past prompts, branch off into alternate timelines, and regenerate responses without losing your history.
4. **Endpoint Agnostic**: Whether you are running `llama.cpp` locally, using Ollama, or hitting the OpenAI API, Drea Chat adapts. It includes support for advanced, non-standard samplers (like Min P and Power Law) for power users.
5. **Security by Design**: Your API keys are the keys to your wallet. Drea Chat includes an optional "Vault" mode that encrypts your API key with AES-256-GCM before it ever touches your disk.

---

## ✨ Features

- **Robust SSE Streaming**: Line-buffered parsing guarantees complete responses.
- **Conversation Branching**: Edit past user messages or regenerate assistant responses to explore different conversational paths. Switch between branches using the built-in version switcher.
- **IndexedDB Persistence**: Conversations are stored individually in IndexedDB, preventing the UI from blocking even with thousands of messages.
- **Vault Encryption**: Secure your API keys using Web Crypto API (PBKDF2 + AES-256-GCM). The password is never stored.
- **Image Attachments**: Paste, drag-and-drop, or upload images. They are automatically scaled and compressed locally before being sent as Base64 data URLs.
- **Cost Estimation**: Tracks token usage (if supported by the endpoint) and calculates estimated costs based on your configured input/output pricing.
- **Presets System**: Save your favorite endpoint, model, sampler, and API key configurations as presets. Switch between them instantly from the top bar.
- **Advanced Samplers**: Supports Temperature, Top P, Top K, Min P, Power Law, and Adaptive samplers.
- **Import/Export**: Easily backup or migrate your entire chat history (with image data stripped to save space).
- **Markdown & Syntax Highlighting**: Renders markdown beautifully with `marked.js`, `highlight.js`, and sanitizes everything with `DOMPurify` to prevent XSS.
- **Fully Local Assets (Standard Version)**: All fonts and third-party scripts are bundled locally in the standard version. No CDN requests are made, ensuring complete offline capability and preventing IP leaks.
- **Single-File Convenience (CDN Version)**: For those who prefer a drop-in file without a directory structure, an `drea-chat-cdn.html` version is included. It bundles all HTML, CSS, and JS into one file and loads dependencies from CDNs.

---

## 🚀 Getting Started

Because Drea Chat uses modern browser APIs like IndexedDB, Fetch, and Web Crypto, it is highly recommended to serve it over `http://` or `https://` rather than opening it directly as a `file://` URL, as some browsers restrict cross-origin requests and storage APIs on the `file://` protocol.

The standard project is structured into web files:
- `index.html` (Main UI)
- `css/styles.css` (Styling and local `@font-face` declarations)
- `js/app.js` (Application logic)
- `third-party/` (Bundled libraries like `marked.min.js`, `purify.min.js`, etc.)
- `fonts/` (Local `.woff2` font files)

### Option 1: Use a simple local server (Recommended)
If you have Python installed, you can serve the directory easily:

```bash
# Clone the repository
git clone https://github.com/dreaagent/drea-chat.git
cd drea-chat

# Start a local server (Python 3)
python -m http.server 8000
```
Then, open your browser and navigate to `http://localhost:8000`.

### Option 2: VS Code Live Server
If you use Visual Studio Code, you can install the "Live Server" extension, right-click `index.html`, and select **Open with Live Server**.

### Option 3: Use the single-file CDN version
If you don't want to manage the folder structure, you can use `drea-chat-cdn.html`. This file contains the entire application inline and uses CDNs for Google Fonts and third-party libraries. 

Simply download `drea-chat-cdn.html` and host it or serve it locally. *(Note: Opening it via `file://` might still trigger browser security restrictions for IndexedDB/Fetch, so serving it via a local web server is still advised).*

---

## ⚙️ Configuration

1. Click the **Settings** button in the bottom left corner.
2. **Endpoint URL**: Enter your OpenAI-compatible base URL (e.g., `http://localhost:8080` for a local server, or `https://api.openai.com` for OpenAI).
3. **API Key**: Enter your key. If you want to encrypt it, check the **Vault Protection** box and set a vault password.
4. **Model**: Specify the model name (e.g., `meta-llama/Llama-3.3-70B-Instruct` or `gpt-4o`).
5. **System Prompt**: Define the behavior of the model.
6. **Samplers**: Toggle and adjust standard or advanced samplers as needed by your endpoint.
7. Click **Save**. 

*Tip: You can save this configuration as a "Preset" by typing a name in the Presets section and clicking "Save current".*

---

## 🔒 Security & Privacy

- **No Backend**: Drea Chat is 100% client-side. Your prompts, images, and API keys are only ever sent directly to the LLM endpoint you configure.
- **Vault Encryption**: If enabled, your API key is encrypted using AES-256-GCM. A password is derived using PBKDF2 (600,000 iterations). The plaintext key and password are kept in memory only while the app is open and are evicted when you lock the vault or close the tab.
- **Strict CSP**: The application includes a strict Content Security Policy meta tag. The standard version restricts script and style execution to local files (`'self'` and `file:`) only. The `drea-chat-cdn.html` version includes a modified CSP that safely permits the required CDNs (like `cdnjs.cloudflare.com` and `fonts.googleapis.com`).
- **Sanitized Markdown**: All AI-generated HTML is passed through `DOMPurify` to strip malicious scripts before rendering.

---

## 🛠️ Tech Stack

- **Vanilla JavaScript (ES6+)**: No build step, no frameworks, no external runtime dependencies.
- **IndexedDB**: For robust, asynchronous local storage of conversations and settings.
- **Web Crypto API**: For secure, native browser encryption.
- **Third-Party Libraries**:
  - `marked.js` (Markdown parsing) — MIT License
  - `highlight.js` (Syntax highlighting) — BSD 3-Clause License
  - `DOMPurify` (HTML sanitization) — Apache License 2.0
  - `github-syntax-dark` (Code block styling) — MIT License
  - Fonts: Geist, Instrument Serif, JetBrains Mono — SIL Open Font License 1.1

---

## 📜 License

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

For licenses and attributions of the third-party libraries and fonts bundled with this project, please see [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page or submit a pull request. By contributing, you agree that your contributions will be licensed under the Apache 2.0 License.
