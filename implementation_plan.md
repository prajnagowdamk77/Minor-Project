# Print Management System Implementation Plan

This plan details the implementation of an Automated Stationery Pre-Booking & Print Management System using Next.js and Supabase. The system allows students to upload print jobs, get a unique reference ID, and provides a real-time admin dashboard for the shopkeeper to manage the queue.

## User Review Required

> [!IMPORTANT]
> **Styling**: Vanilla CSS (CSS Modules) will be used per the system directives, avoiding TailwindCSS. I will focus on a highly aesthetic, modern, and dynamic design with glassmorphism and subtle animations.
> **Supabase Project setup**: You will need to create a new Supabase project at supabase.com and provide the `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` to complete the setup. I will provide a Setup Guide for the DB schema and storage.

## Open Questions

> [!WARNING]
> 1. Do you already have a Supabase project set up, or should I proceed with generating the code and a SQL initialization script that you can run in your Supabase SQL Editor once you create one?
> 2. Are there any specific brand colors or themes you want for the college stationery shop, or should I use a generic premium dark/light mode aesthetic?

## Proposed Changes

### Setup & Infrastructure
- Initialize Next.js project with `npx create-next-app` (App Router, vanilla CSS) in the `c:\Users\HP\OneDrive\Desktop\AI` directory.
- Install `@supabase/supabase-js` and `@supabase/ssr` for authentication and database interaction.

### Database Architecture & Logic (Supabase)
#### [NEW] `supabase/schema.sql`
- SQL script to initialize tables (`orders`, `reference_pool`).
- RLS Policies for secure access (public insert for orders, authenticated read/update for admin).
- PL/pgSQL function to safely assign a unique 3-digit `reference_id` avoiding concurrent collisions using `SELECT ... FOR UPDATE SKIP LOCKED`.
- Script to populate `reference_pool` with IDs 000-999.

### Frontend Features
#### [NEW] Global Styles
- Define CSS custom properties for colors, typography, glassmorphism, and micro-animations in `globals.css` and use CSS Modules for components.

#### [NEW] Public Routes (`/`, `/student`)
- Landing page with a stunning visual intro and clear call to action.
- Booking form that handles file upload (PDF/DOCX), gathers print details, and calls the Supabase assignment function.
- Prominent display of the assigned Reference ID upon success.

#### [NEW] Authentication (`/login`)
- Admin login page using Supabase Auth (Email/Password).

#### [NEW] Admin Dashboard (`/admin`)
- Protected route accessible only to authenticated users.
- Live-updating queue using Supabase Realtime subscriptions.
- Data table displaying active orders, implementing FIFO sorting and Reference ID.
- "Click to reveal name" privacy feature (hides student names by default).
- Action buttons to Mark as Done and Toggle Payment Status.

### Background Jobs
#### [NEW] `supabase/cleanup.sql`
- A `pg_cron` script to run at 11 PM daily to remove completed orders, reset `reference_pool`, and clear storage objects.

## Verification Plan

### Manual Verification
- Run the local Next.js development server to verify the UI.
- Test the UI aesthetics, responsive layouts, animations, and forms.
- Verify the routing (student vs admin).
- Provide the SQL scripts and setup guide so you can initialize the Supabase backend and connect the application.
