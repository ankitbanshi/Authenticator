
# Role-Based Authentication

A full-stack web application with role-based authentication (User/Admin) built with **Next.js**, **Express**, **MongoDB**, and **JWT**.

---

## 🚀 Features

- **Role-Based Authentication:** Sign up and login with `User` or `Admin` roles
- **Secure Password Storage:** Passwords hashed using `bcrypt`
- **JWT Authentication:** Token-based authentication for secure API access
- **Protected Routes:** Dashboard accessible only to authenticated users
- **CRUD Operations:** Create, read, update, and delete user resources
- **Modern UI:** Responsive design with TailwindCSS
- **TypeScript:** Full type safety across frontend and backend

---

## 🌐 Live Deployment

### 🔸 Frontend (Next.js)
**Live URL:** [https://your-frontend-url.vercel.app](https://authx-teal.vercel.app/login)

### 🔸 Backend (Express API)
**API Base URL:** [https://your-backend-url.vercel.app](https://authentication-assignment-backend.vercel.app/)

---

## 📋 Tech Stack

**Backend**
- Node.js + Express
- MongoDB Atlas + Mongoose ODM
- JWT for authentication
- bcryptjs for password hashing

**Frontend**
- Next.js 14 with TypeScript
- TailwindCSS
- React Hook Form + Zod validation
- Axios

---

## 📁 Project Structure

```
MiniProject/
├── backend/
│   ├── models/
│   │   └── User.js
│   ├── routes/
│   │   └── auth.js
│   ├── middleware/
│   │   └── auth.js
│   ├── server.js
│   ├── package.json
│   └── .env.example
├── frontend/
│   ├── app/
│   │   ├── login/
│   │   ├── signup/
│   │   ├── dashboard/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── globals.css
│   ├── components/
│   │   └── ProtectedRoute.tsx
│   ├── lib/
│   │   ├── api.ts
│   │   └── auth.ts
│   ├── package.json
│   └── .env.example
└── README.md
```

---

## 🛠️ Setup Instructions

### Prerequisites
- Node.js v18+
- MongoDB Atlas account (free tier works)
- npm or yarn

### Backend Setup

```bash
cd backend
npm install
cp .env.example .env
```

Update `.env`:
```env
MONGODB_URI=your_mongodb_atlas_connection_string
JWT_SECRET=your_super_secret_jwt_key
PORT=5000
```

```bash
npm run dev
# Runs on http://localhost:5000
```

### Frontend Setup

```bash
cd frontend
npm install
cp .env.example .env.local
```

Update `.env.local`:
```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```

```bash
npm run dev
# Runs on http://localhost:3000
```

---

## 🔐 API Documentation

> **Base URL:** `https://your-backend-url.vercel.app`  
> **Auth header format:** `Authorization: Bearer <token>`  
> A full Postman collection is available in [`/backend/postman_collection.json`](./backend/postman_collection.json)

---

### Auth Endpoints

#### `POST /auth/signup`
Create a new user account.

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "role": "User"
}
```

**Response `201`:**
```json
{
  "message": "User created successfully",
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "role": "User"
  }
}
```

**Error `400`:** Email already in use  
**Error `422`:** Validation failed (password < 6 chars, missing fields)

---

#### `POST /auth/login`
Login and receive a JWT token.

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

**Response `200`:**
```json
{
  "message": "Login successful",
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "role": "User"
  }
}
```

**Error `401`:** Invalid credentials

---

#### `GET /auth/me` 🔒
Get the currently authenticated user's profile.

**Headers:** `Authorization: Bearer <token>`

**Response `200`:**
```json
{
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "role": "User"
  }
}
```

**Error `401`:** Missing or invalid token

---

### User CRUD Endpoints (Admin only)

> All routes below require `Authorization: Bearer <token>` with an **Admin** role.

#### `GET /users`
Returns a list of all registered users.

**Response `200`:**
```json
[
  { "id": "1", "name": "John Doe", "email": "john@example.com", "role": "User" },
  { "id": "2", "name": "Jane Smith", "email": "jane@example.com", "role": "Admin" }
]
```

---

#### `GET /users/:id`
Get a single user by ID.

**Response `200`:**
```json
{ "id": "1", "name": "John Doe", "email": "john@example.com", "role": "User" }
```

**Error `404`:** User not found

---

#### `PUT /users/:id`
Update a user's name or role.

**Request Body:**
```json
{
  "name": "John Updated",
  "role": "Admin"
}
```

**Response `200`:**
```json
{ "message": "User updated", "user": { "id": "1", "name": "John Updated", "role": "Admin" } }
```

---

#### `DELETE /users/:id`
Delete a user account.

**Response `200`:**
```json
{ "message": "User deleted successfully" }
```

**Error `404`:** User not found

---

## 🚢 Deployment

### Vercel (Frontend + Backend)

**Backend:**
1. Create a new Vercel project → set Root Directory to `backend`
2. Add environment variables:
   - `MONGODB_URI` — your MongoDB Atlas connection string
   - `JWT_SECRET` — a secure random string
3. Deploy and copy the backend URL

**Frontend:**
1. Create another Vercel project → set Root Directory to `frontend`
2. Add environment variable:
   - `NEXT_PUBLIC_API_URL` — your deployed backend URL
3. Deploy

### MongoDB Atlas Setup
1. Create a free account at [mongodb.com/atlas](https://www.mongodb.com/atlas)
2. Create a cluster and a database user
3. Whitelist `0.0.0.0/0` (all IPs) for development
4. Copy the connection string into your `.env`

---

## 📬 Postman Collection

Import the collection from `/backend/postman_collection.json` to test all endpoints locally or against the live API. It includes pre-configured environment variables for `base_url` and `auth_token`.

Alternatively, Swagger/OpenAPI docs are auto-generated and accessible at:
```
http://localhost:5000/api-docs
```

---

## 📈 Scalability Notes

This project is designed as a monolith suitable for a mini-project, but can be scaled using the following strategies:

### Microservices
As the application grows, the auth service and user management service can be extracted into independent microservices, each with its own database. This allows teams to deploy, scale, and update services independently without affecting the whole system.

### Caching
A Redis layer can be introduced to cache JWT validation results and frequently-accessed user records (e.g., `/auth/me`), reducing redundant database reads and improving response times under load.

### Load Balancing
Deploying multiple instances of the Express backend behind a load balancer (e.g., AWS ALB, NGINX) distributes traffic evenly and eliminates single points of failure. Combined with horizontal auto-scaling (e.g., AWS ECS or Kubernetes), the API can handle traffic spikes gracefully.

### Database Scaling
MongoDB Atlas supports automatic sharding and replica sets. Read-heavy workloads can be offloaded to secondary replicas, and sharding can be enabled to distribute data across multiple nodes as user volume grows.

### Rate Limiting & Security at Scale
At scale, API gateway-level rate limiting (e.g., AWS API Gateway, Kong) can protect against abuse, while a CDN (e.g., Cloudflare) can handle static frontend assets and edge caching globally.

---

## 🔒 Security Features

- Passwords hashed with bcrypt (salt rounds: 10)
- JWT tokens expire after 7 days
- Protected API routes with authentication middleware
- CORS configured for allowed origins only
- Input validation on both frontend (Zod) and backend (Express middleware)
- Role-based access control (RBAC) for admin-only routes

---

## 📝 Notes

- Passwords must be at least 6 characters
- JWT tokens expire after 7 days
- Dashboard content varies by user role
- All routes except `/auth/login` and `/auth/signup` require authentication

---

## 👤 Author

Created as a full-stack mini project assignment.

