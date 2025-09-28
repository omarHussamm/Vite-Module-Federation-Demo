# Micro Frontend Federation Implementation Plan - Revised

> **🎨 Styling Note**: Throughout this document, you'll see some inline styles in code examples for clarity. In your actual implementation, **use the organized CSS class structure** outlined in `Phase-1-Project-Specifications.md` with separate files (`src/styles/layout.css`, `src/styles/components.css`) instead of inline styles. This keeps your code clean and maintainable!

## Phase 1: Foundation - Standalone Applications

### 1.1 Git Repository Setup for Individual Remotes

**Create three separate repositories:**

```bash
# Create individual repositories for each remote
git init mf-products-app
cd mf-products-app
# ... create products app structure
git add .
git commit -m "Initial products app setup"
git remote add origin git@github.com:yourusername/mf-products-app
git push -u origin main

# Repeat for orders and users
git init mf-orders-app
# ... setup and push
git init mf-users-app
# ... setup and push
```

### 1.2 Create Three Standalone Vite + React + TypeScript Remotes

- **Remote 1**: `mf-products-app` (Product catalog)
- **Remote 2**: `mf-orders-app` (Order management)  
- **Remote 3**: `mf-users-app` (User management)

**Each remote should have:**

typescript

```typescript
// types/index.ts
export interface User {
  id: string;
  name: string;
  email: string;
  role: string;
}

export interface AppProps {
  user?: User;
  basePath?: string;
}
```

**Initial Layout Structure for Each Remote:**

> **Note**: In your actual implementation, use the CSS classes from `src/styles/layout.css` and `src/styles/components.css` as outlined in Phase-1-Project-Specifications.md instead of inline styles.

```typescript
// components/Layout.tsx (in each remote)
import { Link, useLocation } from "react-router-dom"

const RemoteLayout: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const location = useLocation();
  
  return (
    <div className="sidebar-layout">
      <aside className="sidebar">
        <nav>
          <h3>Products {/* Or Orders/Users based on app */}</h3>
          <ul className="sidebar-nav">
            <li>
              <Link 
                to="/list" 
                className={location.pathname === '/list' ? 'active' : ''}
              >
                📦 Product List
              </Link>
            </li>
            <li>
              <Link 
                to="/create"
                className={location.pathname === '/create' ? 'active' : ''}
              >
                ➕ Add Product
              </Link>
            </li>
            <li>
              <Link 
                to="/categories"
                className={location.pathname === '/categories' ? 'active' : ''}
              >
                🏷️ Categories
              </Link>
            </li>
          </ul>
        </nav>
      </aside>
      <main className="main-content">
        {children}
      </main>
    </div>
  );
};
```

**Initial setup:**

- Individual React Router setup with BrowserRouter
- **Left-side navigation** within each app (will contrast with host's top navigation later)
- Basic routing within each app
- TypeScript configuration
- Error boundaries for each app

### 1.3 pnpm Setup for Each Remote

**Package.json for each remote (using pnpm):**

```json
{
  "name": "mf-products-app",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite --port 5001",
    "build": "vite build",
    "build:watch": "vite build --watch",
    "preview": "vite preview --port 5001",
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
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "@vitejs/plugin-react": "^4.0.3",
    "@originjs/vite-plugin-federation": "^1.3.5",
    "eslint": "^8.45.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "eslint-plugin-react-refresh": "^0.4.3",
    "typescript": "^5.0.2",
    "vite": "^4.4.5"
  }
}
```

**Install dependencies in each remote:**

```bash
cd mf-products-app && pnpm install
cd mf-orders-app && pnpm install  
cd mf-users-app && pnpm install
```

### 1.4 Development Setup for Standalone Apps

**Package.json for each remote:**

```json
{
  "scripts": {
    "dev": "vite --port 5001",           // Products on 5001, Orders on 5002, Users on 5003
    "build": "tsc && vite build",               
    "preview": "vite preview --port 5001"
  }
}
```

**Run each remote standalone:**

```bash
# Terminal 1
cd mf-products-app && pnpm dev  # ✅ Hot reload works perfectly

# Terminal 2  
cd mf-orders-app && pnpm dev    # ✅ Hot reload works perfectly

# Terminal 3
cd mf-users-app && pnpm dev     # ✅ Hot reload works perfectly
```

### 1.5 Demonstrate Standalone Functionality

- Show each app running independently on different ports
- **Highlight the left-side navigation** in each remote
- Navigate through each app's routes with **full hot reload**
- Emphasize the isolated nature and independent UI layouts
- **Key point**: Normal Vite development experience works perfectly

---

## Phase 2: Basic Federation Setup + Git Submodules

### 2.1 Create Vite Host Application

**Create and setup host as separate repository:**

```bash
# Create host repository
git init mf-host-app
cd mf-host-app
# ... create host app structure
git add .
git commit -m "Initial host app setup"  
git remote add origin git@github.com:yourusername/mf-host-app
git push -u origin main
```

**Host package.json (using pnpm):**

```json
{
  "name": "mf-host-app", 
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite --port 5000",
    "build": "vite build", 
    "preview": "vite preview --port 5000"
  },
  "dependencies": {
    "react": "^19.1.0",
    "react-dom": "^19.1.0", 
    "react-router-dom": "^6.20.0"
  },
  "devDependencies": {
    "@types/react": "^19.1.0",
    "@types/react-dom": "^19.1.0",
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "@vitejs/plugin-react": "^4.0.3",
    "@originjs/vite-plugin-federation": "^1.3.5",
    "eslint": "^8.45.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "eslint-plugin-react-refresh": "^0.4.3",
    "typescript": "^5.0.2",
    "vite": "^4.4.5"
  }
}
```

### 2.2 Create Main Repository with Git Submodules

**Setup main repository with submodules:**

```bash
# Create main repository for orchestration (empty)
git init microfrontend-federation-main
cd microfrontend-federation-main

# Create root configuration files first
# (we'll show these files in the next step)

# Add each app as submodule (directories should NOT exist yet)
git submodule add git@github.com:yourusername/mf-products-app products-app
git submodule add git@github.com:yourusername/mf-orders-app orders-app
git submodule add git@github.com:yourusername/mf-users-app users-app  
git submodule add git@github.com:yourusername/mf-host-app host-app

# Commit submodule setup
git add .gitmodules products-app orders-app users-app host-app
git commit -m "Add micro frontend submodules"
git remote add origin git@github.com:yourusername/microfrontend-federation-main
git push -u origin main
```

**Important Note:** The directories `products-app`, `orders-app`, `users-app`, and `host-app` should NOT exist before running the `git submodule add` commands. Git will create them and clone the repositories into them.

### 2.3 pnpm Workspace Configuration

**Root `pnpm-workspace.yaml`:**

```yaml
packages:
  - 'host-app'
  - 'products-app'
  - 'orders-app'
  - 'users-app'
```

**Root `package.json` (orchestration only):**

```json
{
  "name": "microfrontend-federation",
  "private": true,
  "scripts": {
    "install:all": "pnpm install -r",
    "submodules:update": "git submodule update --remote",
    "submodules:init": "git submodule update --init --recursive",
    
    "dev:products": "pnpm --filter products-app dev",
    "dev:orders": "pnpm --filter orders-app dev",
    "dev:users": "pnpm --filter users-app dev", 
    "dev:host": "pnpm --filter host-app dev",
    
    "build:remotes": "pnpm --filter products-app build && pnpm --filter orders-app build && pnpm --filter users-app build",
    "preview:remotes": "concurrently \"pnpm --filter products-app preview\" \"pnpm --filter orders-app preview\" \"pnpm --filter users-app preview\"",
    
    "dev:federation": "concurrently \"pnpm --filter products-app build:watch\" \"pnpm --filter orders-app build:watch\" \"pnpm --filter users-app build:watch\" \"sleep 3 && pnpm --filter products-app preview\" \"sleep 3 && pnpm --filter orders-app preview\" \"sleep 3 && pnpm --filter users-app preview\" \"sleep 5 && pnpm --filter host-app dev\"",
    
    "build:all": "pnpm build:remotes && pnpm --filter host-app build",
    "preview:all": "pnpm preview:remotes && pnpm --filter host-app preview"
  },
  "devDependencies": {
    "concurrently": "^8.2.2"
  }
}
```

**Setup workspace:**

```bash
cd microfrontend-federation-main
pnpm install:all  # Installs deps in all submodules
```

### 2.4 Configure Module Federation

**Host `vite.config.ts`:**

typescript

```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import federation from '@originjs/vite-plugin-federation'

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'host-app',
      remotes: {
        productsApp: 'http://localhost:5001/assets/remoteEntry.js',
        ordersApp: 'http://localhost:5002/assets/remoteEntry.js',
        usersApp: 'http://localhost:5003/assets/remoteEntry.js',
      }
    })
  ]
})
```

**Each Remote `vite.config.ts`:**

typescript

```typescript
federation({
  name: 'products-app',
  filename: 'remoteEntry.js',
  exposes: {
    './App': './src/App.tsx',
  }
})
```

### 2.3 Host Layout Implementation

**Host layout structure with top navigation:**

> **Note**: In your actual implementation, use the CSS classes from `src/styles/layout.css` and `src/styles/components.css` instead of inline styles.

```typescript
// components/Layout.tsx (in host)
import { Link } from "react-router-dom"
import { User } from "@/types"

interface LayoutProps {
  children: React.ReactNode;
  user?: User;
}

const Layout: React.FC<LayoutProps> = ({ children, user }) => (
  <div>
    <header className="header">
      <nav className="header-nav">
        <div style={{ display: 'flex', alignItems: 'center', gap: '30px' }}>
          <div className="brand">MicroFrontend App</div>
          <ul className="nav-links">
            <li><Link to="/products">Products</Link></li>
            <li><Link to="/orders">Orders</Link></li>
            <li><Link to="/users">Users</Link></li>
          </ul>
        </div>
        <div style={{ display: 'flex', alignItems: 'center', gap: '15px' }}>
          <div className="user-info">
            <div className="user-avatar">
              {user?.name?.split(' ').map(n => n[0]).join('') || 'U'}
            </div>
            <span>{user?.name || 'Guest'}</span>
          </div>
          <div>
            <Link to="/profile" className="btn btn-outline" style={{ marginRight: '10px' }}>
              Profile
            </Link>
            <Link to="/licenses" className="btn btn-outline">
              Licenses
            </Link>
          </div>
        </div>
      </nav>
    </header>
    <main className="main-content">
      {children}
    </main>
  </div>
);
```

### 2.4 Demonstrate Navigation Conflicts

- Show that remotes now have **both left navigation (their own) AND top navigation (host)**
- Demonstrate routing conflicts with multiple router instances
- Highlight URL conflicts and navigation problems
- **Visual problem**: Duplicate navigation areas creating confusing UX

### 2.5 Development Reality Check - Federation Requirements

**The Problem: Try to run federation with normal dev servers:**

```bash
# This seems logical but WON'T WORK:
cd products-app && pnpm dev  # ❌ No remoteEntry.js generated!
cd orders-app && pnpm dev    # ❌ Vite dev is bundleless
cd users-app && pnpm dev     # ❌ Federation needs built files
cd host-app && pnpm dev      # ❌ Can't find remote modules
```

**Error you'll see in host:**
```
Failed to fetch dynamically imported module: http://localhost:5001/assets/remoteEntry.js
```

**The Solution: Federation requires built files:**

```bash
# Correct approach for federation development:
# Terminal 1-3: Build and serve remotes
cd products-app && pnpm build && pnpm preview  # ✅ Generates remoteEntry.js
cd orders-app && pnpm build && pnpm preview    # ✅ Serves built files  
cd users-app && pnpm build && pnpm preview     # ✅ Federation ready

# Terminal 4: Host can now consume remotes
cd host-app && pnpm dev  # ✅ Host gets hot reload, finds remotes
```

**OR use the root orchestration script:**

```bash
# From main repository root
pnpm dev:federation  # ✅ Starts everything in correct order
```

**Key Insight for Live Demo:**
- `vite dev` = bundleless development (no remoteEntry.js)
- `vite build` = creates the federation bundle
- Federation requires the built `remoteEntry.js` file

---

## Phase 3: Centralized Routing Architecture

### 3.1 Remove Individual Routers and Left Navigation from Remotes

**Transform each remote to remove router and left navigation:**

typescript

```typescript
// Before: Remote with router and left navigation
const App = () => (
  <BrowserRouter>
    <RemoteLayout> {/* Has left sidebar */}
      <Routes>
        <Route path="/products" element={<ProductList />} />
        <Route path="/products/:id" element={<ProductDetail />} />
      </Routes>
    </RemoteLayout>
  </BrowserRouter>
);

// After: Remote exposing just content, no router, no left nav
export const routes = [
  { path: '/list', element: <ProductList /> },
  { path: '/detail/:id', element: <ProductDetail /> }
];

const App: React.FC<AppProps> = ({ user, basePath = '' }) => (
  <div style={{ padding: '20px' }}>
    {/* Just the content, no layout wrapper, no router */}
    <Routes>
      <Route path="/list" element={<ProductList />} />
      <Route path="/detail/:id" element={<ProductDetail />} />
    </Routes>
  </div>
);
```

### 3.2 Implement Centralized Router in Host

typescript

```typescript
// Router.tsx in host
import { BrowserRouter, Routes, Route } from 'react-router-dom';

const AppRouter = () => (
  <BrowserRouter>
    <Layout> {/* Only host layout with top navigation */}
      <Routes>
        <Route path="/products/*" element={<ProductsApp basePath="/products" />} />
        <Route path="/orders/*" element={<OrdersApp basePath="/orders" />} />
        <Route path="/users/*" element={<UsersApp basePath="/users" />} />
      </Routes>
    </Layout>
  </BrowserRouter>
);
```

### 3.3 Update Remote Navigation to Use BasePath

typescript

```typescript
// In remote components - navigation now works in both modes
import { useNavigate, Link } from "react-router-dom"

const ProductList = ({ basePath = '' }) => {
  const navigate = useNavigate();
  
  const handleViewDetail = (productId: string) => {
    navigate(`${basePath}/detail/${productId}`); // Works standalone AND federated
  };
  
  return (
    <div style={{ marginBottom: '20px' }}>
      <div style={{ 
        display: 'flex', 
        justifyContent: 'space-between', 
        alignItems: 'center',
        marginBottom: '20px'
      }}>
        <h2 style={{ fontSize: '24px', fontWeight: 'bold', margin: 0 }}>Products</h2>
        <Link 
          to={`${basePath}/create`}
          style={{
            padding: '10px 15px',
            backgroundColor: '#007bff',
            color: 'white',
            textDecoration: 'none',
            borderRadius: '4px',
            fontSize: '14px'
          }}
        >
          Add New Product
        </Link>
      </div>
      {/* Product list content */}
    </div>
  );
};
```

**Demonstrate the fix:**

- Single navigation (host's top bar only)
- Clean routing without conflicts
- Remote navigation adapts to context via basePath

---

## Phase 4: State Sharing Implementation

### 4.1 Create Profile Route in Host

typescript

```typescript
// pages/Profile.tsx
import { User } from "@/types"

interface ProfileProps {
  user: User;
  onUserUpdate: (user: User) => void;
}

const Profile: React.FC<ProfileProps> = ({ user, onUserUpdate }) => {
  return (
    <div style={{ maxWidth: '600px', margin: '0 auto' }}>
      <div className="card">
        <div style={{ textAlign: 'center', marginBottom: '30px' }}>
          <div 
            className="user-avatar" 
            style={{ 
              width: '100px', 
              height: '100px', 
              fontSize: '36px',
              margin: '0 auto 20px'
            }}
          >
            {user.name.split(' ').map(n => n[0]).join('')}
          </div>
          <h2 style={{ fontSize: '24px', margin: 0 }}>{user.name}</h2>
        </div>
        
        <div style={{ 
          display: 'grid', 
          gridTemplateColumns: '1fr 1fr', 
          gap: '20px',
          marginBottom: '30px'
        }}>
          <div>
            <label style={{ 
              fontSize: '14px', 
              fontWeight: '500', 
              color: '#666',
              display: 'block',
              marginBottom: '5px'
            }}>Email</label>
            <p style={{ margin: 0, fontSize: '16px' }}>{user.email}</p>
          </div>
          <div>
            <label style={{ 
              fontSize: '14px', 
              fontWeight: '500', 
              color: '#666',
              display: 'block',
              marginBottom: '5px'
            }}>Role</label>
            <span className={`badge ${user.role === 'admin' ? 'badge-primary' : 'badge-secondary'}`}>
              {user.role}
            </span>
          </div>
        </div>
        
        <button className="btn" style={{ width: '100%' }}>
          Edit Profile
        </button>
      </div>
    </div>
  );
};
```

### 4.2 Implement User State Management in Host

typescript

```typescript
// hooks/useUser.ts
export const useUser = () => {
  const [user, setUser] = useState<User>({
    id: '1',
    name: 'John Doe',
    email: 'john@example.com',
    role: 'admin'
  });

  return { user, setUser };
};
```

### 4.3 Pass User State to Remotes via Props

typescript

```typescript
// Host App.tsx
const App = () => {
  const { user, setUser } = useUser();

  return (
    <BrowserRouter>
      <Layout user={user}>
        <Routes>
          <Route path="/profile" element={<Profile user={user} onUserUpdate={setUser} />} />
          <Route 
            path="/products/*" 
            element={<ProductsApp user={user} basePath="/products" />} 
          />
          <Route 
            path="/orders/*" 
            element={<OrdersApp user={user} basePath="/orders" />} 
          />
          <Route 
            path="/users/*" 
            element={<UsersApp user={user} basePath="/users" />} 
          />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
};
```

### 4.4 Create Context in Remotes for Props Access

typescript

```typescript
// contexts/AppContext.tsx (in each remote)
import React, { createContext, useContext, ReactNode } from 'react';

interface AppContextType {
  user?: User | null;
  basePath?: string;
}

const AppContext = createContext<AppContextType | undefined>(undefined);

export const AppProvider: React.FC<{ children: ReactNode; value: AppContextType }> = ({ 
  children, 
  value 
}) => (
  <AppContext.Provider value={value}>
    {children}
  </AppContext.Provider>
);

export const useAppContext = () => {
  const context = useContext(AppContext);
  if (context === undefined) {
    throw new Error('useAppContext must be used within an AppProvider');
  }
  return context;
};
```

### 4.5 Use Shared State in Remotes

typescript

```typescript
// In ProductList component (remote)
import { Link } from "react-router-dom"
import { useAppContext } from "@/contexts/AppContext"

const ProductList = () => {
  const { user, basePath = '' } = useAppContext();
  
  return (
    <div style={{ marginBottom: '20px' }}>
      <div style={{ 
        display: 'flex', 
        justifyContent: 'space-between', 
        alignItems: 'center',
        marginBottom: '20px'
      }}>
        <div>
          <h2 style={{ fontSize: '24px', fontWeight: 'bold', margin: '0 0 10px 0' }}>
            Products for {user?.name || 'Guest'}
          </h2>
          <span style={{
            display: 'inline-block',
            padding: '4px 8px',
            fontSize: '12px',
            border: '1px solid #ddd',
            borderRadius: '12px',
            backgroundColor: '#f8f9fa',
            color: '#495057'
          }}>
            {user?.role}
          </span>
        </div>
        {user?.role === 'admin' && (
          <Link 
            to={`${basePath}/admin`}
            style={{
              padding: '10px 15px',
              backgroundColor: '#6c757d',
              color: 'white',
              textDecoration: 'none',
              borderRadius: '4px',
              fontSize: '14px'
            }}
          >
            Admin Panel
          </Link>
        )}
      </div>
      {/* Product list filtered by user permissions */}
    </div>
  );
};
```

### 4.6 Update Remote App to Use Context

typescript

```typescript
// Remote App.tsx
const App: React.FC<AppProps> = ({ user, basePath = '' }) => (
  <AppProvider value={{ user, basePath }}>
    <div style={{ padding: '20px' }}>
      <Routes>
        <Route path="/list" element={<ProductList />} />
        <Route path="/detail/:id" element={<ProductDetail />} />
      </Routes>
    </div>
  </AppProvider>
);
```

### 4.5 Props Validation in Remotes

typescript

```typescript
// utils/propsValidator.ts
export const validateProps = (props: AppProps): AppProps => {
  return {
    user: props.user || null,
    basePath: props.basePath || ''
  };
};

// Use in remote App
const App: React.FC<AppProps> = (props) => {
  const validatedProps = validateProps(props);
  // Use validatedProps throughout the app
};
```

### 4.6 Development Workflow for Active Development

**For ongoing development with auto-rebuild:**

```bash
# Option 1: Build watch + serve (recommended for active development)
# Terminal 1: Products
cd products-app
pnpm build -- --watch &  # Auto-rebuilds on changes
pnpm preview              # Serves the built files

# Terminal 2: Orders  
cd orders-app
pnpm build -- --watch &
pnpm preview

# Terminal 3: Users
cd users-app
pnpm build -- --watch &  
pnpm preview

# Terminal 4: Host (gets hot reload for host-only changes)
cd host-app && pnpm dev
```

**OR use root orchestration:**

```bash
# From main repository root
pnpm dev:federation  # ✅ Handles everything automatically
```

**Development Experience:**
- ✅ **Host changes**: Full hot reload
- ✅ **Remote changes**: Auto-rebuild, manual refresh needed in browser
- ✅ **Standalone remote development**: Use `npm run dev` for each remote individually

---

## Phase 5: Advanced Features - Error Handling & Loading States

### 5.1 Implement Error Boundaries

typescript

```typescript
// components/RemoteErrorBoundary.tsx
import React from "react"

interface Props {
  children: React.ReactNode;
  remoteName: string;
}

class RemoteErrorBoundary extends React.Component<Props, { hasError: boolean }> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error) {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error(`Error in remote ${this.props.remoteName}:`, error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        <div style={{
          margin: '20px',
          padding: '20px',
          border: '1px solid #dc3545',
          borderRadius: '8px',
          backgroundColor: '#f8d7da',
          color: '#721c24'
        }}>
          <div style={{ display: 'flex', alignItems: 'center', marginBottom: '10px' }}>
            <div style={{
              width: '20px',
              height: '20px',
              borderRadius: '50%',
              backgroundColor: '#dc3545',
              color: 'white',
              display: 'flex',
              alignItems: 'center',
              justifyContent: 'center',
              marginRight: '10px',
              fontSize: '14px'
            }}>
              !
            </div>
            <strong>Unable to load {this.props.remoteName}</strong>
          </div>
          <div style={{ marginBottom: '15px' }}>
            Please try refreshing the page or contact support.
          </div>
          <button 
            style={{
              padding: '6px 12px',
              backgroundColor: 'transparent',
              border: '1px solid #721c24',
              borderRadius: '4px',
              color: '#721c24',
              cursor: 'pointer',
              fontSize: '14px'
            }}
            onClick={() => window.location.reload()}
          >
            Refresh
          </button>
        </div>
      );
    }
    return this.props.children;
  }
}
```

### 5.2 Add Loading States

typescript

```typescript
// components/RemoteLoader.tsx
import React, { Suspense } from "react"

const RemoteLoadingSpinner = () => (
  <div style={{
    display: 'flex',
    flexDirection: 'column',
    alignItems: 'center',
    justifyContent: 'center',
    height: '200px',
    gap: '20px'
  }}>
    <div style={{
      width: '32px',
      height: '32px',
      border: '3px solid #f3f3f3',
      borderTop: '3px solid #007bff',
      borderRadius: '50%',
      animation: 'spin 1s linear infinite'
    }} />
    <p style={{ color: '#6c757d', margin: 0 }}>Loading application...</p>
    <style>
      {`
        @keyframes spin {
          0% { transform: rotate(0deg); }
          100% { transform: rotate(360deg); }
        }
      `}
    </style>
  </div>
);

const RemoteLoader: React.FC<{ children: React.ReactNode }> = ({ children }) => (
  <Suspense fallback={<RemoteLoadingSpinner />}>
    {children}
  </Suspense>
);
```

---

## Phase 6: Lazy Loading with License Validation

### 6.1 Implement License Validation Hook

typescript

```typescript
// hooks/useLicenseValidation.ts
interface License {
  appName: string;
  bought: boolean;
  expiresAt: string;
}

export const useLicenseValidation = () => {
  const [licenses, setLicenses] = useState<Record<string, License>>({
    'products-app': { appName: 'products-app', bought: true, expiresAt: '2024-12-31' },
    'orders-app': { appName: 'orders-app', bought: true, expiresAt: '2024-12-31' },
    'users-app': { appName: 'users-app', bought: false, expiresAt: '2024-01-31' }
  });

  const validateLicense = (appName: string): boolean => {
    const license = licenses[appName];
    if (!license) return false;
    
    const isExpired = new Date(license.expiresAt) < new Date();
    return license.bought && !isExpired;
  };

  return { licenses, setLicenses, validateLicense };
};
```

### 6.2 License Management Route

typescript

```typescript
// pages/LicenseManagement.tsx
import { useLicenseValidation } from "@/hooks/useLicenseValidation"

const LicenseManagement = () => {
  const { licenses, setLicenses } = useLicenseValidation();
  
  const toggleLicense = (appName: string) => {
    setLicenses(prev => ({
      ...prev,
      [appName]: {
        ...prev[appName],
        bought: !prev[appName].bought
      }
    }));
  };
  
  return (
    <div style={{ maxWidth: '800px', margin: '0 auto' }}>
      <h1 style={{ fontSize: '32px', fontWeight: 'bold', marginBottom: '30px' }}>
        License Management
      </h1>
      <div style={{ display: 'grid', gap: '20px' }}>
        {Object.values(licenses).map(license => {
          const isExpired = new Date(license.expiresAt) < new Date();
          const status = license.bought && !isExpired ? 'active' : 
                        !license.bought ? 'not-purchased' : 'expired';
          
          const getStatusColor = (status: string) => {
            switch (status) {
              case 'active': return '#28a745';
              case 'expired': return '#dc3545';
              default: return '#6c757d';
            }
          };
          
          return (
            <div 
              key={license.appName}
              style={{
                border: '1px solid #ddd',
                borderRadius: '8px',
                padding: '20px',
                backgroundColor: '#fff'
              }}
            >
              <div style={{ 
                display: 'flex', 
                justifyContent: 'space-between', 
                alignItems: 'center' 
              }}>
                <div>
                  <h3 style={{ 
                    margin: 0, 
                    fontSize: '18px', 
                    fontWeight: '600',
                    textTransform: 'capitalize'
                  }}>
                    {license.appName.replace('-', ' ')}
                  </h3>
                  <p style={{ margin: '5px 0 0 0', color: '#6c757d', fontSize: '14px' }}>
                    Expires: {license.expiresAt}
                  </p>
                </div>
                <div style={{ display: 'flex', alignItems: 'center', gap: '15px' }}>
                  <span style={{
                    padding: '4px 12px',
                    fontSize: '12px',
                    fontWeight: '500',
                    borderRadius: '12px',
                    backgroundColor: getStatusColor(status),
                    color: 'white'
                  }}>
                    {status === 'active' ? 'Active' : 
                     status === 'expired' ? 'Expired' : 'Not Purchased'}
                  </span>
                  <label style={{ display: 'flex', alignItems: 'center', cursor: 'pointer' }}>
                    <input 
                      type="checkbox"
                      checked={license.bought}
                      onChange={() => toggleLicense(license.appName)}
                      style={{ marginRight: '8px' }}
                    />
                    Purchased
                  </label>
                </div>
              </div>
            </div>
          );
        })}
      </div>
    </div>
  );
};
```

### 6.3 Conditional Remote Loading

typescript

```typescript
// components/ConditionalRemote.tsx
interface Props {
  appName: string;
  component: React.LazyExoticComponent<React.ComponentType<any>>;
  props: AppProps;
}

const LicenseExpiredFallback = ({ appName }: { appName: string }) => (
  <div style={{
    display: 'flex',
    flexDirection: 'column',
    alignItems: 'center',
    justifyContent: 'center',
    height: '200px',
    gap: '20px',
    textAlign: 'center'
  }}>
    <div>
      <h3 style={{ 
        fontSize: '20px', 
        fontWeight: '600', 
        color: '#dc3545',
        margin: '0 0 10px 0'
      }}>
        License Expired
      </h3>
      <p style={{ color: '#6c757d', margin: 0 }}>
        The license for {appName} has expired or is not purchased.
      </p>
    </div>
    <Link 
      to="/licenses"
      style={{
        padding: '10px 20px',
        backgroundColor: '#007bff',
        color: 'white',
        textDecoration: 'none',
        borderRadius: '4px',
        fontSize: '14px'
      }}
    >
      Manage Licenses
    </Link>
  </div>
);

const ConditionalRemote: React.FC<Props> = ({ appName, component: Component, props }) => {
  const { validateLicense } = useLicenseValidation();
  
  if (!validateLicense(appName)) {
    return <LicenseExpiredFallback appName={appName} />;
  }

  return (
    <RemoteErrorBoundary remoteName={appName}>
      <RemoteLoader>
        <Component {...props} />
      </RemoteLoader>
    </RemoteErrorBoundary>
  );
};
```

### 6.4 Update Host Router with Lazy Loading

typescript

```typescript
// Lazy load remotes
const ProductsApp = lazy(() => import('productsApp/App'));
const OrdersApp = lazy(() => import('ordersApp/App'));
const UsersApp = lazy(() => import('usersApp/App'));

// Updated Router
const AppRouter = () => {
  const { user } = useUser();
  
  return (
    <BrowserRouter>
      <Layout>
        <Routes>
          <Route path="/profile" element={<Profile user={user} />} />
          <Route path="/licenses" element={<LicenseManagement />} />
          <Route 
            path="/products/*" 
            element={
              <ConditionalRemote 
                appName="products-app"
                component={ProductsApp}
                props={{ user, basePath: "/products" }}
              />
            } 
          />
          {/* Similar for other remotes */}
        </Routes>
      </Layout>
    </BrowserRouter>
  );
};
```

---

## Phase 7: Environment Configuration for Dual-Mode Operation

### 7.1 Environment Variables Setup

**Each remote `.env.development` (standalone):**

bash

```bash
VITE_IS_STANDALONE=true
```

**Each remote `.env.production` (or when used in host):**

bash

```bash
VITE_IS_STANDALONE=false
```

### 7.2 Environment-Aware App Setup

typescript

```typescript
// main.tsx in each remote
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

const isStandalone = import.meta.env.VITE_IS_STANDALONE === 'true';

if (isStandalone) {
  // Standalone mode: render with router and left navigation
  const StandaloneApp = () => (
    <BrowserRouter>
      <RemoteLayout> {/* Includes left navigation */}
        <App basePath="" />
      </RemoteLayout>
    </BrowserRouter>
  );
  
  createRoot(document.getElementById('root')!).render(
    <StrictMode>
      <StandaloneApp />
    </StrictMode>
  );
}

// Export for federation (no router, no layout)
export default App;
```

### 7.3 Environment-Aware Navigation Links

typescript

```typescript
// components/ProductList.tsx
import { Link } from "react-router-dom"
import { useAppContext } from "@/contexts/AppContext"

const ProductList = () => {
  const { basePath = '' } = useAppContext();
  const isStandalone = import.meta.env.VITE_IS_STANDALONE === 'true';
  
  // Navigation adapts to mode
  const getNavigationPath = (path: string) => {
    if (isStandalone) {
      return path; // Direct path in standalone
    }
    return `${basePath}${path}`; // Prefixed path in federation
  };
  
  return (
    <div style={{ marginBottom: '20px' }}>
      <h2 style={{ fontSize: '24px', fontWeight: 'bold', marginBottom: '20px' }}>Products</h2>
      <div style={{ display: 'flex', gap: '10px' }}>
        <Link 
          to={getNavigationPath('/create')}
          style={{
            padding: '10px 15px',
            backgroundColor: '#007bff',
            color: 'white',
            textDecoration: 'none',
            borderRadius: '4px',
            fontSize: '14px'
          }}
        >
          Add Product
        </Link>
        <Link 
          to={getNavigationPath('/categories')}
          style={{
            padding: '10px 15px',
            backgroundColor: 'transparent',
            color: '#007bff',
            textDecoration: 'none',
            border: '1px solid #007bff',
            borderRadius: '4px',
            fontSize: '14px'
          }}
        >
          Categories
        </Link>
      </div>
      {/* Product list */}
    </div>
  );
};
```

---

## Phase 8: TypeScript Integration

### 8.1 Shared Interface Definitions

typescript

```typescript
// types/shared.ts (in each app)
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

**Note:** Vite's federation plugin handles most TypeScript integration automatically. The `vite-env.d.ts` file (auto-generated by Vite) already provides the necessary environment variable types.

---

## Phase 9: Production Build & Deployment

### 9.1 Simple Build All Applications

```bash
# From main repository root
pnpm build:all  # ✅ Builds remotes then host in correct order
```

**OR build individually:**

```bash
# Build each remote first
cd products-app && pnpm build
cd orders-app && pnpm build  
cd users-app && pnpm build

# Build host last  
cd host-app && pnpm build
```

### 9.2 Simple Deployment - Serve Built Applications

```bash
# From main repository root  
pnpm preview:all  # ✅ Serves all apps on their respective ports
```

**OR serve individually:**

```bash
# Terminal 1-3: Serve built remotes
cd products-app && pnpm preview  # Serves on port 5001
cd orders-app && pnpm preview    # Serves on port 5002  
cd users-app && pnpm preview     # Serves on port 5003

# Terminal 4: Serve host
cd host-app && pnpm preview      # Serves on port 5000
```

**Alternative: Use any static file server:**

```bash
# Using serve (pnpm install -g serve)  
cd products-app/dist && serve -p 5001
cd orders-app/dist && serve -p 5002
cd users-app/dist && serve -p 5003
cd host-app/dist && serve -p 5000

# OR using Python
cd products-app/dist && python -m http.server 5001
```

### 9.3 Git Submodules Management

**Cloning the main project:**

```bash
# Clone with submodules
git clone --recursive git@github.com:yourusername/microfrontend-federation-main

# OR if already cloned
git submodule update --init --recursive
```

**Updating submodules to latest:**

```bash
# From main repository root
pnpm submodules:update  # Updates all submodules to latest commits
```

**Making changes to submodules:**

```bash
# Work in individual submodule
cd products-app
# Make changes...
git add .
git commit -m "Update products feature"
git push

# Update main repo to point to new commit
cd ..
git add products-app
git commit -m "Update products-app submodule"
git push
```

**Common Submodule Issues & Solutions:**

```bash
# If you accidentally created folders first and need to remove them:
rm -rf products-app orders-app users-app host-app  # Remove directories
git submodule add ...  # Then add submodules

# If submodules appear as regular folders (not linked):
git rm -r products-app  # Remove from git (but keep files)
rm -rf products-app     # Remove directory
git submodule add git@github.com:yourusername/mf-products-app products-app
```

### 9.4 Production Vite Configuration

**Production considerations for remotes:**

```typescript
// vite.config.ts (each remote)
export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'products-app',
      filename: 'remoteEntry.js',
      exposes: {
        './App': './src/App.tsx',
      },
      shared: ['react', 'react-dom', 'react-router-dom']
    })
  ],
  build: {
    target: 'esnext',
    rollupOptions: {
      external: ['react', 'react-dom'] // Optimize bundle size
    }
  }
})
```

**Host production configuration:**

```typescript  
// vite.config.ts (host)
export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'host-app', 
      remotes: {
        // Update URLs for production deployment
        productsApp: process.env.VITE_PRODUCTS_URL || 'http://localhost:5001/assets/remoteEntry.js',
        ordersApp: process.env.VITE_ORDERS_URL || 'http://localhost:5002/assets/remoteEntry.js',
        usersApp: process.env.VITE_USERS_URL || 'http://localhost:5003/assets/remoteEntry.js'
      },
      shared: ['react', 'react-dom', 'react-router-dom']
    })
  ]
})
```

### 9.5 Deployment Summary

**Simple Production Deployment:**
1. **Clone project**: `git clone --recursive` to get all submodules
2. **Install dependencies**: `pnpm install:all` in main repo
3. **Build all apps**: `pnpm build:all`
4. **Deploy dist folders**: Upload each `dist` folder to your hosting service  
5. **Update remote URLs**: Set production URLs in host's vite config
6. **Access application**: Navigate to host URL

**Key Benefits of This Setup:**
- ✅ Each micro frontend is an independent repository (realistic team structure)
- ✅ Main repo orchestrates all apps with pnpm workspaces
- ✅ Git submodules allow version control of the entire system
- ✅ pnpm provides faster installs and better dependency management
- ✅ Each team can work independently on their micro frontend
- ✅ Simple deployment with orchestration scripts

---

## Presentation Flow Summary:

1. **Demo standalone apps** → Show **individual Git repos** + **left navigation** with simple CSS + **pnpm dev workflow**
2. **Create host + submodules** → Show **Git submodule setup** + **main repo orchestration** + **development reality check** (why `pnpm dev` doesn't work for federation)
3. **Fix routing & navigation** → Remove left nav from remotes, centralize router + **show root pnpm scripts** for federation development
4. **Add profile page** → Show host-specific features in dropdown menu at top-right
5. **Implement state sharing** → Props-based communication with **useContext** in remotes + **pnpm workspace benefits**
6. **Add error handling** → Error boundaries + loading states with simple styling
7. **License validation** → Lazy loading + business logic with **bought** and **expiry** validation
8. **Environment configuration** → **Dual-mode operation** (standalone with left nav vs federated)
9. **TypeScript integration** → Basic typing for federation
10. **Production deployment** → **Git submodules management** + **pnpm build/deploy scripts**

## Key Design Decisions:

- ✅ **Git submodules architecture** - Individual repos for each micro frontend (realistic team structure)
- ✅ **pnpm workspaces** - Better dependency management and faster installs across all apps
- ✅ **Root orchestration scripts** - Single commands to manage complex federation development workflow
- ✅ **Development workflow education** - Show why federation development is different from normal Vite
- ✅ **Live coding reality** - Demonstrate the `build + preview` requirement during federation setup
- ✅ **BasePath via props and context** for navigation (clean and accessible everywhere)
- ✅ **Left navigation in remotes initially** to contrast with host's top navigation  
- ✅ **Profile and license management in dropdown** at top-right for realistic app UX
- ✅ **useContext in remotes** for clean props access throughout components
- ✅ **License validation with bought + expiry** for realistic business logic
- ✅ **Simple CSS styling** to focus on micro frontend concepts, not UI complexity
- ✅ **Environment variables control dual-mode** without needing VITE_BASE_PATH
- ✅ **Git submodules for version control** - Realistic multi-team development approach

## Live Coding Teaching Moments:

1. **"Each team has their own repo!"** - Phase 1 individual repository setup
2. **"Let's orchestrate everything!"** - Phase 2 submodules + pnpm workspace setup  
3. **"Normal Vite works great standalone!"** - Individual app development experience
4. **"Wait, federation broke our dev workflow!"** - Reality check moment  
5. **"Ah, federation needs built files!"** - Understanding why `build + preview` is required
6. **"Root scripts save the day!"** - Show pnpm workspace orchestration benefits
7. **"Teams work independently, deploy together!"** - Git submodules benefits

This provides a complete journey showing **realistic team structure** (individual repos), **practical development workflow** (pnpm workspaces + orchestration), **UI/UX evolution** (navigation patterns), **technical architecture evolution** (from distributed to centralized routing), and **real-world deployment** (Git submodules management)!