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

- **Node.js** (LTS version recommended)
- **npm** or **yarn** (depending on your package manager)
- **Docker** (optional for local PostgreSQL/Redis setup)
- **GraphQL Client**: For querying and testing APIs.

### Environment Setup

1. **Clone the Repository**:
   ```bash
   git clone <repository-url>.git
   cd nextjs-graphql-project
   ```

2. **Install Dependencies**:
   Install all required packages:
   ```bash
   npm install
   # Or if you are using yarn:
   yarn install
   ```

3. **Set Up Environment Variables**:
   - Copy the `.env.example` file to `.env.local`:
     ```bash
     cp .env.example .env.local
     ```
   - Fill in the environment variables:
     - `NEXT_PUBLIC_GRAPHQL_API_URL`: GraphQL API endpoint.
     - `DATABASE_URL`: URL for your PostgreSQL or preferred database.
     - `NEXT_PUBLIC_BASE_URL`: Base URL for the frontend (optional).
   
4. **Database Setup** (Optional):
   If you're using Docker for the database setup, run:
   ```bash
   docker-compose up -d
   ```

   Alternatively, ensure your local PostgreSQL instance is running and the `DATABASE_URL` in your `.env.local` file is configured correctly.

### Running the Project

To start the Next.js application:

1. **Development Server**:
   Run the development server:
   ```bash
   npm run dev
   # Or with yarn:
   yarn dev
   ```
   The app will be available at `http://localhost:3000`.

2. **GraphQL Queries**:
   Test the GraphQL API with your preferred tool (e.g., GraphiQL, Postman, or Insomnia) using the `NEXT_PUBLIC_GRAPHQL_API_URL` endpoint.

3. **Build and Start in Production Mode**:
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
   - Verify the `NEXT_PUBLIC_GRAPHQL_API_URL` is reachable.
   - Check if you need to authenticate the API by providing a token in headers.

3. **Database Connection Issues**:
   - Ensure the database is running locally or through Docker.
   - Check the `DATABASE_URL` in `.env.local`.

4. **Docker Issues**:
   - Ensure Docker is installed and running properly.
   - Verify Docker Compose configuration for PostgreSQL if using the containerized setup.

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