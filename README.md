# 🤖 RigPulse AI (formerly PCPower Analyzer)

RigPulse AI is a state-of-the-art, AI-powered hardware intelligence suite and PC performance analyzer. It enables users to instantly input their system specifications through multiple convenient methods, generating deep, interactive, and shareable hardware diagnostic reports. 

Whether checking if a laptop can run the latest AAA game, seeking localized LLM compatibility, detecting performance bottlenecks, or looking for a prioritized upgrade roadmap, RigPulse AI provides comprehensive, custom-tailored hardware insights.

---

## 🚀 Key Features

*   **Multi-Input Specs Wizard**
    *   **Manual Form:** Select specific components from a curated dropdown system.
    *   **Direct Paste:** Paste raw system summary outputs (such as Windows `dxdiag` or system reports).
    *   **Screenshot Upload:** Upload a screenshot of your System properties (processed via OCR parser).
    *   **File Upload:** Import a standard `.txt` DxDiag file export.
*   **10-Category Performance Engine**
    *   Normalized 0–100 performance intelligence scoring across: *Gaming, Coding, Video Editing, AI/ML, Streaming, Productivity, Multitasking, 3D Rendering, and Graphic Design*.
*   **Advanced Bottleneck Visualizer**
    *   Calculates hardware balance and flags bottlenecks across the CPU, GPU, VRAM capacity, RAM capacity/speed, and storage read/write performance.
*   **Smart Upgrade Roadmap**
    *   Generates prioritized component upgrade paths complete with cost estimations, performance-gain projections (%), and custom advice.
*   **Game Compatibility Database**
    *   Evaluates specifications against 50+ popular titles to predict FPS ranges, optimal settings presets, and recommended resolutions (1080p, 1440p, 4K).
*   **Local AI & LLM Advisor**
    *   Projects compatibility with open-source LLMs (e.g., Llama 3, Mistral, DeepSeek), determining memory fitment and estimated token-generation speed (tokens/sec).
*   **Professional Software Suitability**
    *   Analyzes system capacity against productivity, engineering, and creation suites like Adobe Premiere Pro, Blender, VS Code, and Photoshop.
*   **Side-by-Side Device Comparison**
    *   Enables detailed spec comparison between two systems with custom-calculated winner metrics.

---

## 🛠️ Technology Stack

*   **Framework:** [Next.js 16](https://nextjs.org/) (App Router, Dynamic SSR, & Route Handlers)
*   **Language:** [TypeScript](https://www.typescriptlang.org/)
*   **Styling & UI:** Tailwind CSS v4 (PostCSS integration) combined with custom Vanilla CSS variables for glassmorphic dark-mode effects, and [Framer Motion](https://www.framer.com/motion/) for micro-animations.
*   **Database:** PostgreSQL mapped through [Prisma ORM v7](https://www.prisma.io/)
*   **AI Integration:** [Vercel AI SDK](https://sdk.vercel.ai/docs) + OpenAI client libraries
*   **Authentication:** [Auth.js (NextAuth.js v5)](https://authjs.dev/)
*   **Validation:** [Zod](https://zod.dev/) (runtime verification schemas)
*   **Caching & Rates:** Redis (for session caching and request rate-limiting)
*   **Object Storage:** Cloudflare R2 (for storing uploaded system screenshots and files)

---

## 📂 Project Architecture

```filepath
d:\buildingcool\day1\
├── prisma/                    # Database models, schemas, and migrations
│   └── schema.prisma          # PostgreSQL relational schema
├── src/
│   ├── app/                   # App Router pages and API routes
│   │   ├── api/               # API endpoints (auth, spec parsing, reports)
│   │   ├── analyze/           # Entry point for the spec input wizard
│   │   ├── ai-advisor/        # Local LLM compatibility advisor
│   │   ├── compare/           # Side-by-side device comparison page
│   │   └── report/            # User-facing custom report display
│   ├── components/            # Reusable UI components
│   │   ├── analysis/          # Spec Wizard and text/file parse components
│   │   ├── report/            # Scorecards, upgrade timelines, and compatibility grids
│   │   └── layout/            # Navigation, footer, and shell frames
│   └── lib/                   # Heuristic scoring and analysis engines
│       ├── analysis/          # Scoring formulas, bottleneck math, LLM speeds, and game specs
│       ├── db.ts              # Global Prisma client instance
│       └── auth.ts            # Authentication configuration
├── docker-compose.yml         # Dev services configuration (Postgres, Redis)
└── package.json               # Package declarations and dependency manifest
```

---

## ⚙️ Getting Started

### Prerequisites

Ensure you have the following installed:
*   [Node.js](https://nodejs.org/) (v18.x or later recommended)
*   [Docker Desktop](https://www.docker.com/products/docker-desktop/) (for local services)

### Step 1: Install Dependencies

Clone the repository and run:

```bash
npm install
```

### Step 2: Spin Up Local Services

Use Docker Compose to launch local PostgreSQL and Redis containers in the background:

```bash
docker-compose up -d
```

### Step 3: Setup Environment Variables

Create your local environment file:

```bash
cp .env.example .env.local
```

Open `.env.local` and configure your credentials:

| Variable | Description | Default / Example |
| :--- | :--- | :--- |
| `NEXT_PUBLIC_APP_URL` | URL base for local redirects | `http://localhost:3000` |
| `DATABASE_URL` | Prisma DB connection string | `postgresql://pcpower:pcpower@localhost:5432/pcpower_db` |
| `AUTH_SECRET` | NextAuth secure key | Generate with `openssl rand -base64 32` |
| `OPENAI_API_KEY` | OpenAI credential (for text analysis / OCR summaries) | `sk-...` |
| `REDIS_URL` | Cache and Rate Limiting connection | `redis://localhost:6379` |
| `R2_BUCKET_NAME` | Cloudflare R2 storage bucket | `pcpower-uploads` |

### Step 4: Run Database Migrations

Apply the database schema to your local PostgreSQL instance:

```bash
npx prisma db push
```

*(Optional)* Run database migrations if you are running in production environments:

```bash
npx prisma migrate dev
```

### Step 5: Run the Development Server

Start Next.js in development mode:

```bash
npm run dev
```

Your server will boot at [http://localhost:3000](http://localhost:3000).

---

## 🔒 Production & Deployment

### Production Builds

To package and run the application in production mode:

```bash
npm run build
npm run start
```

### Hosting on Vercel
1. Link your git repository to Vercel.
2. Set your environment variables in the Vercel Dashboard (ensure you set up a hosted PostgreSQL server and hosted Redis instance, e.g., Upstash).
3. Vercel automatically runs the build steps and provisions standard deployments.
