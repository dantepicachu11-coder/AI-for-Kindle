# AI Chat for Kindle Paperwhite

A web-based chat application with AI designed specifically for the old browser of the Kindle Paperwhite 7th generation (2015). Also compatible with modern browsers.

## Features

- **3 AI models to choose from**: Groq (Llama 3.1), Google Gemini 2.5 Flash, Cohere Command R+
- **Document loading from GitHub**: Upload `.txt` files to the `docs/` folder of your repo and load them as context in the chat
- **Custom instructions system**: Define how the AI should behave (default is educational/exam-style configuration)
- **URL-based persistence**: Save your API keys in a Kindle bookmark — they load automatically on every visit
- **Kindle 7th gen compatible**: Works with Kindle's old WebKit browser — no `fetch()`, no ES6, no flexbox

## Requirements

You need **at least one API key** from:

1. **Groq** (recommended — free tier): https://console.groq.com
   - Llama 3.1 8B Instant, up to 14,400 requests/day free
   - Key begins with `gsk_`

2. **Google Gemini** (free with Google AI Studio): https://aistudio.google.com
   - Gemini 2.5 Flash
   - Key begins with `AIza`

3. **Cohere** (free): https://dashboard.cohere.com
   - Command R+ model
   - Custom key format

Also optional but recommended:

- **GitHub token**: https://github.com/settings/tokens
  - Mark only `public_repo` scope
  - Key begins with `ghp_`

## Installation

1. Clone or fork this repository
2. Enable GitHub Pages in your repo settings (Settings → Pages → Deploy from main)
3. Make sure the file is named `index.html` in the root

## Getting Started

### Option A: Via Bookmark (recommended for Kindle)

1. Open the page in any browser: `https://yourusername.github.io/yourrepository/`
2. Fill in the fields with your API keys
3. Click **Generate Bookmark URL**
4. Copy the long URL that appears
5. On your Kindle, save that URL as a bookmark
6. Next time you open the bookmark, your keys load automatically

**Example URL:**
```
https://yourusername.github.io/yourrepository/?gk=YOUR_GROQ_KEY&gem=YOUR_GEMINI_KEY&gh=YOUR_GITHUB_TOKEN&ai=groq
```

Parameters:
- `gk=` → Groq key
- `gem=` → Gemini key  
- `co=` → Cohere key
- `gh=` → GitHub token
- `ai=` → Default AI (`groq`, `gemini`, or `cohere`)

### Option B: Enter keys each session

1. Open the page
2. Type your keys in the fields
3. Chat freely
4. When you close the browser, keys are lost (Kindle limitation)

## Document Loader

1. Create a `docs/` folder in your repo
2. Upload `.txt` files (plain text files)
3. Inside the app, click **Refresh file list**
4. Select the files you want to use
5. The content is sent to the chat as context

**Example:** If you have `notes.txt` in `docs/`, you can load that file and ask questions based on its content.

## Custom Instructions

The **Instructions for the AI** field controls AI behavior.

**Default instructions** (educational, exam-style):
- Respond in the user's language
- For math: verify calculations step by step
- For theory: answer as in an exam
- Length: ~4 sentences per response

You can change them anytime. The **Reset to Default** button restores the originals.

Instructions are saved in browser localStorage (in-session only, not persisted to URL).

## Kindle Paperwhite 7th Gen Limitations

- **No persistent localStorage**: Data is lost when closing the browser (that's why we use URLs)
- **No position:fixed**: Layout adapts to natural page scrolling
- **No flexbox**: Basic CSS only
- **No fetch() API**: Uses XMLHttpRequest instead
- **No ES6**: Classic JavaScript only (`var`, no arrow functions)
- **No CORS on raw.githubusercontent.com**: Files are loaded via GitHub API instead

## Repository Structure

```
your-repo/
├── index.html              # The web application
├── docs/                   # Documents folder (create if doesn't exist)
│   ├── notes.txt
│   ├── summary.txt
│   └── ...
└── README.md              # This file
```

## Troubleshooting

### Error 401 when using Gemini
The key is invalid or associated with a restricted project. Try generating it from **aistudio.google.com** (not Google Cloud Console).

### "error loading" when loading documents
Make sure:
- The `docs/` folder exists in your repo
- You have a valid GitHub token (optional but helpful)
- Files are `.txt` (plain text)

### Keys don't save on Kindle
It's a Kindle browser limitation with localStorage on external domains. Use the bookmark URL option instead.

### Chat looks small/cut off on Kindle
Zoom in your Kindle browser (pinch-to-zoom or menu). The page is optimized for readability.

## Performance

- **Groq Llama 3.1**: Very fast (~1-2 sec), short responses (1024 tokens)
- **Gemini 2.5 Flash**: Fast (~2-3 sec), longer responses
- **Cohere Command R+**: Moderate (~2-3 sec), very detailed

Groq tokens are limited (14,400/day free). Gemini and Cohere have different limits based on your plan.

## Privacy & Security

- Keys are stored in the bookmark URL or browser localStorage
- **Warning**: If someone has physical access to your Kindle, they can see your keys
- Don't share URLs publicly with keys included
- If you revoke a key, update your bookmark

## Credits & APIs

Built to work with old hardware (Kindle Paperwhite 7th gen).

Uses:
- Groq API (https://groq.com)
- Google Gemini API (https://ai.google.dev)
- Cohere API (https://cohere.com)
- GitHub API (https://docs.github.com)

## FAQ

**Does it work on other browsers?**
Yes, it works the same on modern browsers. Kindle limitations don't affect other devices.

**Can I use other AI models?**
Sure. The code is customizable. Just add a new `sendOtherAI()` function following the Groq/Gemini/Cohere pattern.

**What happens if I run out of tokens?**
You'll get an API error (401, 429, etc.). Wait for your next billing period or use a different AI.

**Can I save the chat?**
Not automatically. You can copy the conversation manually from the browser.

**Can I host this on a different platform?**
Yes. Any static hosting (GitHub Pages, Vercel, Netlify, etc.) works. Just update the `GITHUB_USER` and `GITHUB_REPO` variables in the code if needed.

---

**Last updated**: June 2026
