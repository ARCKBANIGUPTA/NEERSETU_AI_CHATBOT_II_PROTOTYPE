# NEERSETU - SIH2025

**India Groundwater Resource Information System (INGRES)** - An AI-powered platform for analyzing and querying groundwater data across India using RAG (Retrieval-Augmented Generation) and conversational AI.
## Link for the demo video: https://drive.google.com/drive/u/0/folders/14ulrFaZ1JNn6AuYo_dXc9bCfb5VTzNUu
## 📋 Overview

NEERSETU is an intelligent groundwater data analysis platform that makes India's comprehensive groundwater data (covering 28 states, 8 UTs, 700+ districts, and 6,000+ blocks) accessible through natural language conversations. Built for SIH2025, it leverages modern AI technologies to provide real-time insights, visualizations, and analysis of groundwater resources.

### Key Features

- 🤖 **Conversational AI Assistant** - Ask questions in plain English about groundwater data
- 📊 **Smart Visualizations** - Auto-generated charts (bar, pie) and statistical summaries
- 🗄️ **Comprehensive Data** - Complete coverage of Indian states, districts, and taluks
- 🔍 **Advanced Search** - Filter by category (Safe, Critical, Over-Exploited), region, extraction levels
- 📈 **Detailed Metrics** - Rainfall, recharge, extraction, stage of extraction, and more
- 🌐 **Multi-Platform** - Web application + Chrome extension
- ⚡ **Real-time Streaming** - Streaming AI responses for better user experience

## 🏗️ Architecture

### System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Client Layer                              │
│  ┌──────────────────┐              ┌────────────────────┐       │
│  │  Next.js Web App │              │ Chrome Extension   │       │
│  │  (React 19)      │              │ (Content Script)   │       │
│  └────────┬─────────┘              └──────────┬─────────┘       │
│           │                                    │                 │
│           │         HTTP/REST API              │                 │
│           └────────────────┬───────────────────┘                 │
└────────────────────────────┼──────────────────────────────────────┘
                             │
┌────────────────────────────┼──────────────────────────────────────┐
│                   Backend Layer (Express.js)                      │
│                             │                                     │
│  ┌──────────────────────────┴───────────────────────────┐        │
│  │              API Routes Layer                         │        │
│  │  • /api/chat - RAG chat endpoints                    │        │
│  │  • /api/gw-chat - Groundwater AI agent               │        │
│  │  • /api/search - Vector similarity search            │        │
│  │  • /api/embed - Embedding generation                 │        │
│  └──────────────┬──────────────────────┬────────────────┘        │
│                 │                      │                          │
│  ┌──────────────┴────────┐  ┌─────────┴──────────┐              │
│  │  AI Agent Layer       │  │  RAG Pipeline      │              │
│  │                       │  │                    │              │
│  │  ┌─────────────────┐  │  │  ┌──────────────┐ │              │
│  │  │ LangGraph Agent │  │  │  │ Query →      │ │              │
│  │  │ (State Machine) │  │  │  │ Embedding →  │ │              │
│  │  └────────┬────────┘  │  │  │ Search →     │ │              │
│  │           │            │  │  │ Context →    │ │              │
│  │  ┌────────┴────────┐  │  │  │ LLM →        │ │              │
│  │  │   AI Tools      │  │  │  │ Response     │ │              │
│  │  │  • Find Location│  │  │  └──────────────┘ │              │
│  │  │  • Get Data     │  │  │                    │              │
│  │  │  • Compare      │  │  │                    │              │
│  │  │  • Top/Bottom   │  │  │                    │              │
│  │  │  • Category Sum │  │  │                    │              │
│  │  │  • Chart Gen    │  │  │                    │              │
│  │  └─────────────────┘  │  │                    │              │
│  └───────────────────────┘  └────────────────────┘              │
│                 │                      │                          │
│  ┌──────────────┴──────────────────────┴────────────────┐        │
│  │              Service Layer                            │        │
│  │  • Embeddings (Xenova Transformers - BGE-small)     │        │
│  │  • LLM (Groq - Llama 3.3 70B)                       │        │
│  │  • Vector Store (ChromaDB)                          │        │
│  │  • Location Search (Fuse.js)                        │        │
│  │  • Groundwater Service (Business Logic)            │        │
│  └──────────┬──────────────────────┬────────────────────┘        │
└─────────────┼──────────────────────┼───────────────────────────────┘
              │                      │
┌─────────────┴──────────┐  ┌────────┴─────────────┐
│  PostgreSQL Database   │  │  ChromaDB Vector DB  │
│  (Drizzle ORM)        │  │  (Embeddings Store) │
│                        │  │                      │
│  • locations          │  │  • Document chunks   │
│  • groundwater_data   │  │  • Embeddings        │
│  • hierarchical       │  │  • Metadata          │
│    relationships      │  │                      │
└────────────────────────┘  └──────────────────────┘
```

### Technology Stack

#### Backend (`/backend`)
- **Runtime**: Node.js 20+
- **Framework**: Express.js - REST API server
- **AI/ML**:
  - LangChain Core - LLM orchestration framework
  - LangGraph - State machine for AI agents
  - Groq SDK - Fast LLM inference (Llama 3.3 70B)
  - Xenova Transformers - Local embedding generation (BGE-small-en-v1.5)
- **Database**:
  - PostgreSQL - Structured groundwater data
  - Drizzle ORM - Type-safe database queries
  - ChromaDB - Vector embeddings storage
- **Utilities**:
  - Fuse.js - Fuzzy location search
  - Zod - Schema validation
  - TypeScript - Type safety

#### Frontend (`/frontend`)
- **Framework**: Next.js 16 (App Router)
- **UI Library**: React 19
- **Styling**:
  - Tailwind CSS 4 - Utility-first styling
  - HeroUI - Component library
  - Framer Motion - Animations
- **Visualization**: Recharts - Chart generation
- **Markdown**: react-markdown, remark-gfm, rehype-raw
- **TypeScript**: Full type safety

#### Extension (`/extension`)
- **Platform**: Chrome Extension (Manifest V3)
- **Framework**: React 18 + Vite
- **Build Tool**: @crxjs/vite-plugin
- **Styling**: Tailwind CSS 3
- **Icons**: Lucide React
- **Features**:
  - Content script injection
  - Background service worker
  - Chat widget overlay
  - Real-time API communication

#### Infrastructure
- **Containerization**: Docker Compose
  - ChromaDB container (port 8000)
  - PostgreSQL container (port 5432)
  - Adminer - Database management UI (port 8080)
- **Monorepo**: TurboRepo + pnpm workspaces
- **Dev Tools**: mise - Task runner

## 📁 Project Structure

```
sih2025/
├── backend/                    # Express.js API server
│   ├── src/
│   │   ├── index.ts           # Server entry point
│   │   ├── routes/            # API endpoints
│   │   │   ├── chat.ts        # RAG chat endpoints
│   │   │   ├── gwChat.ts      # Groundwater AI agent
│   │   │   ├── search.ts      # Vector search
│   │   │   └── embed.ts       # Embedding generation
│   │   ├── services/          # Business logic
│   │   │   ├── gwAgent.ts     # LangGraph AI agent
│   │   │   ├── gwTools.ts     # AI agent tools
│   │   │   ├── groundwaterService.ts  # Data queries
│   │   │   ├── llm.ts         # Groq LLM client
│   │   │   ├── embeddings.ts  # Local embeddings
│   │   │   ├── vectorStore.ts # ChromaDB operations
│   │   │   └── locationSearch.ts  # Fuzzy search
│   │   ├── db/                # Database layer
│   │   │   ├── gw-schema.ts   # Drizzle schema
│   │   │   ├── gw-db.ts       # Database client
│   │   │   └── drizzle/       # Migration files
│   │   └── scripts/           # Utility scripts
│   │       ├── seedGroundwaterData.ts
│   │       ├── ingest.ts
│   │       └── loadData.ts
│   ├── package.json
│   └── drizzle.config.ts
│
├── frontend/                   # Next.js web application
│   ├── app/
│   │   ├── page.tsx           # Landing page
│   │   ├── chat/page.tsx      # Chat interface
│   │   ├── layout.tsx         # Root layout
│   │   └── provider.tsx       # Context providers
│   ├── components/
│   │   ├── ChatComposer.tsx   # Chat input component
│   │   ├── MarkdownRenderer.tsx
│   │   ├── ChartRenderer.tsx  # Chart visualization
│   │   ├── InteractiveDemoSection.tsx
│   │   └── charts/
│   │       ├── BarChartComponent.tsx
│   │       ├── PieChartComponent.tsx
│   │       └── StatsChart.tsx
│   ├── package.json
│   └── next.config.ts
│
├── extension/                  # Chrome extension
│   ├── src/
│   │   ├── background/        # Service worker
│   │   ├── content/           # Content script
│   │   ├── popup/             # Extension popup
│   │   ├── components/        # React components
│   │   │   ├── ChatWidget/
│   │   │   ├── ChatMessage/
│   │   │   ├── ChatInput/
│   │   │   ├── FilterBar/
│   │   │   └── Suggestions/
│   │   ├── services/
│   │   │   └── api.ts         # Backend communication
│   │   └── types/
│   ├── public/
│   │   └── manifest.json
│   └── package.json
│
├── scripts/                    # Data processing scripts
│   ├── central_annexure_attribute_etl.py
│   ├── state_report_etl.py
│   ├── semantic_chunker.py
│   └── unified_dataset_generator_optimized.py
│
├── docker-compose.yml          # Docker services config
├── turbo.json                  # TurboRepo config
├── pnpm-workspace.yaml         # pnpm workspace config
└── package.json                # Root package.json
```

## 🚀 Getting Started

### Prerequisites

- **Node.js** >= 20
- **pnpm** >= 10.17.1
- **Docker** and Docker Compose
- **mise** (optional, for task running)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd SIH2025
   ```

2. **Install dependencies**
   ```bash
   pnpm install
   ```

3. **Set up environment variables**
   ```bash
   cd backend
   cp .env.example .env
   # Edit .env and add your GROQ_API_KEY
   ```

4. **Start Docker services**
   ```bash
   mise run docker:up
   # Or: docker-compose up -d
   ```

5. **Initialize database**
   ```bash
   # Push database schema
   mise run db:push
   # Or: cd backend && pnpm db:push

   # Seed groundwater data
   mise run db:seed
   # Or: cd backend && pnpm db:seed
   ```

6. **Run development servers**
   ```bash
   pnpm dev
   # This starts both backend and frontend
   ```

### Accessing the Application

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:3001
- **ChromaDB**: http://localhost:8000
- **PostgreSQL**: localhost:5432
- **Adminer** (DB admin): http://localhost:8080

## 📝 Available Commands

### Root Level
```bash
pnpm dev              # Start all development servers
pnpm build            # Build all packages
pnpm lint             # Lint all packages
pnpm clean            # Clean all build artifacts
```

### Using mise (Task Runner)
```bash
mise run dev          # Start all services
mise run docker:up    # Start Docker containers
mise run db:push      # Push database schema
mise run db:seed      # Seed database with data
```

### Backend
```bash
cd backend
pnpm dev              # Start backend dev server
pnpm build            # Build TypeScript
pnpm db:generate      # Generate Drizzle migrations
pnpm db:push          # Push schema to database
pnpm db:seed          # Seed groundwater data
pnpm ingest           # Ingest documents to vector store
```

### Frontend
```bash
cd frontend
pnpm dev              # Start Next.js dev server
pnpm build            # Build for production
pnpm start            # Start production server
```

### Extension
```bash
cd extension
pnpm dev              # Build extension in watch mode
pnpm build            # Build extension for production
pnpm icons            # Generate extension icons
```

## 🔑 Key Concepts

### RAG Pipeline
The system uses Retrieval-Augmented Generation to provide accurate, context-aware responses:
1. User query → Embedding generation
2. Vector similarity search in ChromaDB
3. Retrieve relevant document chunks
4. Construct context with retrieved data
5. Send to LLM with context
6. Stream response to user

### AI Agent (LangGraph)
The groundwater agent uses LangGraph state machine with multiple tools:
- **findLocation**: Search for states/districts/taluks
- **getGroundwaterData**: Retrieve data by location ID
- **compareLocations**: Compare multiple locations
- **getTopLocations**: Find top/bottom performers
- **getCategorySummary**: Summarize by category
- **generateChart**: Create visualization data

### Data Model
Hierarchical structure:
```
Country (India)
  └── State (e.g., Maharashtra)
      └── District (e.g., Pune)
          └── Taluk (e.g., Haveli)
```

Each location has groundwater metrics:
- Rainfall (mm)
- Ground Water Recharge (ham)
- Natural Discharges (ham)
- Extractable Resources (ham)
- Ground Water Extraction (ham)
- Stage of Extraction (%)
- Category (Safe/Critical/Over-Exploited/etc.)

## 🛠️ Development

### Adding New Features

1. **Backend**: Add routes in `backend/src/routes/`, services in `backend/src/services/`
2. **Frontend**: Add pages in `frontend/app/`, components in `frontend/components/`
3. **Database**: Modify schema in `backend/src/db/gw-schema.ts`, run `pnpm db:push`

### Environment Variables

Backend `.env`:
```env
# Required
GROQ_API_KEY=your_groq_api_key

# Optional (defaults shown)
PORT=3001
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/ingres
CHROMA_URL=http://localhost:8000
COLLECTION_NAME=ingres_groundwater
```

## 📊 Data Sources

- **CGWB Reports**: Central Ground Water Board official documentation
- **State Reports**: Annual groundwater assessments
- **District-level Data**: Detailed metrics for 700+ districts
- **Block-level Data**: Granular data for 6,000+ blocks

## 🤝 Contributing

This project was developed for SIH2025. For contributions or issues, please follow standard Git workflow.

## 📄 License



## 👥 Team

NEERSETU - SIH2025 Project Team
- **ARCK BANI GUPTA**
- **PRATHAM MEHTA**
- **TISHA SONI**
- **DEV PATEL**
- **TEJAS DETROJA**
- **VINIT THAKKAR**

---

Built with ❤️ for Smart India Hackathon 2025
