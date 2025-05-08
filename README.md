# CrUX Analyzer Frontend

A React-based frontend that interacts with the CrUX Analyzer backend to analyze web performance metrics using the [Chrome UX Report (CrUX)](https://developers.google.com/web/tools/chrome-user-experience-report) API.

---

## Features

- Simple and responsive UI to input multiple URLs
- Visualizes metrics like:
  - First Contentful Paint (FCP)
  - Largest Contentful Paint (LCP)
  - Cumulative Layout Shift (CLS)
  - Interaction to Next Paint (INP)
  - Time to First Byte (TTFB)
- API integration with backend
- Error handling for invalid or unreachable URLs
- Dynamic result display with success & error summaries

---

## 🛠 Tech Stack

- React (via Create React App or Vite)
- Axios for API calls
- MUI
- dotenv for environment variables

---

## 📦 Installation

1. **Clone the repository:**

```bash
git clone https://github.com/your-username/crux-analyzer-frontend.git
cd crux-analyzer-frontend
```
