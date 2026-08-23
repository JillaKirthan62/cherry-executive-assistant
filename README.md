# 🍒 CHERRY EXECUTIVE AI ASSISTANT

### MyChart Bot — Production-Grade, Streaming, Fact-Grounded Conversational AI

Cherry Executive AI Assistant is a production-grade conversational AI web application designed to provide professionals with fast, accurate, trustworthy, and resilient assistance for everyday executive work.

The platform combines **streaming AI responses, live fact verification, web grounding, persistent conversations, document scanning, voice interaction, authentication, security hardening, and model-failure resilience** in a single full-stack web application.

## 🚀 Live Demo

**Try Cherry Executive AI Assistant:**

👉 #This Chat bot Link:[https://cherry-executive-ai-assistant.lovable.app]

## 📌 Project Overview

Traditional AI assistants can struggle with:

* Outdated information
* Unsupported or unverifiable claims
* Lost conversation history
* Poor document workflows
* Generic provider failure messages
* Weak user-data isolation
* Limited voice and accessibility support

Cherry addresses these problems through **request-time grounding, persistent authenticated conversations, structured file scanning, resilient AI provider handling, and database-level security**.

---

## ✨ Key Features

### 🤖 AI Conversational Assistant

* Natural-language conversational interface
* Token-by-token streaming responses
* Visible thinking/loading state
* Markdown responses
* Code blocks and lists
* Professional executive-oriented response style

### 🔎 Live Fact Verification & Grounding

Cherry does not rely exclusively on the model's static knowledge for volatile information.

The system can:

* Resolve time-sensitive entities at request time
* Retrieve structured factual information
* Perform web/encyclopaedic grounding
* Inject verified context into the model prompt
* Display grounding sources alongside responses

This approach is specifically designed to reduce factual staleness and make answers easier to verify.

### 💬 Persistent Chat History

* Create conversations
* Automatically title conversations
* Browse previous conversations
* Reopen complete conversation history
* Rename conversations
* Delete conversations
* Persist messages server-side

Conversation data is protected using PostgreSQL **Row Level Security (RLS)** so authenticated users can access only their own conversations.

### 📄 Document Intelligence

Cherry supports text-based document workflows.

Users can:

* Upload files
* Drag and drop files
* Preview attachments
* Validate file types
* Extract text server-side
* Scan uploaded content
* Receive a structured file summary
* Ask questions using the uploaded content

Supported text formats include:

```text
.txt
.md
.csv
.tsv
.json
.log
.yaml
Office text documents
```

Images and PDFs are intentionally excluded from the current implementation to maintain predictable scanning behaviour.

### 🎙️ Voice Interaction

* Browser-native speech-to-text
* Microphone-based input
* Text-to-speech responses
* Optional spoken assistant replies

Voice functionality depends on browser speech API support and is currently considered a beta capability.

### 🔐 Authentication & Security

* Google authentication
* Protected chat workspace
* Route guarding
* Server-side session handling
* PostgreSQL Row Level Security
* Strict request validation
* Payload size limits
* Content Security Policy
* HSTS
* Anti-sniffing protections
* Clickjacking protection
* Restrictive permissions policy
* Server-only API secrets

The database enforces per-user authorization rather than relying only on application-level checks.

### 🛡️ AI Resilience

Cherry is designed to continue operating when AI providers experience problems.

The platform includes:

* Model fallback chain
* Circuit breaker
* Exponential backoff
* Bounded retries
* Rate-limit handling
* Quota handling
* Provider outage handling
* Trace identifiers for troubleshooting

This prevents failures from appearing as an unexplained infinite loading state.

### 📥 Transcript Export

Users can export the current conversation as a transcript for later reference.

---

# 🏗️ Architecture

Cherry follows a **single-deployable full-stack architecture** running at the network edge.

```text
                         ┌─────────────────────┐
                         │       User          │
                         │ Browser / Client    │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   React 19 UI       │
                         │   TanStack Start    │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │  Edge Server Route  │
                         │  Validation/Auth    │
                         └──────────┬──────────┘
                                    │
                  ┌─────────────────┼─────────────────┐
                  │                 │                 │
                  ▼                 ▼                 ▼
           ┌─────────────┐  ┌─────────────┐  ┌──────────────┐
           │   Grounding │  │  AI Gateway │  │ PostgreSQL   │
           │ Fact Search │  │   Gemini    │  │ Chat History │
           └─────────────┘  └─────────────┘  └──────────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │ Streaming Response  │
                         │ + Sources + Status  │
                         └─────────────────────┘
```

## The application uses separate presentation, routing, application, integration, and data layers, with validation, logging, security headers, and error mapping applied across the system.

# 🧰 Technology Stack

| Category             | Technology                         |
| -------------------- | ---------------------------------- |
| Frontend             | React 19                           |
| Full-stack Framework | TanStack Start                     |
| Language             | TypeScript                         |
| Styling              | Tailwind CSS v4                    |
| AI SDK               | Vercel AI SDK                      |
| AI Gateway           | Lovable AI Gateway                 |
| AI Models            | Gemini family                      |
| Backend              | Edge server routes                 |
| Runtime              | Cloudflare-compatible Edge Runtime |
| Database             | PostgreSQL                         |
| Authentication       | Google OAuth / Managed Auth        |
| Database Security    | Row Level Security                 |
| Validation           | Zod                                |
| Package Manager      | Bun                                |
| Deployment           | Lovable / Global Edge Runtime      |
| Static Assets        | CDN                                |

The documented technology stack is TanStack Start, React 19, TypeScript, Tailwind CSS v4, Vercel AI SDK, Lovable AI Gateway with Gemini, Lovable Cloud PostgreSQL/Auth, and Cloudflare Edge Runtime.

---

# 📂 Core System Design

The chat request follows a controlled processing pipeline:

```text
Request
   ↓
Size Guard
   ↓
JSON Parsing
   ↓
Schema Validation
   ↓
Authentication
   ↓
File Extraction
   ↓
Fact Verification
   ↓
Web Grounding
   ↓
Model Selection
   ↓
AI Generation
   ↓
Token Streaming
   ↓
Message Persistence
```

Each stage has a defined failure path, including `400` validation responses, `413` oversized payload responses, fallback-model selection, retry handling, and graceful provider failure.

---

# 🗄️ Database

The application uses a deliberately small conversational data model.

### `chat_threads`

Stores:

* Thread ID
* User ID
* Title
* Creation timestamp
* Last updated timestamp

### `chat_messages`

Stores:

* Message ID
* Thread ID
* User ID
* Role
* Structured message parts
* File references
* Grounding metadata
* Creation timestamp

Row Level Security ensures that authenticated users can access only records belonging to their own user ID.

---

# 🔧 Local Development

## Prerequisites

* Node.js 20+ **or Bun**
* AI Gateway credentials
* Managed cloud backend credentials
* Modern web browser

The project's technical manual specifies Node.js 20 or later/Bun and a modern browser as development prerequisites.

## Installation

Clone the repository:

```bash
git clone https://github.com/JillaKirthan62/cherry-executive-assistant.git
cd cherry-executive-assistant
```

Install dependencies:

```bash
bun install
```

## Environment Configuration

Create a local environment file and configure:

```env
# Server-only
LOVABLE_API_KEY=<ai-gateway-key>
GEMINI_API_KEY=<google-ai-studio-key>

# Client-safe
VITE_SUPABASE_URL=<backend-url>
VITE_SUPABASE_PUBLISHABLE_KEY=<publishable-key>
```

**Important:** server-only keys must never be exposed to the browser. The production implementation reads server-scoped values inside request handlers rather than bundling them into client-side code.

## Start Development Server

```bash
bun run dev
```

The development server runs at:

```text
http://localhost:8080
```

## Run Linting

```bash
bun run lint
```

## Build Production Bundle

```bash
bun run build
```

The documented local workflow uses `bun install`, `bun run dev`, `bun run lint`, and `bun run build`.

---

# 🧪 Testing

Cherry uses a layered testing strategy.

### Static Analysis

* TypeScript compiler
* ESLint

### Unit Testing

Tests cover logic including:

* Request validation
* Decoding
* File extraction
* Error mapping

### Integration Testing

The chat route is exercised end-to-end with stubbed providers.

### Functional Testing

Real user journeys are tested against the deployed application.

### Non-Functional Testing

Tests cover:

* Performance
* Security headers
* HTTPS enforcement
* Data isolation
* Payload limits
* Keyboard accessibility
* Responsive layouts
* Provider recovery
* Traceability

## Test Results

The documented test programme reported:

* **26/26 functional scenarios passed**
* **15/15 non-functional scenarios passed**
* **61 recorded test cases passed overall**
* Median time to first token: **0.8 seconds**
* First Contentful Paint: **1.1 seconds**
* Grounding overhead: **410 ms median**
* Cross-user data isolation verified
* Security headers verified
* Oversized payload handling verified
* Provider recovery verified

---

# 📊 Performance Targets

| Metric                 |               Target |     Observed |
| ---------------------- | -------------------: | -----------: |
| Time to first token    |               < 1.5s |  0.8s median |
| First Contentful Paint |               < 1.5s |         1.1s |
| Grounding overhead     |              < 900ms | 410ms median |
| Concurrent sessions    |            No errors |    No errors |
| Oversized payload      |                  413 |       Passed |
| Malformed JSON         |                  400 |       Passed |
| Security headers       |          All present |       Passed |
| Data isolation         | No cross-user access |       Passed |

---

# 🚀 Deployment

Cherry is deployed as a single application bundle to a global edge runtime.

The deployment pipeline follows:

```text
Git Repository
      ↓
Install Locked Dependencies
      ↓
Lint + Type Check
      ↓
Production Build
      ↓
Deploy to Edge Runtime
      ↓
Smoke Test
      ↓
Live Application
```

The documented deployment process includes dependency installation, linting/type checking, production compilation, edge deployment, and a smoke test against the public application and chat endpoint.

### Live Application

https://cherry-executive-ai-assistant.lovable.app

---

# 🔐 Security Principles

Cherry follows a security-by-default approach.

### Application Security

* Strict request schema validation
* Payload size restrictions
* Hardened HTTP headers
* Content Security Policy
* HSTS
* Anti-sniffing protection
* Clickjacking protection
* Permissions Policy
* HTTPS-only transport

### Data Security

* Google authentication
* Authenticated routes
* PostgreSQL Row Level Security
* Per-user database authorization
* Anonymous users cannot access conversational data
* Server secrets remain server-side

### Operational Security

* Trace IDs
* Structured logs
* Explicit status mapping
* Provider failure handling
* Circuit breaker
* Retry limits

The architecture deliberately enforces authorization at the database layer, reducing the impact of application-layer authorization defects.

---

# 🎯 Project Scope

## Included

* Public landing page
* Pricing page
* Google authentication
* Protected chat workspace
* Streaming AI responses
* Live fact verification
* Web grounding
* Persistent chat history
* Text-file upload
* File scanning and summarization
* Voice input/output
* Transcript export
* Security hardening
* Edge deployment

## Currently Out of Scope

* Native mobile applications
* AI model fine-tuning
* Self-hosted foundation models
* Multi-user team workspaces
* Shared threads and role hierarchies
* Payment/subscription processing
* Offline operation
* Image ingestion
* PDF ingestion

---

# 🧠 Design Philosophy

The project focuses on the engineering surrounding the AI model rather than treating the language model as the complete product.

The core principles are:

> **Accuracy through grounding.**
> **Transparency through sources.**
> **Reliability through resilience.**
> **Security through database-level authorization.**
> **Performance through streaming and edge execution.**
> **Extensibility through modular integrations.**

The project documentation identifies failure handling as a first-class engineering concern and highlights context construction, provenance, authorization, persistence, resilience, and interface discipline as key differences between a chatbot demonstration and a professional assistant.

---

# 🔮 Future Roadmap

### Near Term

* Automated browser end-to-end testing
* Distributed circuit-breaker state across edge locations

### Medium Term

* General retrieval over user-owned document libraries
* Pluggable retrieval interfaces
* Streaming ingestion for very large documents

### Long Term

* Team workspaces
* Shared threads and roles
* Calendar and email actions through tool calling

### Vision

* Autonomous multi-step executive workflows
* AI capable of delegating and completing complete tasks

---

# 📚 Documentation

The complete project documentation covers:

* Requirements analysis
* System architecture
* System design
* Database design
* AI design
* Retrieval and grounding
* Voice interaction
* File handling
* Authentication
* Security
* Implementation
* Testing
* Deployment
* Technical manual
* User guide
* Project evaluation
* Future roadmap

The report contains **47 engineering diagrams** documenting the system architecture, workflows, data flow, database model, security model, deployment, and other engineering decisions.

---

# 👨‍💻 Author

**Kirthan**

Computer Science / Artificial Intelligence & Machine Learning

## Project

**Cherry Executive AI Assistant — MyChart Bot**

## Live Application

https://cherry-executive-ai-assistant.lovable.app

## GitHub Repository

https://github.com/JillaKirthan62/cherry-executive-assistant

---

# 📄 License

This project is distributed under the **MIT License**.

---

## ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub.

---

### 🍒 Cherry Executive AI Assistant

**Current. Transparent. Resilient. Secure.**

A conversational AI assistant engineered for professional workflows.

