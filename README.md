Here’s a documentation template for running a Next.js project with GraphQL for data fetching:

---

# Project Documentation

## Table of Contents

1. [How to Run the Project Locally](#1-how-to-run-the-project-locally)
   - Prerequisites
   - Environment Setup
   - Running the Project
   - Common Issues

2. [How to Promote a Branch to STG (Staging)](#2-how-to-promote-a-branch-to-stg-staging)
   - Branching Strategy
   - Promotion Steps
   - Verification

3. [How to Promote a Branch to PRD (Production)](#3-how-to-promote-a-branch-to-prd-production)
   - Release Preparation
   - Release Process
   - Post-Release

---

## 1. How to Run the Project Locally

### Prerequisites

Before starting, ensure the following software is installed:

- **npm** (depending on your package manager)

### Environment Setup

1. **Clone the Repository**:
   ```bash
   git clone $ git clone https://github.com/Up-Brasil-Opal/opal-hr-dashboard.git
   cd opal-hr-dashboard
   ```

2. **Install Dependencies**:
   Install all required packages:
   ```bash
   npm install
   ```

3. **Set Up Environment Variables**:
   - Copy the `.env.example` file to `.env.local`:
     ```bash
     cp .env.example .env.local
     ```
   - Fill in the environment variables:

      ### API and WebSocket
      - **`NEXT_PUBLIC_API_ROOT_URL`**: Base URL for API requests.
      - **`NEXT_PUBLIC_SOCKET_URL`**: WebSocket URL for real-time notifications.

      ###  Security and Configuration
      - **`NEXT_PUBLIC_ENCRYPTION_KEY`**: Key for encrypting/decrypting sensitive data.
      - **`NEXT_PUBLIC_BENEFIT_ID`**: ID for a specific benefit configuration.

      ###  Error Tracking and Monitoring
      - **`NEXT_PUBLIC_SENTRY_DSN`**: Sentry URL for error tracking.
      - **`NEXT_PUBLIC_SENTRY_ENV`**: Environment for Sentry (e.g., development, production).

      ###  Datadog Monitoring
      - **`NEXT_PUBLIC_DD_CLIENT_TOKEN`**: Client token for Datadog analytics.
      - **`NEXT_PUBLIC_DD_APPLICATION_KEY`**: Application key for Datadog.
      - **`NEXT_PUBLIC_DD_SERVICE`**: Name of the service monitored by Datadog.
      - **`NEXT_PUBLIC_DD_VERSION`**: Current version of the app for Datadog.
      - **`NEXT_PUBLIC_DD_SITE`**: Datadog site region (e.g., `datadoghq.com`).
      - **`NEXT_PUBLIC_DD_SESSION_SAMPLE_RATE`**: Percentage of sessions to sample (e.g., `100` for all).
      - **`NEXT_PUBLIC_DD_SESSION_REPLAY_SAMPLE_RATE`**: Percentage of sessions to capture for replay (e.g., `20`).
   

### Running the Project

To start the Next.js application:

1. **Development Server**:
   Run the development server:
   ```bash
   npm run dev
   ```
   The app will be available at `http://localhost:3000`.

2. **Build and Start in Production Mode**:
   ```bash
   npm run build
   npm start
   ```
   The app will now be running in production mode at `http://localhost:3000`.

### Common Issues

1. **Environment Variables Not Loaded**:
   - Ensure you have properly created and filled in `.env.local`.
   - Restart the development server after changing environment variables.

2. **GraphQL API Connectivity**:
   - Verify the `NEXT_PUBLIC_API_ROOT_URL` is reachable.
   - Check if you need to authenticate the API by providing a token in headers.



---

## 2. How to Promote a Branch to STG (Staging)

### Branching Strategy

The project uses a well-structured branching strategy:

**feature** > **dev** > **staging** > **main** > **release**

- **feature**: Individual branches for new features or bug fixes.
- **dev**: Consolidates all feature branches after review.
- **staging**: Pre-production environment for thorough testing.
- **main**: Represents the production environment, ready for release.
- **release**: Prepares and manages production releases.

### Promotion Steps

1. **Ensure the Feature Branch is Complete**:
   - Feature should pass tests and review.
   ```bash
   git checkout <feature-branch>
   git commit -m "<feature-branch>:work"
   git push origin <feature-branch>
   ```
   
2. **Merge the Feature Branch into DEV**:
   ```bash
   git checkout dev
   git merge <feature-branch>
   git push origin dev
   ```

3. **Promote to STAGING**:
   After verifying the `dev` branch, merge into `staging`:
   ```bash
   git checkout staging
   git merge dev
   git push origin staging
   ```

### Verification

- **Deployment Check**: Verify the app is live on the staging environment.
- **Testing**: Conduct thorough testing in the staging environment.

---

## 3. How to Promote a Branch to PRD (Production)

### Release Preparation

Before releasing:

1. **Run Tests**:
   Ensure all tests pass before deployment:
   ```bash
   npm run test
   ```

2. **Code Freeze**: Ensure no further changes are made.

### Release Process

1. **Merge to Main**:
   - Merge `staging` into `main` after final verification:
     ```bash
     git checkout main
     git merge staging
     git push origin main
     ```

2. **Deploy to Production**:
   Follow your deployment procedure (e.g., Vercel, Netlify, custom server).

### Post-Release

1. **Validation**: Check production status and monitor performance.
2. **Monitoring**: Use tools like Sentry, LogRocket, or Datadog to track errors.
3. **Rollback**: If necessary, revert to a stable release.

---

## Error Monitoring with Sentry
Ensure you’ve correctly set the Sentry DSN for error logging:

In .env.production, add:
```bash
SENTRY_DSN=<your_sentry_dsn>
```
This configuration ensures Sentry is capturing errors in the production environment for proactive monitoring.