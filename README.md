# 🏠 HomeAI — AI-Powered Real Estate Platform

> A full-stack real estate web application with AI-powered property search, 360° virtual tours, real-time agent dashboard, and an intelligent AI assistant.

**Live Demo:** [imovelai.netlify.app](https://imovelai.netlify.app)

---

## ✨ Features

- **AI Property Search** — Natural language search powered by Claude (Anthropic API) via serverless proxy
- **Aria AI Assistant** — 24/7 chatbot that understands buyer intent, answers questions, and captures leads
- **360° Virtual Tours** — Immersive property tours integrated via Kuula
- **Agent Dashboard** — Full CRUD panel for agents: manage listings, schedule visits, view leads, mortgage calculator
- **Google OAuth Authentication** — Secure login via Supabase + Google OAuth
- **Mortgage Calculator** — Interactive financing simulator with amortization table
- **Mobile Responsive** — Fully optimized for all screen sizes
- **CI/CD Pipeline** — Auto-deploy on push via GitHub → Netlify

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla JS, HTML5, CSS3 |
| AI | Anthropic Claude API (claude-sonnet-4-6) |
| Auth | Supabase + Google OAuth |
| Database | Supabase (PostgreSQL) |
| Maps | Leaflet.js + OpenStreetMap |
| Virtual Tours | Kuula 360° |
| Serverless | Netlify Functions (Node.js) |
| Deploy | Netlify (CI/CD via GitHub) |

---

## 🚀 Getting Started

```bash
git clone https://github.com/WillBillz/imovelai.git
cd imovelai
```

### Environment Variables (Netlify)

```
ANTHROPIC_API_KEY=your_anthropic_key
SUPABASE_URL=your_supabase_url
SUPABASE_ANON_KEY=your_supabase_anon_key
```

### Deploy

Push to `main` branch — Netlify auto-deploys via the connected GitHub repo.

---

## 📁 Project Structure

```
imovelai/
├── index.html              # Main application (SPA)
├── netlify.toml            # Netlify config + redirects
├── netlify/
│   └── functions/
│       └── claude.js       # Serverless proxy for Anthropic API
└── assets/
    ├── favicon.ico
    ├── logo-nav.png
    └── og-image.png
```

---

## 🌍 Branches

| Branch | Description |
|---|---|
| `main` | English version — international portfolio |
| `pt-BR` | Portuguese version — original Brazilian client |

---

## 📸 Screenshots

> AI chat, property cards, agent dashboard, 360° virtual tour, mortgage calculator

---

## 👤 Author

**Willian de Paula Oliveira**  
Full-Stack Developer — React · Node.js · Supabase · AI Integration  
[GitHub](https://github.com/WillBillz) · [LinkedIn](https://linkedin.com/in/willian-de-paula-oliveira)

---

*Built as part of an international developer portfolio showcasing AI-integrated web applications.*
