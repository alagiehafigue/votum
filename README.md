# Votum

A full-stack platform for sharing, browsing and refining startup ideas and side hustles. Visitors can read ideas freely, and registered members can post and manage their own.

This repository is the **React frontend**. The REST API lives in [votum-api](https://github.com/alagiehafigue/votum-api).

> The app is branded **IdeaDrop** in the interface.

## Features

- Browse all ideas, or see the three latest on the home page
- Read a full idea with its title, summary, description and tags
- Create, edit and delete ideas through a REST API protected by JWT
- Register, log in and log out
- Stay signed in across page reloads using a silent token refresh
- Responsive layout built with Tailwind CSS
- Friendly 404 page and loading handled by route loaders

## Tech Stack

| Area | Tools |
| --- | --- |
| UI | React 19, TypeScript |
| Routing | TanStack Router (file-based routes, auto code splitting) |
| Data fetching | TanStack Query, Axios |
| Styling | Tailwind CSS 4, Lucide icons |
| Build and test | Vite 7, Vitest, Testing Library |
| Backend | Express 5, MongoDB with Mongoose, jose (JWT), bcryptjs |

## How Authentication Works

1. A user registers or logs in and the API returns a short-lived **access token** (JWT) in the response body.
2. The API also sets a **refresh token** (30 days) in an `httpOnly` cookie, so JavaScript can never read it.
3. On every page load the app calls `POST /api/auth/refresh`. If the cookie is valid, the user is signed in again without typing a password.
4. Passwords are hashed with bcrypt before they are stored.

## Project Structure

```
src/
  api/          Axios calls for auth and ideas
  componets/    Shared components (Header, IdeaCard)
  context/      AuthContext (access token and current user)
  lib/          Axios instance (baseURL /api, credentials enabled)
  routes/       File-based routes
    (auth)/     login and register
    ideas/      list, new, $ideaId detail and edit
  types.ts      Shared TypeScript types
```

## Getting Started

### Prerequisites

- Node.js 18 or newer
- A MongoDB database (local or MongoDB Atlas)

### 1. Start the API

```bash
git clone https://github.com/alagiehafigue/votum-api.git
cd votum-api
npm install
```

Create a `.env` file:

```env
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=a_long_random_secret
PORT=8000
```

Then run it:

```bash
npm run dev
```

### 2. Start the frontend

```bash
git clone https://github.com/alagiehafigue/votum.git
cd votum
npm install
npm run dev
```

Open http://localhost:3000. Vite forwards every `/api` request to the API on port 8000, so no extra CORS setup is needed in development.

## Scripts

| Command | What it does |
| --- | --- |
| `npm run dev` | Start the dev server on port 3000 |
| `npm run build` | Create a production build and type-check |
| `npm run serve` | Preview the production build |
| `npm run test` | Run tests with Vitest |
| `npm run json-server` | Run a mock API from `src/data/db.json` on port 8000 |

## API Endpoints

| Method | Route | Access | Description |
| --- | --- | --- | --- |
| POST | `/api/auth/register` | Public | Create an account |
| POST | `/api/auth/login` | Public | Log in |
| POST | `/api/auth/logout` | Public | Clear the refresh cookie |
| POST | `/api/auth/refresh` | Cookie | Get a new access token |
| GET | `/api/ideas` | Public | List ideas, newest first (`?_limit=3` to limit) |
| GET | `/api/ideas/:id` | Public | Get one idea |
| POST | `/api/ideas` | Protected | Create an idea |
| PUT | `/api/ideas/:id` | Protected | Update an idea |
| DELETE | `/api/ideas/:id` | Protected | Delete an idea |

An idea has a `title`, `summary`, `description` and a list of `tags`, plus automatic `createdAt` and `updatedAt` timestamps.

## Roadmap

- [ ] Send the access token with every request from the frontend
- [ ] Link each idea to its author and allow only the owner to edit or delete it
- [ ] Add voting on ideas
- [ ] Add search and tag filtering
- [ ] Add component tests with Vitest and Testing Library
- [ ] Deploy the API and frontend

## Author

**Alagie Hafigue Bah**
Computer Science student at KNUST
GitHub: [@alagiehafigue](https://github.com/alagiehafigue)
