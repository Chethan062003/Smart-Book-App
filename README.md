<<<<<<< HEAD
This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) to automatically optimize and load [Geist](https://vercel.com/font), a new font family for Vercel.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

# 🔖 Smart Bookmark App

A full-stack bookmark manager built using **Next.js (App Router)**, **Supabase**, and **Tailwind CSS**.  
Users can log in with Google, add bookmarks, delete them, and see real-time updates across tabs.

---

## 🚀 Live Demo

🔗 https://smart-book-app-3m73.vercel.app/

---

## 🛠️ Tech Stack

- **Frontend:** Next.js (App Router), TypeScript
- **Styling:** Tailwind CSS
- **Backend & Database:** Supabase (PostgreSQL)
- **Authentication:** Google OAuth (Supabase Auth)
- **Security:** Row Level Security (RLS)
- **Deployment:** Vercel

---

## ✨ Features

- 🔐 Google OAuth Login
- ➕ Add bookmarks (title + URL)
- ❌ Delete bookmarks
- 👤 Private bookmarks per user
- 🔄 Real-time updates across tabs (Supabase Realtime)
- 🌐 Deployed to production (Vercel)

---

## 🔒 Database Security

Row Level Security (RLS) is enabled on the `bookmarks` table.

Policies ensure:

- Users can only view their own bookmarks
- Users can only insert their own bookmarks
- Users can only delete their own bookmarks

This ensures complete data isolation between users.

---

## ⚡ Real-Time Implementation

Real-time updates are implemented using Supabase `postgres_changes` subscription:

```ts
.channel("bookmarks-changes")
.on("postgres_changes", ...)
.subscribe()


 Problems Faced & How I Solved Them
1️ OAuth Redirect Issues (Production)

Problem:
After deploying to Vercel, Google login was stuck or redirecting incorrectly.

Solution:
Updated Supabase → Authentication → URL Configuration:

Set correct Site URL

Added correct Redirect URL

Ensured redirect used:

redirectTo: `${window.location.origin}/dashboard`

2️ Environment Variables Not Working in Production

Problem:
Supabase URL and key were undefined in production.

Solution:
Added environment variables in Vercel:

NEXT_PUBLIC_SUPABASE_URL

NEXT_PUBLIC_SUPABASE_ANON_KEY
Then redeployed the project.

3️ Row Level Security Blocking Queries

Problem:
After enabling RLS, inserts/selects failed.

Solution:
Created proper RLS policies:

SELECT policy

INSERT policy

DELETE policy

Each policy checks:

auth.uid() = user_id

4️ Real-Time Not Working in Production

Problem:
Realtime worked locally but not consistently in Vercel.

Solution:

Ensured bookmarks table was added to supabase_realtime publication

Used filtered subscription:

filter: `user_id=eq.${currentUser.id}`


Ensured session was loaded before subscribing
