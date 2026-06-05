# 🛒 Shop4Ever Backend — Express.js REST API

**A production-grade Node.js backend with role-based access control, JWT authentication, OTP verification, and AI integration**

## ✨ Core Features

### 🔐 Authentication & Security
- **Multi-role login** — Owner, Employee, Consumer with JWT sessions
- **OTP verification** — Email-based OTP via Nodemailer for signup, login, password reset
- **Password hashing** — bcrypt with configurable salt rounds
- **Role-based middleware** — Fine-grained access control on every endpoint
- **Forgot password flow** — 3-step email recovery with OTP verification
- **Session management** — Token refresh and auth session tracking

### 👤 Consumer Portal API
- **Product browsing** — Full product catalog with pagination
- **Search & filters** — Category, price range, and text search
- **Semantic similarity** — Find similar products using embedding vectors
- **Shopping cart** — Add, update, remove items with quantity management
- **Order placement** — Checkout with order tracking
- **Order history** — View past orders and status
- **Ratings & reviews** — Operational rating system on products
- **Profile management** — Personal info and preferences

### 👷 Employee Portal API
- **Order management** — View and update order status
- **Product operations** — Add, update, delete products with image uploads
- **Team visibility** — View team members and hierarchy
- **Profile management** — Update info and profile photos

### 🏢 Owner (Admin) Portal API
- **Employee management** — Full CRUD: hire, fire, update salary, assign managers
- **Business analytics** — 8 real-time KPI metrics
- **Product oversight** — Manage inventory and discounts
- **Discount management** — Create and update product discounts
- **Order monitoring** — View all orders across the platform

### 🤖 AI Chatbot Integration
- **RAG pipeline** — Multi-level retrieval for intelligent responses
- **Vector database** — Integration with ChromaDB for embedding search
- **LLM proxy** — Connect to Groq API (Llama 3.1 8B)
- **Product indexing** — Semantic product data in vectors
- **Contextual responses** — Relevant product data before LLM generation

## 🏗 Tech Stack

| Component | Technology |
|-----------|------------|
| **Runtime** | Node.js v18+ |
| **Framework** | Express.js 5.x (ES Modules) |
| **Database** | MySQL on Aiven Cloud (SSL encrypted) |
| **Authentication** | JWT, bcrypt, OTP |
| **File Upload** | Multer + ImageKit CDN |
| **Email** | Nodemailer (Gmail SMTP) |
| **Image Processing** | ImageKit API |
| **Database Connection** | mysql2 with connection pooling |
| **Environment** | dotenv for config management |
| **Development** | nodemon for hot reload |

## 🗄️ Database Schema

**Normalized MySQL schema on Aiven Cloud with SSL:**

```
Consumers ──┬── Cart_Items ──┐
            │                ├─→ Product ──→ Discount
            └─→ Orders ──────┘

Employee ──┬── Manager (self-reference)
           └── Team Members

Discount ──→ Product
```

**Key Tables:**
- `Consumers` — User accounts, contact info
- `Product` — Catalog, pricing, images, stock
- `Orders` — Order headers with status tracking
- `Cart_Items` — Shopping cart items
- `Employee` — Staff with roles and salary
- `Discount` — Product discounts with descriptions

## 📡 API Endpoints

### Authentication (`/api/auth`)
```
POST   /signup                    Register consumer
POST   /login                     Login (all roles)
POST   /send-login-otp           Send OTP for login
POST   /verify-login-otp         Verify and get token
POST   /send-signup-otp          Send OTP for signup
POST   /verify-signup-otp        Create account with OTP
POST   /forgot-password          Send reset OTP
POST   /verify-otp               Verify reset token
POST   /reset-password           Set new password
```

### Products (`/api/product`)
```
GET    /products                 Get all products
GET    /products/:id             Get product by ID
POST   /add                      Add product (Auth: Employee, Admin)
POST   /update/:id               Update product
POST   /delete/:id               Delete product
POST   /similar/:id              Get similar products (semantic)
```

### Cart (`/api/cart`) — *Consumer only*
```
POST   /add                      Add to cart
GET    /get                      Get cart items
POST   /update                   Update quantity
POST   /remove/:cart_id          Remove item
POST   /clear                    Clear cart
POST   /checkout                 Place order
```

### Orders (`/api/order`) — *Consumer only*
```
GET    /orders                   Get consumer's orders
GET    /orders/:order_id         Get order details
```

### Employee (`/api/employee`) — *Employee, Owner*
```
GET    /orders                   Get all orders
POST   /status/:order_id         Update order status
GET    /profile                  Get employee profile
POST   /update-profile           Update profile with photo
GET    /team-member              Get team members
```

### Admin/Owner (`/api/admin`) — *Owner only*
```
GET    /employees                Get all employees
POST   /add                      Add employee
POST   /delete/:employee_id      Delete employee
POST   /update/:employee_id      Update employee
POST   /update-discount/:product_id  Create/update discount
GET    /dashboard                Get dashboard stats
```

## 🚀 Quick Start

### Prerequisites
- Node.js v18+
- MySQL database (or Aiven Cloud account)
- ImageKit account
- Gmail account with App Password
- Groq API key (for AI chatbot)

### Installation

```bash
cd backend
npm install
```

### Configuration

Create `.env`:

```env
# Database (MySQL / Aiven Cloud)
DB_HOST=your-db-host.aivencloud.com
DB_USER=avnadmin
DB_PASSWORD=your-db-password
DB_NAME=defaultdb
DB_PORT=15127

# JWT
JWT_SECRET=your-secret-key

# Server
PORT=3000

# ImageKit (product images)
IMAGEKIT_PUBLIC_KEY=your-public-key
IMAGEKIT_PRIVATE_KEY=your-private-key
IMAGEKIT_URL_ENDPOINT=https://ik.imagekit.io/your-id

# Email (Gmail App Password)
EMAIL_SERVICE=gmail
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
COMPANY_EMAIL=your-email@gmail.com
COMPANY_EMAIL_APP_PASSWORD=your-app-password

# Environment
NODE_ENV=development
```

### Database Setup

**For Aiven MySQL:**
1. Download CA certificate → `backend/certs/ca.pem`
2. See [Aiven SSL docs](https://aiven.io/docs)

**Seed admin account (first-time only):**

```bash
node createAdmin.js
```

### Start Server

```bash
npm run server    # Development (nodemon)
npm start         # Production
```

Runs at `http://localhost:3000`

## 📁 Project Structure

```
backend/
├── config/
│   ├── db.js                    # MySQL pool with SSL
│   └── imageKit.js              # ImageKit CDN config
├── controllers/
│   ├── authController.js        # Auth logic
│   ├── adminController.js       # Owner operations
│   ├── cartController.js        # Cart & checkout
│   ├── employeeController.js    # Employee operations
│   ├── orderController.js       # Order retrieval
│   ├── productController.js     # Product CRUD
│   └── userController.js        # Consumer profile
├── middlewares/
│   ├── authRole.js              # JWT + role verification
│   └── multer.js                # File upload
├── models/
│   ├── admin.js                 # Admin model
│   ├── employee.js              # Employee model
│   └── user.js                  # Consumer model
├── routes/
│   ├── authRoutes.js            # /api/auth/*
│   ├── adminRoutes.js           # /api/admin/*
│   ├── cartRoutes.js            # /api/cart/*
│   ├── employeeRoutes.js        # /api/employee/*
│   ├── orderRoutes.js           # /api/order/*
│   └── productRoutes.js         # /api/product/*
├── utils/
│   ├── emailService.js          # OTP email templates
│   ├── generateToken.js         # JWT generation
│   ├── genHash.js               # Password hashing
│   ├── otpStore.js              # OTP management
│   └── authSessionStore.js      # Session tracking
├── certs/
│   └── ca.pem                   # Aiven MySQL CA cert
├── server.js                    # Express app entry
├── createAdmin.js               # Admin seeding script
└── .env                         # Environment variables
```

## 🔐 Security Features

- **SSL/TLS** — MySQL connection encrypted with CA certificates
- **Password hashing** — bcrypt with salt rounds
- **JWT tokens** — Signed authentication tokens with expiry
- **Role-based access** — Middleware-enforced per endpoint
- **OTP verification** — Time-limited one-time passwords
- **Input validation** — Request sanitization
- **CORS protection** — Configurable cross-origin policies

## 📊 OTP System

**Development mode (`NODE_ENV=development`):**
- OTPs logged to console for testing
- Email still sent for verification

**Production mode:**
- OTPs only sent via email
- 5-minute expiry
- Single-use only

## 🤖 AI Chatbot Integration

**Chatbot API (`http://localhost:8000`):**
```
GET    /              Health check
POST   /chat          Send query, get RAG response
```

**RAG Pipeline:**
1. **Multi-level retrieval** — Graph, agentic, embedding-based
2. **Vector search** — ChromaDB with HuggingFace embeddings
3. **LLM generation** — Groq Llama 3.1 8B
4. **Context injection** — Top-k products included in prompt

## 📧 Email Configuration

**Gmail SMTP Setup:**
1. Enable 2-Step Verification on Google Account
2. Generate App Password at [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
3. Add 16-character App Password to `.env`

**Email Templates:**
- Welcome/signup confirmation
- OTP verification
- Password reset
- Order notifications

## 🧪 Testing

**API Testing with cURL:**

```bash
# Login
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@test.com","password":"pass","role":"consumer"}'

# Get products
curl http://localhost:3000/api/product/products

# Add to cart (requires auth)
curl -X POST http://localhost:3000/api/cart/add \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"product_id":1,"quantity":2}'
```

## 🚀 Deployment

**Recommended:**
- **Backend** — Render, Railway, or Heroku
- **Database** — Aiven Cloud MySQL (recommended) or AWS RDS
- **Images** — ImageKit CDN (already integrated)

---

**Built as part of the Shop4Ever DBIS Course Project** ❤️
