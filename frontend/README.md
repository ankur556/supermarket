# 🛒 Shop4Ever Frontend — React + Vite

**A modern, responsive React SPA with AI-powered shopping assistant and semantic product search**

## ✨ Features

### 🛍️ Consumer Experience
- **Product browsing** — Responsive grid with search, category/price filtering
- **Dynamic product images** — Auto-fetched from Unsplash API with ImageKit fallback
- **Semantic product search** — Find similar products by embedding similarity (AI-powered)
- **Product details & Reviews** — Dedicated page with description, pricing, and **operational rating/review system**
- **Shopping cart** — Real-time quantity management with checkout flow
- **Order tracking** — Monitor status: Pending → Shipped → Delivered
- **Automatic discounts** — Real-time calculation with strikethrough pricing

### 🤖 AI Shopping Assistant (RAG-Powered)
- **Multi-level retrieval strategy** — Intelligent responses using:
  - Graph-based retrieval (product relationships)
  - Agentic reasoning (intelligent context)
  - Embedding search (semantic similarity)
- **Vector database** — ChromaDB with HuggingFace embeddings
- **LLM integration** — Groq API running Llama 3.1 8B
- **Floating UI widget** — Glassmorphism design with typing indicators
- **Natural language queries** — Ask about products, prices, availability in plain English

### 🔐 Multi-Role Authentication
- **Three user roles** — Owner, Employee, Consumer
- **OTP-based verification** — Email validation via Nodemailer
- **Password reset flow** — 3-step secure recovery
- **JWT sessions** — Role-based route protection
- **Glassmorphism login** — Modern animated UI

### 👷 Employee Features
- **Order management** — Update status across the platform
- **Product management** — Add/update/delete with image uploads
- **Team visibility** — See team members and hierarchy
- **Profile management** — Update personal info and photos

### 🏢 Owner (Admin) Dashboard
- **8 Real-time KPIs** — Revenue, orders, stock, employees, customers
- **Employee management** — Full CRUD with salary management
- **Product oversight** — Manage discounts and inventory
- **Order monitoring** — Track all orders across the platform

## 🏗 Tech Stack

| Category | Technology |
|----------|------------|
| **Framework** | React 19, Vite 7 |
| **Styling** | TailwindCSS 4, Glassmorphism, Dark Theme |
| **Animations** | Framer Motion |
| **UI Components** | React Icons, Lucide React |
| **State Management** | Context API |
| **Notifications** | React Toastify |
| **Image Optimization** | ImageKit CDN, Unsplash API |
| **Module System** | ES Modules |

## 🚀 Quick Start

### Prerequisites
- Node.js v18+
- npm or yarn

### Installation

```bash
cd frontend
npm install
```

### Configuration

Create `.env`:

```env
VITE_BACKEND_URL=http://localhost:3000
VITE_UNSPLASH_API_KEY=your-unsplash-api-key
```

### Development

```bash
npm run dev
```

Runs at `http://localhost:5173`

### Production Build

```bash
npm run build
npm run preview
```

## 📁 Project Structure

```
src/
├── components/
│   ├── FloatingChatbot.jsx      # AI chatbot widget (RAG-powered)
│   ├── Navbar.jsx               # Navigation
│   ├── OrderCard.jsx            # Order card UI
│   └── ProtectedRoute.jsx       # Auth guard
├── context/
│   └── AuthContext.jsx          # Global auth + cart state
├── layouts/
│   ├── ConsumerLayout.jsx       # Consumer shell
│   ├── EmployeeLayout.jsx       # Employee shell
│   └── OwnerLayout.jsx          # Owner shell
├── pages/
│   ├── Home.jsx                 # Landing page
│   ├── Login.jsx                # Multi-role auth
│   ├── Features.jsx             # Feature showcase
│   ├── consumer/                # Dashboard, cart, checkout, product details
│   ├── employee/                # Management pages
│   ├── owner/                   # Admin dashboard & operations
│   └── chatbot/                 # RAG backend (Python) + UI
├── App.jsx                      # Routes
└── main.jsx                     # Entry point
```

## 🎨 Design System

- **Dark-first** — `#121212` base with `#F5F5F5` text
- **Orange accent** — `#FF8C00` primary, `#FF4B91` & `#8A2BE2` gradients
- **Glassmorphism** — Backdrop blur effects
- **Responsive** — Mobile-first with `sm`, `md`, `lg`, `xl` breakpoints
- **Typography** — Google Fonts (Inter)

## 🔗 Integration

- **Backend** — REST API at `http://localhost:3000`
- **AI Chatbot** — FastAPI server at `http://localhost:8000` (multi-level RAG)
- **External APIs** — Unsplash (images), ImageKit (CDN)

## 🎯 Key Components

### FloatingChatbot.jsx
- RAG-powered AI assistant with multi-level retrieval
- Natural language queries for products
- Embedding-based semantic search
- Contextual product recommendations

### ProtectedRoute.jsx
- Role-based route protection
- JWT token validation
- Redirect to login on auth failure

### AuthContext.jsx
- Global authentication state
- Shopping cart management
- User role management
- Token persistence

## 📚 See Also

- [Main README](../readme.md) — Project overview
- [Backend README](../backend/README.md) — API reference and server setup

---

**Built as part of the Shop4Ever DBIS Course Project** ❤️
