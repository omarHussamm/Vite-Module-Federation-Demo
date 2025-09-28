# MicroFrontend Federation Demo

A complete **Module Federation** setup using **React 19**, **Vite**, **TypeScript**, and **pnpm workspaces**.

## 🏗️ Architecture

This monorepo contains 4 applications demonstrating micro frontend architecture:

- **🏠 Host App** (Port 5000) - Main orchestrator application
- **📦 Products App** (Port 5001) - Product management micro frontend  
- **🛒 Orders App** (Port 5002) - Order management micro frontend
- **👥 Users App** (Port 5003) - User management micro frontend

Each app can run **standalone** or **federated** through the host application.

## 🚀 Quick Start

### Initial Setup
```bash
# Install all dependencies across workspace
pnpm install
```

### Development Modes

#### 1. **Standalone Development** (Individual Apps)
```bash
# Run individual apps independently
pnpm -w run dev:products    # http://localhost:5001
pnpm -w run dev:orders      # http://localhost:5002  
pnpm -w run dev:users       # http://localhost:5003
pnpm -w run dev:host        # http://localhost:5000

# Or run all remotes together
pnpm -w run dev:remotes
```

#### 2. **Federation Development** (Complete System)
```bash
# Build & run complete federated system
pnpm -w run dev:federation

# This will:
# 1. Build all remote apps
# 2. Start remotes in preview mode (required for federation)
# 3. Start host in dev mode
```

### Production Build & Preview
```bash
# Build everything
pnpm -w run build

# Build only remotes
pnpm -w run build:remotes

# Preview built apps
pnpm -w run preview
pnpm -w run start:federation  # Preview complete federation
```

### Utility Commands
```bash
# Stop all services
pnpm -w run stop

# Clean build artifacts
pnpm -w run clean

# Clean all dependencies
pnpm -w run clean:deps

# Lint all apps
pnpm -w run lint

# Type check all apps
pnpm -w run type-check
```

## 📱 Access Points

When running the federated system:

- **🏠 Host Dashboard**: http://localhost:5000
- **📦 Products Standalone**: http://localhost:5001
- **🛒 Orders Standalone**: http://localhost:5002
- **👥 Users Standalone**: http://localhost:5003

## 🔧 Tech Stack

- **⚛️ React 19** - Latest React with concurrent features
- **⚡ Vite** - Lightning-fast build tool
- **📘 TypeScript** - Type safety across all apps
- **🔗 Module Federation** - `@originjs/vite-plugin-federation`
- **📦 pnpm Workspaces** - Monorepo management
- **🎨 Simple CSS** - Organized, maintainable styling

## 🏛️ Module Federation Setup

### Host Configuration
```typescript
// Host consumes remotes
remotes: {
  productsApp: 'http://localhost:5001/assets/remoteEntry.js',
  ordersApp: 'http://localhost:5002/assets/remoteEntry.js', 
  usersApp: 'http://localhost:5003/assets/remoteEntry.js',
}
```

### Remote Configuration  
```typescript
// Each remote exposes its main App
exposes: {
  './App': './src/App.tsx',
}
```

## 📚 Key Features Demonstrated

✅ **Dynamic Module Loading** - Remotes loaded on-demand  
✅ **Shared Dependencies** - Optimized React sharing  
✅ **Independent Development** - Each team works separately  
✅ **Centralized Routing** - Unified navigation experience  
✅ **Error Boundaries** - Graceful failure handling  
✅ **Type Safety** - TypeScript across federation boundary  
✅ **Hot Reload** - Real-time development updates  
✅ **Production Ready** - Complete build & deployment pipeline  

## 🛠️ Development Workflow

### For Federation Development:
1. **Build remotes**: `pnpm -w run build:remotes`
2. **Start remotes**: `pnpm -w run preview:remotes` 
3. **Start host**: `pnpm -w run dev:host`

Or use the one-command shortcut: `pnpm -w run dev:federation`

### For Individual App Development:
```bash
# Work on specific apps independently
pnpm -w run dev:products  # Full hot reload, normal Vite experience
```

### 💡 Important: Workspace Commands

All commands must use the `-w` flag to run scripts from the workspace root:

```bash
# ✅ Correct
pnpm -w run dev:federation

# ❌ Wrong - will fail
pnpm dev:federation
```

### 🔧 Command Syntax Explained

```bash
# pnpm workspace filtering:
--filter=app-name    # Full syntax (clear and readable)
-F app-name         # Short syntax (brief but less clear)

# We use --filter= for clarity in our scripts
pnpm --filter=mf-products-app dev
```

## 📖 Project Structure

```
├── mf-host-app/          # Main orchestrator application
├── mf-products-app/      # Products micro frontend
├── mf-orders-app/        # Orders micro frontend  
├── mf-users-app/         # Users micro frontend
├── package.json          # Root orchestration scripts
├── pnpm-workspace.yaml   # Workspace configuration
└── README.md             # This file
```

## 🎯 Live Coding Demo Ready!

This setup is perfect for live demonstrations of:
- **Phase 1**: Individual standalone apps
- **Phase 2**: Module Federation setup  
- **Phase 3**: Advanced federation features
- **Realistic team workflow** with independent repositories
- **Production deployment** strategies

Each phase can be demonstrated step-by-step, showing the evolution from standalone applications to a complete micro frontend federation system! 🚀
