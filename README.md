# DualCal-Scheduler - Bi-Directional Calendar Scheduler

A full-stack real-time appointment scheduling platform engineered with Next.js 15 and TypeScript, featuring bidirectional Google Calendar synchronization. This application implements a dual-portal architecture enabling service providers to expose their real-time calendar availability while clients seamlessly book appointments with automatic event creation across both participants' calendars, complete with Google Meet integration.

The platform leverages OAuth 2.0 authentication flows, RESTful API endpoints, and PostgreSQL-backed data persistence to deliver a production-grade scheduling solution with enterprise-level calendar integration.

<p align="left">
<img src="https://img.shields.io/badge/Next.js-000000?logo=nextdotjs&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black&style=for-the-badge"/>
<img src="https://img.shields.io/badge/Tailwind_CSS-06B6D4?logo=tailwindcss&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/NextAuth.js-000000?logo=nextdotjs&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/Google_Calendar_API-4285F4?logo=googlecalendar&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/Supabase-3FCF8E?logo=supabase&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white&style=for-the-badge"/>
<img src="https://img.shields.io/badge/Vercel-000000?logo=vercel&logoColor=white&style=for-the-badge"/>
</p>

## Core Features

- **Provider Portal**: OAuth 2.0 authenticated dashboard with Google Calendar API integration for real-time availability management
- **Client Interface**: Dynamic seller discovery with slot-based booking system utilizing Server-Side Rendering (SSR)
- **Appointment Management**: Centralized view of scheduled meetings with bidirectional calendar event tracking
- **Calendar Synchronization**: Implements Google Calendar API v3 for fetching availability windows and creating calendar events with embedded Google Meet conference links
- **Data Persistence Layer**: Supabase PostgreSQL database with service role authentication for secure user and appointment record management

## Technology Stack

- **Framework**: Next.js 15 with App Router and React Server Components
- **Language**: TypeScript 5.x with strict type checking
- **Authentication**: NextAuth.js 5.x implementing OAuth 2.0 authorization code flow
- **External APIs**: Google Calendar API v3, Google OAuth 2.0 Provider
- **Database**: Supabase (managed PostgreSQL) with Row Level Security (RLS)
- **Styling**: Tailwind CSS 3.x with JIT compiler
- **Deployment**: Vercel Edge Network with serverless function execution

## Installation & Configuration

### Prerequisites
- Node.js 18.x or higher
- npm or yarn package manager
- Google Cloud Platform account
- Supabase account

### Local Development Setup

1. **Clone Repository**
   ```bash
   git clone <repository-url>
   cd nextjs-scheduler
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Configure Google Cloud Platform**
   - Navigate to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project or select existing
   - Enable Google Calendar API from API Library
   - Configure OAuth 2.0 credentials:
     - Application type: Web application
     - Authorized redirect URIs:
       - Development: `http://localhost:3000/api/auth/callback/google`
       - Production: `https://<your-domain>/api/auth/callback/google`
     - OAuth scopes required:
       - `openid`
       - `email`
       - `profile`
       - `https://www.googleapis.com/auth/calendar.events`
       - `https://www.googleapis.com/auth/calendar.readonly`

4. **Configure Supabase Backend**
   - Create new project at [Supabase Dashboard](https://supabase.com/)
   - Navigate to SQL Editor and execute schema from `db/schema.sql`
   - Retrieve project credentials:
     - API URL
     - Anon/Public key
     - Service role key (with admin privileges)

5. **Environment Configuration**
   
   Create `.env.local` in project root:
   ```env
   # Google OAuth Configuration
   GOOGLE_CLIENT_ID=your_google_client_id
   GOOGLE_CLIENT_SECRET=your_google_client_secret
   
   # NextAuth Configuration
   NEXTAUTH_URL=http://localhost:3000
   NEXTAUTH_SECRET=your_random_secret_key
   
   # Supabase Configuration
   SUPABASE_URL=your_supabase_project_url
   SUPABASE_ANON_KEY=your_anon_public_key
   SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
   ```

6. **Launch Development Server**
   ```bash
   npm run dev
   ```
   Application accessible at `http://localhost:3000`

## Production Deployment (Vercel)

### Deployment Pipeline

1. **Version Control Setup**
   ```bash
   git remote add origin <your-github-repository>
   git push -u origin main
   ```
Application Workflow

### Provider Onboarding
1. Navigate to Provider Portal from landing page
2. Authenticate via Google OAuth 2.0 (calendar permissions required)
3. Access dashboard displaying calendar integration status and availability overview

### Client Booking Flow
1. Access ClArchitecture

PostgreSQL relational database schema implementing normalized table structures:
- **Users Table**: Stores authenticated user profiles with Google OAuth metadata
- **Appointments Table**: Manages booking records with foreign key constraints to users
- **Indexes**: Optimized query performance on frequently accessed columns

Complete schema definition available in `db/schema.sql`

## API Endpoints

RESTful API architecture with Next.js App Router:

| Endpoint | Method | Description | Authentication |
|----------|--------|-------------|----------------|
| `/api/auth/[...nextauth]` | GET/POST | NextAuth.js OAuth handler for Google authentication flows | Public |
| `/api/sellers` | GET | Retrieves list of registered service providers | Required |
| `/api/buyers` | POST | Persists client user data to database | Required |
| `/api/availability/[sellerId]` | GET | Fetches real-time calendar availability for specific provider | Required |
| `/api/availability/editor` | GET | Provider-specific availability management endpoint | Required |
| `/api/book` | POST | Creates bidirectional calendar events and appointment records | Required |
| `/api/appointments` | GET | Retrieves user-specific appointment history | Required |

All authenticated endpoints utilize NextAuth.js session validation with JWT tokens.
   Add all environment variables in Vercel dashboard:
   - `GOOGLE_CLIENT_ID`
   - `GOOGLE_CLIENT_SECRET`
   - `NEXTAUTH_URL` (set to `https://<your-project>.vercel.app`)
   - `NEXTAUTH_SECRET`
   - `SUPABASE_URL`
   - `SUPABASE_ANON_KEY`
   - `SUPABASE_SERVICE_ROLE_KEY`

4. **Update OAuth Configuration**
   - Return to Google Cloud Console
   - Add production redirect URI: `https://<your-project>.vercel.app/api/auth/callback/google`

5. **Deploy Application**
   - Trigger deployment from Vercel dashboard
   - Automatic CI/CD pipeline on subsequent git pushes

## Usage

- Visit the homepage and choose Seller or Buyer
- Sign in with Google (grant calendar permissions)
- Sellers: Dashboard shows integration status
- Buyers: Select seller, choose available slot, book appointment
- Appointments: View all your appointments

## Database Schema

See `db/schema.sql` for the PostgreSQL schema.

## API Routes

- `/api/auth/[...nextauth]` - Authentication
- `/api/sellers` - Get sellers
- `/api/buyers` - Save buyer
- `/api/availability/[sellerId]` - Get seller availability
- `/api/book` - Book appointment
- `/api/appointments` - Get user appointments
