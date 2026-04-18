# 🤖 CodeReviewAI

> **Ship better code, faster.** Automated AI-powered code reviews that catch bugs, security issues, and maintainability problems before they reach production.

![CodeReviewAI Preview](https://i.ytimg.com/vi/gRyMcPGF2o0/hqdefault.jpg)

---

## 🏷️ Badges

[![Next.js](https://img.shields.io/badge/Next.js_16-000000?style=for-the-badge&logo=next.js&logoColor=white)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![npm](https://img.shields.io/badge/npm-CB3837?style=for-the-badge&logo=npm&logoColor=white)](https://www.npmjs.com/)
[![pnpm](https://img.shields.io/badge/pnpm-F69220?style=for-the-badge&logo=pnpm&logoColor=white)](https://pnpm.io/)
[![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=prisma&logoColor=white)](https://www.prisma.io/)
[![Tailwind CSS v4](https://img.shields.io/badge/Tailwind_CSS_v4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![shadcn/ui](https://img.shields.io/badge/shadcn%2Fui-000000?style=for-the-badge&logo=shadcnui&logoColor=white)](https://ui.shadcn.com/)
[![GitHub OAuth](https://img.shields.io/badge/GitHub_OAuth-181717?style=for-the-badge&logo=github&logoColor=white)](https://docs.github.com/en/apps/oauth-apps)
[![GitHub Webhooks](https://img.shields.io/badge/GitHub_Webhooks-181717?style=for-the-badge&logo=github&logoColor=white)](https://docs.github.com/en/webhooks)
[![Better Auth](https://img.shields.io/badge/Better_Auth-6366F1?style=for-the-badge&logo=auth0&logoColor=white)](https://better-auth.com/)
[![Inngest](https://img.shields.io/badge/Inngest-5C2D91?style=for-the-badge&logo=inngest&logoColor=white)](https://www.inngest.com/)
[![Grok SDK](https://img.shields.io/badge/Grok_SDK-FF6600?style=for-the-badge&logo=x&logoColor=white)](https://x.ai/)

---

## ✨ Features

- 🔍 **Instant Feedback** — Comprehensive code reviews in seconds, not hours
- 🔒 **Security Scanning** — Automatically detect vulnerabilities and exposed secrets
- 💡 **Clear Suggestions** — Actionable feedback you can apply immediately
- 🔗 **PR Integration** — Reviews appear directly inside your pull requests
- 🧠 **Context Aware** — Understands your codebase patterns and style
- 🚀 **Always Improving** — Powered by the latest Grok AI models

---

## 🛠️ Tech Stack

| Layer              | Technology                  |
| ------------------ | --------------------------- |
| Framework          | Next.js 16 (App Router)     |
| Language           | TypeScript                  |
| Styling            | Tailwind CSS v4 + shadcn/ui |
| Database ORM       | Prisma (PostgreSQL)         |
| Authentication     | Better Auth + GitHub OAuth  |
| Background Jobs    | Inngest                     |
| AI Model           | Grok SDK (xAI)              |
| GitHub Integration | GitHub Webhooks             |
| Package Manager    | pnpm                        |

---

## 🚀 Getting Started

### Prerequisites

- Node.js 20+
- pnpm
- PostgreSQL database
- GitHub OAuth App
- Grok API key (xAI)

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/ai-code-reviewer.git
cd ai-code-reviewer

# Install dependencies
pnpm install

# Copy environment variables
cp .env.example .env
```

### Environment Variables

```env
DATABASE_URL="postgresql://..."

# GitHub OAuth
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=

# Better Auth
BETTER_AUTH_SECRET=
BETTER_AUTH_URL=http://localhost:3000

# Grok / xAI
GROK_API_KEY=

# Inngest
INNGEST_EVENT_KEY=
INNGEST_SIGNING_KEY=

# GitHub Webhooks
GITHUB_WEBHOOK_SECRET=
```

### Setup & Run

```bash
# Push database schema
pnpm prisma db push

# Generate Prisma client
pnpm prisma generate

# Start development server
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🗄️ Database Schema (ERD)

```mermaid
erDiagram
    User {
        String id PK
        String name
        String email UK
        Boolean emailVerified
        String image
        DateTime createdAt
        DateTime updatedAt
    }

    Session {
        String id PK
        DateTime expiresAt
        String token UK
        DateTime createdAt
        DateTime updatedAt
        String ipAddress
        String userAgent
        String userId FK
    }

    Account {
        String id PK
        String accountId
        String providerId
        String userId FK
        String accessToken
        String refreshToken
        String idToken
        DateTime accessTokenExpiresAt
        DateTime refreshTokenExpiresAt
        String scope
        String password
        DateTime createdAt
        DateTime updatedAt
    }

    Verification {
        String id PK
        String identifier
        String value
        DateTime expiresAt
        DateTime createdAt
        DateTime updatedAt
    }

    Repository {
        String id PK
        String userId FK
        Int githubId UK
        String name
        String fullName
        Boolean private
        String htmlUrl
        DateTime createdAt
        DateTime updatedAt
    }

    Review {
        String id PK
        String repositoryId FK
        String userId FK
        Int prNumber
        String prTitle
        String prUrl
        ReviewStatus status
        String summary
        Int riskScore
        Json comments
        String error
        DateTime createdAt
        DateTime updatedAt
    }

    User ||--o{ Session : "has"
    User ||--o{ Account : "has"
    User ||--o{ Repository : "owns"
    User ||--o{ Review : "requests"
    Repository ||--o{ Review : "receives"
```

---

## 📁 Project Structure

```
├── app/
│   ├── (auth)/
│   │   ├── sign-in/
│   │   └── sign-up/
│   ├── (dashboard)/
│   │   ├── dashboard/
│   │   └── repositories/
│   ├── api/
│   │   ├── auth/
│   │   ├── inngest/
│   │   └── webhooks/github/
│   └── page.tsx
├── components/
│   └── ui/
├── lib/
│   ├── auth.ts
│   ├── db.ts
│   ├── inngest/
│   └── grok.ts
├── prisma/
│   └── schema.prisma
└── public/
```

---

## 🔄 How It Works

```
GitHub PR Opened
      │
      ▼
GitHub Webhook ──► /api/webhooks/github
      │
      ▼
Inngest Event Triggered
      │
      ▼
Background Job: Fetch PR Diff
      │
      ▼
Grok AI Analysis
      │
      ▼
Post Review Comments to GitHub PR
      │
      ▼
Save Review to Database (Prisma + PostgreSQL)
```

---

## 📄 License

MIT © 2026 CodeReviewAI
