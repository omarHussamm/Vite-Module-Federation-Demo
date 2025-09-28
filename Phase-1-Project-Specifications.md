# Phase 1: Individual Remote Applications - Detailed Specifications

## Project Overview

Create three standalone Vite + React + TypeScript applications that will later be converted into micro frontends. Each application should be fully functional as a standalone app with simple, clean UI and realistic business logic.

---

## Global Specifications (All Three Projects)

### Tech Stack
- **Build Tool**: Vite
- **Framework**: React 19
- **Language**: TypeScript
- **Package Manager**: pnpm
- **Routing**: React Router DOM
- **Styling**: Simple CSS (inline styles for simplicity)

### Initial Setup Commands (Run These First)

```bash
# 1. Create Vite project
pnpm create vite . --template react-ts
pnpm install

# 2. Upgrade to React 19 
pnpm add react@rc react-dom@rc
pnpm add -D @types/react @types/react-dom

# 3. Install routing
pnpm add react-router-dom

# That's it! Keep it simple - focus on micro frontend concepts.
```

### Required Configuration Files

#### `vite.config.ts`
```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
  server: {
    port: 5001 // Change to 5002 for orders-app, 5003 for users-app
  }
})
```

#### CSS Organization (Separate Files)

**`src/index.css` (Global reset and base styles):**
```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: system-ui, -apple-system, sans-serif;
  line-height: 1.6;
  background-color: #f5f5f5;
  color: #333;
}

a {
  color: #007bff;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

button {
  font-family: inherit;
  cursor: pointer;
}

input, select, textarea {
  font-family: inherit;
  font-size: 14px;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

input:focus, select:focus, textarea:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.25);
}
```

**`src/styles/components.css` (Reusable component styles):**
```css
/* Card Component */
.card {
  background: white;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* Button Components */
.btn {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  background-color: #007bff;
  color: white;
  font-size: 14px;
  text-decoration: none;
  display: inline-block;
  transition: background-color 0.2s;
}

.btn:hover {
  background-color: #0056b3;
}

.btn-secondary {
  background-color: #6c757d;
}

.btn-secondary:hover {
  background-color: #545b62;
}

.btn-outline {
  background-color: transparent;
  border: 1px solid #007bff;
  color: #007bff;
}

.btn-outline:hover {
  background-color: #007bff;
  color: white;
}

/* Badge Component */
.badge {
  display: inline-block;
  padding: 4px 8px;
  font-size: 12px;
  font-weight: 500;
  border-radius: 12px;
  text-transform: uppercase;
}

.badge-primary { background-color: #007bff; color: white; }
.badge-secondary { background-color: #6c757d; color: white; }
.badge-success { background-color: #28a745; color: white; }
.badge-danger { background-color: #dc3545; color: white; }
.badge-outline { background-color: #f8f9fa; color: #495057; border: 1px solid #ddd; }
```

**`src/styles/layout.css` (Layout-specific styles):**
```css
/* Sidebar Layout */
.sidebar-layout {
  display: flex;
  min-height: 100vh;
}

.sidebar {
  width: 250px;
  border-right: 1px solid #ddd;
  background-color: #f8f9fa;
  padding: 20px;
}

.sidebar h3 {
  margin-bottom: 20px;
  font-size: 18px;
  font-weight: bold;
}

.sidebar-nav {
  list-style: none;
  padding: 0;
}

.sidebar-nav li {
  margin-bottom: 10px;
}

.sidebar-nav a {
  display: block;
  padding: 10px;
  color: #333;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.sidebar-nav a:hover,
.sidebar-nav a.active {
  background-color: #e9ecef;
  text-decoration: none;
}

.main-content {
  flex: 1;
  padding: 20px;
}

/* Header Layout */
.header {
  border-bottom: 1px solid #ddd;
  background-color: #fff;
  padding: 0 20px;
}

.header-nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 60px;
}

.header-nav .brand {
  font-weight: bold;
  font-size: 20px;
}

.header-nav .nav-links {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: 20px;
}

.header-nav .nav-links a {
  padding: 8px 16px;
  color: #333;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.header-nav .nav-links a:hover {
  background-color: #f8f9fa;
  text-decoration: none;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 12px;
  border-radius: 20px;
  background-color: #f8f9fa;
}

.user-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background-color: #007bff;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  font-weight: bold;
}
```

**Import in your main.tsx:**
```typescript
import './index.css'
import './styles/components.css'
import './styles/layout.css'
```

### Individual Remote Applications - Detailed Specifications

## Project Overview

Create three standalone Vite + React + TypeScript applications that will later be converted into micro frontends. Each application should be fully functional as a standalone app with modern UI components and realistic business logic.

---

## Global Specifications (All Three Projects)

### Tech Stack (Outdated - Simplified Version Below)
- **Build Tool**: Vite
- **Framework**: React 19
- **Language**: TypeScript
- **Package Manager**: pnpm
- **UI Library**: ~~shadcn/ui + Tailwind CSS~~ → **Simple CSS**
- **Routing**: React Router DOM
- **Icons**: ~~Lucide React~~ → **Simple HTML/CSS**
- **Styling**: ~~Tailwind CSS with CSS variables for theming~~ → **Simple CSS**

### Project Structure (Each App)
```
src/
├── components/
│   └── layout/       # App-specific layout components
├── pages/            # Route pages
├── contexts/         # React contexts (will be used later for federation)
├── hooks/           # Custom React hooks
├── types/           # TypeScript type definitions
├── styles/          # CSS files for styling
└── data/            # Mock data for demonstration
```

### Common Files Needed (All Apps)

#### `src/types/index.ts`
```typescript
export interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'viewer';
}

export interface AppProps {
  user?: User | null;
  basePath?: string;
}
```

#### Common Layout Pattern for Each App
```typescript
// src/components/layout/AppLayout.tsx
import { ReactNode } from "react"

interface AppLayoutProps {
  children: ReactNode;
}

export const AppLayout = ({ children }: AppLayoutProps) => (
  <div className="sidebar-layout">
    <aside className="sidebar">
      <nav>
        {/* App-specific navigation will go here */}
      </nav>
    </aside>
    <main className="main-content">
      {children}
    </main>
  </div>
);
```

**Usage Example with Navigation (Products App):**
```typescript
// src/components/layout/AppLayout.tsx
import { ReactNode } from "react"
import { Link, useLocation } from "react-router-dom"

interface AppLayoutProps {
  children: ReactNode;
}

export const AppLayout = ({ children }: AppLayoutProps) => {
  const location = useLocation();
  
  const navItems = [
    { name: 'Product List', href: '/list', icon: '📦' },
    { name: 'Add Product', href: '/create', icon: '➕' },
    { name: 'Categories', href: '/categories', icon: '🏷️' },
  ];

  return (
    <div className="sidebar-layout">
      <aside className="sidebar">
        <h3>Products</h3>
        <ul className="sidebar-nav">
          {navItems.map((item) => (
            <li key={item.href}>
              <Link 
                to={item.href}
                className={location.pathname === item.href ? 'active' : ''}
              >
                <span style={{ marginRight: '8px' }}>{item.icon}</span>
                {item.name}
              </Link>
            </li>
          ))}
        </ul>
      </aside>
      <main className="main-content">
        {children}
      </main>
    </div>
  );
};
```

#### Package.json Template (Update after setup)
```json
{
  "name": "mf-[APP-NAME]-app",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite --port [PORT]",
    "build": "vite build",
    "build:watch": "vite build --watch",
    "preview": "vite preview --port [PORT]",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0"
  },
  "dependencies": {
    "react": "^19.1.0",
    "react-dom": "^19.1.0",
    "react-router-dom": "^6.20.0"
  },
  "devDependencies": {
    "@types/react": "^19.1.0",
    "@types/react-dom": "^19.1.0",
    "@typescript-eslint/eslint-plugin": "^6.14.0",
    "@typescript-eslint/parser": "^6.14.0",
    "@vitejs/plugin-react": "^4.2.1",
    "eslint": "^8.55.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "eslint-plugin-react-refresh": "^0.4.5",
    "typescript": "^5.2.2",
    "vite": "^5.0.8"
  }
}
```

**Note**: After running the setup commands, your package.json will be automatically updated with the correct versions.

---

## Project 1: Products App (`mf-products-app`)

### Port: 5001

### Business Context
A product catalog management system where users can browse, search, and manage products. Admin users can add/edit products, while regular users can only browse.

### Pages Required

#### 1. Products List (`/list`)
- **Components**: Product grid/table with search and filters
- **Features**: 
  - Search products by name/description
  - Filter by category and price range
  - Pagination
  - Different views for admin vs regular users
- **Mock Data**: 15-20 sample products with categories, prices, descriptions

#### 2. Product Detail (`/detail/:id`)
- **Components**: Detailed product view with images, specs, reviews
- **Features**:
  - Product information display
  - Image gallery (mock URLs)
  - Related products section
  - Edit button for admins

#### 3. Add Product (`/create`)
- **Components**: Product creation form (admin only)
- **Features**:
  - Form with validation
  - Category selection
  - Price input
  - Description textarea
  - Mock image upload interface

#### 4. Categories (`/categories`)
- **Components**: Category management interface
- **Features**:
  - List all categories
  - Add/edit categories (admin only)
  - Product count per category

### Left Navigation Items
```typescript
const navigation = [
  { name: 'Product List', href: '/list', icon: 'Package' },
  { name: 'Add Product', href: '/create', icon: 'Plus' },
  { name: 'Categories', href: '/categories', icon: 'Tags' },
]
```

### Mock Data Structure
```typescript
interface Product {
  id: string;
  name: string;
  description: string;
  price: number;
  category: string;
  imageUrl: string;
  inStock: boolean;
  createdAt: string;
}

interface Category {
  id: string;
  name: string;
  description: string;
  productCount: number;
}
```

### Detailed Mock Data Examples (Use These Exact Examples)

#### `src/data/mockProducts.ts`
```typescript
export const mockProducts: Product[] = [
  {
    id: "1",
    name: "Wireless Bluetooth Headphones",
    description: "Premium noise-cancelling headphones with 30-hour battery life and superior sound quality",
    price: 299.99,
    category: "electronics",
    imageUrl: "https://images.unsplash.com/photo-1505740420928-5e560c06d30e",
    inStock: true,
    createdAt: "2024-01-15T10:30:00Z"
  },
  {
    id: "2", 
    name: "Gaming Mechanical Keyboard",
    description: "RGB backlit mechanical keyboard with Cherry MX switches and programmable keys",
    price: 159.99,
    category: "electronics",
    imageUrl: "https://images.unsplash.com/photo-1541140532154-b024d705b90a",
    inStock: true,
    createdAt: "2024-01-12T14:20:00Z"
  },
  {
    id: "3",
    name: "Ergonomic Office Chair",
    description: "Adjustable ergonomic office chair with lumbar support and breathable mesh back",
    price: 399.99,
    category: "furniture",
    imageUrl: "https://images.unsplash.com/photo-1586023492125-27b2c045efd7",
    inStock: false,
    createdAt: "2024-01-10T09:15:00Z"
  },
  {
    id: "4",
    name: "Stainless Steel Water Bottle",
    description: "Insulated water bottle that keeps drinks cold for 24h or hot for 12h",
    price: 34.99,
    category: "lifestyle",
    imageUrl: "https://images.unsplash.com/photo-1523362628745-0c100150b504",
    inStock: true,
    createdAt: "2024-01-08T16:45:00Z"
  },
  {
    id: "5",
    name: "Yoga Mat Premium",
    description: "Non-slip yoga mat made from eco-friendly materials with alignment lines",
    price: 79.99,
    category: "fitness",
    imageUrl: "https://images.unsplash.com/photo-1544367567-0f2fcb009e0b",
    inStock: true,
    createdAt: "2024-01-05T11:30:00Z"
  },
  {
    id: "6",
    name: "Smart Watch Series 5",
    description: "Advanced fitness tracker with heart rate monitor and GPS capability",
    price: 249.99,
    category: "electronics",
    imageUrl: "https://images.unsplash.com/photo-1523275335684-37898b6baf30",
    inStock: true,
    createdAt: "2024-01-03T13:22:00Z"
  },
  {
    id: "7",
    name: "Coffee Grinder Manual",
    description: "Hand-crank coffee grinder with adjustable grind settings for perfect brewing",
    price: 89.99,
    category: "kitchen",
    imageUrl: "https://images.unsplash.com/photo-1495474472287-4d71bcdd2085",
    inStock: false,
    createdAt: "2024-01-01T08:00:00Z"
  }
];

export const mockCategories: Category[] = [
  {
    id: "1",
    name: "Electronics",
    description: "Gadgets, devices, and electronic accessories",
    productCount: 12
  },
  {
    id: "2", 
    name: "Furniture",
    description: "Home and office furniture items",
    productCount: 8
  },
  {
    id: "3",
    name: "Lifestyle",
    description: "Everyday items that enhance your lifestyle",
    productCount: 15
  },
  {
    id: "4",
    name: "Fitness",
    description: "Sports and fitness equipment",
    productCount: 6
  },
  {
    id: "5",
    name: "Kitchen",
    description: "Kitchen appliances and cooking tools",
    productCount: 9
  }
];
```

---

## Project 2: Orders App (`mf-orders-app`)

### Port: 5002

### Business Context
An order management system where users can view their orders and admins can manage all orders. Includes order tracking, status updates, and basic analytics.

### Pages Required

#### 1. Orders List (`/list`)
- **Components**: Orders table with status badges and filters
- **Features**:
  - Filter by status (pending, processing, shipped, delivered)
  - Date range filtering
  - Search by order number or customer
  - Different permissions for admin vs user

#### 2. Order Detail (`/detail/:id`)
- **Components**: Comprehensive order view
- **Features**:
  - Order items breakdown
  - Shipping information
  - Order timeline/status history
  - Status update controls (admin only)

#### 3. Create Order (`/create`)
- **Components**: Order creation form (admin only)
- **Features**:
  - Customer selection
  - Product selection with quantities
  - Shipping address form
  - Order total calculation

#### 4. Order Analytics (`/analytics`)
- **Components**: Simple charts and metrics
- **Features**:
  - Orders by status (pie chart mockup)
  - Monthly order trends
  - Top customers
  - Revenue metrics

### Left Navigation Items
```typescript
const navigation = [
  { name: 'All Orders', href: '/list', icon: 'ShoppingCart' },
  { name: 'Create Order', href: '/create', icon: 'Plus' },
  { name: 'Analytics', href: '/analytics', icon: 'BarChart3' },
]
```

### Mock Data Structure
```typescript
interface Order {
  id: string;
  orderNumber: string;
  customerId: string;
  customerName: string;
  status: 'pending' | 'processing' | 'shipped' | 'delivered' | 'cancelled';
  items: OrderItem[];
  total: number;
  shippingAddress: Address;
  createdAt: string;
  updatedAt: string;
}

interface OrderItem {
  productId: string;
  productName: string;
  quantity: number;
  price: number;
}

interface Address {
  street: string;
  city: string;
  state: string;
  zipCode: string;
  country: string;
}
```

### Detailed Mock Data Examples (Use These Exact Examples)

#### `src/data/mockOrders.ts`
```typescript
export const mockOrders: Order[] = [
  {
    id: "1",
    orderNumber: "ORD-2024-001",
    customerId: "cust-001",
    customerName: "John Smith",
    status: "delivered",
    items: [
      {
        productId: "1",
        productName: "Wireless Bluetooth Headphones",
        quantity: 1,
        price: 299.99
      },
      {
        productId: "6",
        productName: "Smart Watch Series 5", 
        quantity: 1,
        price: 249.99
      }
    ],
    total: 549.98,
    shippingAddress: {
      street: "123 Main St",
      city: "New York",
      state: "NY", 
      zipCode: "10001",
      country: "USA"
    },
    createdAt: "2024-01-20T10:30:00Z",
    updatedAt: "2024-01-25T16:45:00Z"
  },
  {
    id: "2",
    orderNumber: "ORD-2024-002", 
    customerId: "cust-002",
    customerName: "Sarah Johnson",
    status: "processing",
    items: [
      {
        productId: "3",
        productName: "Ergonomic Office Chair",
        quantity: 1,
        price: 399.99
      }
    ],
    total: 399.99,
    shippingAddress: {
      street: "456 Oak Avenue",
      city: "Los Angeles",
      state: "CA",
      zipCode: "90210", 
      country: "USA"
    },
    createdAt: "2024-01-22T14:15:00Z",
    updatedAt: "2024-01-23T09:30:00Z"
  },
  {
    id: "3",
    orderNumber: "ORD-2024-003",
    customerId: "cust-003", 
    customerName: "Mike Davis",
    status: "shipped",
    items: [
      {
        productId: "2",
        productName: "Gaming Mechanical Keyboard",
        quantity: 2,
        price: 159.99
      },
      {
        productId: "4",
        productName: "Stainless Steel Water Bottle",
        quantity: 3,
        price: 34.99
      }
    ],
    total: 424.95,
    shippingAddress: {
      street: "789 Pine Road",
      city: "Chicago",
      state: "IL",
      zipCode: "60601",
      country: "USA"
    },
    createdAt: "2024-01-18T11:20:00Z",
    updatedAt: "2024-01-24T13:15:00Z"
  },
  {
    id: "4",
    orderNumber: "ORD-2024-004",
    customerId: "cust-004",
    customerName: "Emily Brown",
    status: "pending",
    items: [
      {
        productId: "5",
        productName: "Yoga Mat Premium",
        quantity: 1,
        price: 79.99
      }
    ],
    total: 79.99,
    shippingAddress: {
      street: "321 Elm Street", 
      city: "Austin",
      state: "TX",
      zipCode: "73301",
      country: "USA"
    },
    createdAt: "2024-01-25T16:30:00Z",
    updatedAt: "2024-01-25T16:30:00Z"
  },
  {
    id: "5",
    orderNumber: "ORD-2024-005",
    customerId: "cust-005",
    customerName: "David Wilson",
    status: "cancelled",
    items: [
      {
        productId: "7",
        productName: "Coffee Grinder Manual",
        quantity: 1,
        price: 89.99
      }
    ],
    total: 89.99,
    shippingAddress: {
      street: "654 Maple Drive",
      city: "Seattle",
      state: "WA",
      zipCode: "98101",
      country: "USA"
    },
    createdAt: "2024-01-15T09:45:00Z",
    updatedAt: "2024-01-16T14:20:00Z"
  }
];
```

---

## Project 3: Users App (`mf-users-app`)

### Port: 5003

### Business Context
A user management system for managing customers, staff, and user permissions. Includes user profiles, role management, and activity tracking.

### Pages Required

#### 1. Users List (`/list`)
- **Components**: Users table with role badges and search
- **Features**:
  - Search users by name/email
  - Filter by role and status
  - User status toggle (active/inactive)
  - Bulk actions for admins

#### 2. User Detail (`/detail/:id`)
- **Components**: Complete user profile view
- **Features**:
  - User information display
  - Activity history
  - Edit profile button
  - Role management (admin only)

#### 3. Add User (`/create`)
- **Components**: User creation form (admin only)
- **Features**:
  - Personal information form
  - Role selection
  - Permission assignment
  - Email verification mock

#### 4. Roles & Permissions (`/roles`)
- **Components**: Role management interface
- **Features**:
  - List all roles
  - Edit role permissions
  - User count per role
  - Permission matrix view

### Left Navigation Items
```typescript
const navigation = [
  { name: 'All Users', href: '/list', icon: 'Users' },
  { name: 'Add User', href: '/create', icon: 'UserPlus' },
  { name: 'Roles & Permissions', href: '/roles', icon: 'Shield' },
]
```

### Mock Data Structure
```typescript
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'viewer';
  status: 'active' | 'inactive';
  lastLogin: string;
  createdAt: string;
  avatar?: string;
}

interface Role {
  id: string;
  name: string;
  description: string;
  permissions: string[];
  userCount: number;
}
```

### Detailed Mock Data Examples (Use These Exact Examples)

#### `src/data/mockUsers.ts`
```typescript
export const mockUsers: User[] = [
  {
    id: "1",
    name: "John Smith",
    email: "john.smith@company.com",
    role: "admin",
    status: "active",
    lastLogin: "2024-01-25T14:30:00Z",
    createdAt: "2023-06-15T09:00:00Z",
    avatar: "https://images.unsplash.com/photo-1472099645785-5658abf4ff4e"
  },
  {
    id: "2",
    name: "Sarah Johnson", 
    email: "sarah.johnson@company.com",
    role: "user",
    status: "active",
    lastLogin: "2024-01-24T16:45:00Z",
    createdAt: "2023-08-22T10:30:00Z",
    avatar: "https://images.unsplash.com/photo-1494790108755-2616b332a7dd"
  },
  {
    id: "3",
    name: "Mike Davis",
    email: "mike.davis@company.com", 
    role: "user",
    status: "active",
    lastLogin: "2024-01-25T08:15:00Z",
    createdAt: "2023-09-10T14:20:00Z",
    avatar: "https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d"
  },
  {
    id: "4",
    name: "Emily Brown",
    email: "emily.brown@company.com",
    role: "viewer",
    status: "inactive", 
    lastLogin: "2024-01-20T11:30:00Z",
    createdAt: "2023-11-05T13:45:00Z",
    avatar: "https://images.unsplash.com/photo-1438761681033-6461ffad8d80"
  },
  {
    id: "5", 
    name: "David Wilson",
    email: "david.wilson@company.com",
    role: "admin",
    status: "active",
    lastLogin: "2024-01-25T12:00:00Z", 
    createdAt: "2023-05-30T16:15:00Z",
    avatar: "https://images.unsplash.com/photo-1500648767791-00dcc994a43e"
  },
  {
    id: "6",
    name: "Lisa Anderson", 
    email: "lisa.anderson@company.com",
    role: "user",
    status: "active",
    lastLogin: "2024-01-23T09:45:00Z",
    createdAt: "2023-10-12T11:00:00Z",
    avatar: "https://images.unsplash.com/photo-1544005313-94ddf0286df2"
  }
];

export const mockRoles: Role[] = [
  {
    id: "1",
    name: "Admin",
    description: "Full system access with all permissions",
    permissions: [
      "users.create",
      "users.read", 
      "users.update",
      "users.delete",
      "products.create",
      "products.read",
      "products.update", 
      "products.delete",
      "orders.create",
      "orders.read",
      "orders.update",
      "orders.delete",
      "analytics.read"
    ],
    userCount: 2
  },
  {
    id: "2", 
    name: "User",
    description: "Standard user with limited permissions",
    permissions: [
      "products.read",
      "orders.read",
      "orders.create",
      "profile.update"
    ],
    userCount: 3
  },
  {
    id: "3",
    name: "Viewer",
    description: "Read-only access to basic information", 
    permissions: [
      "products.read",
      "orders.read",
      "profile.read"
    ],
    userCount: 1
  }
];
```

---

## Implementation Instructions

### Step 1: Setup Each Project
1. Create new Vite React TypeScript project
2. Install all dependencies via pnpm
3. Setup simple CSS styling
4. Configure the port in package.json

### Step 2: Create Base Structure
1. Setup the folder structure as specified
2. Create the common files (types, utils, layout)
3. Setup React Router with the specified routes
4. Implement the left sidebar navigation

### Step 3: Build Pages
1. Create each page component with proper TypeScript interfaces
2. Use simple HTML elements and CSS for styling
3. Implement mock data and business logic
4. Add proper loading states and error handling

### Step 4: Styling & Polish
1. Use CSS flexbox/grid for responsive design
2. Add proper hover states and transitions with simple CSS
3. Ensure accessibility with proper ARIA labels
4. Test all navigation and functionality

### Key Requirements for Each App
- ✅ **Fully functional standalone operation**
- ✅ **Clean UI using simple CSS and semantic HTML**
- ✅ **Responsive design with CSS flexbox/grid**
- ✅ **TypeScript throughout with proper interfaces**
- ✅ **Left sidebar navigation with active states**
- ✅ **Mock data that demonstrates the business logic**
- ✅ **Different user role behaviors (admin vs user)**
- ✅ **Proper routing with React Router**
- ✅ **Loading states and basic error handling**

### Testing Each App
Each app should be fully testable by running:
```bash
pnpm dev
```

And navigating through all routes to verify:
- All pages load correctly
- Navigation works
- Mock data displays properly
- Admin/user role differences are visible
- UI is responsive and professional

This specification should provide enough detail for any AI assistant to create three production-quality micro frontend applications that will work perfectly for your presentation!