# FormForge AI 🤖

**AI-powered form builder and document autofill application.**  
Build custom forms, upload documents (PDF/image), extract data automatically using Claude AI, review results, and export.

---

## ✨ Features

| Feature | Details |
|---|---|
| **Dynamic Form Builder** | Add/edit/remove fields at runtime with live preview |
| **Field Types** | Text, Textarea, Number, Date, Dropdown, Checkbox |
| **Field Reordering** | Move fields up/down |
| **Templates** | Invoice, Personal ID, Medical Record, Job Application |
| **Document Upload** | PDF, PNG, JPG/JPEG with drag-and-drop support |
| **AI Extraction** | Claude vision API extracts data for any form schema |
| **Confidence Indicators** | Per-field confidence scores (High/Medium/Low) |
| **Review & Edit** | Manually edit extracted values, highlight missing required fields |
| **Export** | Download form schema as JSON; download filled data as JSON |
| **Demo Mode** | Works without an API key (simulated extraction) |
| **Multi-provider** | Anthropic Claude + OpenRouter support |

---

## 🚀 Quick Start

### Option A: Local Development

**Requirements:** Node.js 18+

```bash
# 1. Clone repository
git clone https://github.com/YOUR_USERNAME/formforge-ai.git
cd formforge-ai

# 2. Install dependencies
npm install

# 3. Start development server
npm run dev
```

Open **http://localhost:3000** in your browser.

### Option B: Google Colab

Open the notebook at `notebooks/FormForge_AI_Setup.ipynb` in Google Colab and run all cells. A public URL will be generated via cloudflared.

### Option C: Production Build

```bash
npm run build      # builds to /dist
npm run preview    # serves the built app
```

---

## ⚙️ Configuration

### API Key

When the app loads, it prompts for an API key. You can:

- **Skip** to use demo mode (simulated extraction with fake data)
- **Enter Anthropic key** (`sk-ant-...`) — [get one here](https://console.anthropic.com)
- **Enter OpenRouter key** (`sk-or-...`) — [get one here](https://openrouter.ai)

Keys are stored **in memory only** — never in localStorage or sent anywhere except the AI provider.

### Environment Variable (optional)

```bash
# Create .env.local
VITE_ANTHROPIC_API_KEY=sk-ant-your-key-here
```

---

## 📁 Project Structure

```
formforge-ai/
├── index.html                   # Entry point
├── package.json
├── vite.config.js
├── notebooks/
│   └── FormForge_AI_Setup.ipynb  # Google Colab setup
└── src/
    ├── main.jsx                  # React root
    ├── App.jsx                   # Main app (tabs, state)
    ├── App.css                   # Global styles
    ├── components/
    │   ├── FieldEditor.jsx       # Form field builder card
    │   ├── PreviewField.jsx      # Form field preview/input
    │   └── ApiKeyModal.jsx       # API key configuration modal
    ├── services/
    │   └── aiService.js          # Anthropic/OpenRouter extraction logic
    ├── utils/
    │   └── fileUtils.js          # JSON export helpers
    └── data/
        └── templates.js          # Built-in form templates
```

---

## 🔄 User Flow

```
1. Builder Tab   → Add fields, configure types & labels
2. Preview Tab   → See live form preview
3. Upload Tab    → Drop PDF or image document
                   → Click "Extract with AI"
4. Review Tab    → Inspect extracted values + confidence scores
                   → Edit any field manually
                   → Save / Download JSON
```

---

## 🧠 AI Extraction Details

The app sends a structured prompt to Claude that includes:
- The form schema (field names, types, required status, hints)
- The document (as base64 image or PDF)

Claude returns a JSON object with `value` and `confidence` for each field:

```json
{
  "Invoice Number": { "value": "INV-2024-001", "confidence": 95 },
  "Total Amount":   { "value": "1234.56",       "confidence": 88 },
  "Due Date":       { "value": null,             "confidence": 0  }
}
```

- Values with **confidence < 50** are left blank
- Required fields that remain empty are **highlighted in red**
- All values are **editable** before saving

---

## 🛡️ Edge Cases Handled

| Edge Case | Behavior |
|---|---|
| Upload before form creation | Error message, upload blocked |
| Unsupported file type | Clear error, no upload |
| Missing required fields | Highlighted red on Save |
| Low confidence extraction | Field left blank |
| API error | Error shown, no crash |
| No API key | Demo mode with simulated data |

---

## 🛠️ Technologies

| Layer | Technology |
|---|---|
| Frontend | React 18 + Vite |
| Styling | Pure CSS (custom design system) |
| AI Provider | Anthropic Claude (claude-opus-4) |
| Alternative AI | OpenRouter |
| Document Reading | Claude Vision API (images + PDFs) |
| Build Tool | Vite 5 |
| Colab Serving | cloudflared tunnel / ngrok |


---

## 📄 License

MIT — free to use and modify.
