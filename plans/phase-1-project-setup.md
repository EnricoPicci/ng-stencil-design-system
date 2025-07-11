review# 📦 Phase 1: Project Setup
## Angular 19 + Stencil Design System - Foundation

**Duration**: 1-2 days  
**Objective**: Establish monorepo workspace structure with proper configurations  
**Dependencies**: None  
**Next Phase**: Phase 2 - Stencil Design System

---

## 🎯 Phase 1 Objectives

- ✅ Create monorepo workspace structure
- ✅ Configure package.json files for all packages
- ✅ Install and configure Stencil and Angular dependencies
- ✅ Set up TypeScript configurations
- ✅ Configure build scripts and development workflows
- ✅ Establish development environment

---

## 📁 Target Directory Structure

```
/workspaces/ng-stencil-design-system/
├── packages/
│   ├── design-system/              # Stencil UI Library
│   │   ├── src/
│   │   │   ├── components/
│   │   │   │   └── .gitkeep
│   │   │   ├── index.ts
│   │   │   └── utils/
│   │   ├── package.json
│   │   ├── stencil.config.ts
│   │   ├── tsconfig.json
│   │   └── .gitignore
│   └── todo-app/                   # Angular 19 Application
│       ├── src/
│       │   ├── app/
│       │   │   └── .gitkeep
│       │   ├── assets/
│       │   ├── main.ts
│       │   ├── index.html
│       │   └── styles.css
│       ├── package.json
│       ├── angular.json
│       ├── tsconfig.json
│       ├── tsconfig.app.json
│       └── .gitignore
├── package.json                    # Root workspace config
├── tsconfig.json                   # Root TypeScript config
├── .gitignore                      # Root gitignore
└── README.md                       # Updated with setup instructions
```

---

## 🛠️ Implementation Steps

### Step 1: Angular Application Setup with CLI

#### 1.1 Create Angular Application with Standalone Components
```bash
# Create Angular 19 app with standalone components (recommended approach)
npx @angular/cli@19 new todo-app \
  --routing=true \
  --style=scss \
  --standalone=true \
  --skip-git=true \
  --package-manager=npm \
  --directory=packages/todo-app

# Verify Angular version and configuration
cd packages/todo-app
ng version
```

#### 1.2 Root Workspace Configuration
```json
{
  "name": "ng-stencil-design-system",
  "version": "1.0.0",
  "description": "Angular 19 + Stencil Design System Todo Application",
  "private": true,
  "workspaces": [
    "packages/*"
  ],
  "scripts": {
    "build": "npm run build --workspaces --if-present",
    "build:design-system": "npm run build -w packages/design-system",
    "build:todo-app": "npm run build -w packages/todo-app",
    "dev": "npm run dev --workspaces --if-present",
    "dev:design-system": "npm run start -w packages/design-system",
    "dev:todo-app": "npm run start -w packages/todo-app",
    "test": "npm run test --workspaces --if-present",
    "lint": "npm run lint --workspaces --if-present",
    "clean": "npm run clean --workspaces --if-present && rm -rf node_modules",
    "install:all": "npm install && npm install --workspaces"
  },
  "devDependencies": {
    "@angular/cli": "^19.0.0",
    "@types/node": "^20.11.0",
    "typescript": "^5.3.0",
    "prettier": "^3.2.0",
    "eslint": "^8.56.0"
  },
  "engines": {
    "node": ">=18.0.0",
    "npm": ">=9.0.0"
  }
}
```

#### 1.2 Root tsconfig.json
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "files": [],
  "references": [
    {
      "path": "./packages/design-system"
    },
    {
      "path": "./packages/todo-app"
    }
  ]
}
```

#### 1.3 Root .gitignore
```gitignore
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Build outputs
dist/
build/
*.tsbuildinfo

# IDEs
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Environment
.env
.env.local
.env.production

# Logs
logs/
*.log

# Runtime data
pids/
*.pid
*.seed
*.pid.lock

# Coverage directory used by tools like istanbul
coverage/
*.lcov

# Temporary folders
tmp/
temp/
```

### Step 2: Stencil Design System Package

#### 2.1 Design System package.json
```json
{
  "name": "@todo-design-system/core",
  "version": "1.0.0",
  "description": "Stencil design system components for todo application",
  "main": "dist/index.cjs.js",
  "module": "dist/index.js",
  "types": "dist/types/index.d.ts",
  "collection": "dist/collection/collection-manifest.json",
  "collection:main": "dist/collection/index.js",
  "unpkg": "dist/todo-design-system/todo-design-system.esm.js",
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs.js",
      "types": "./dist/types/index.d.ts"
    },
    "./loader": {
      "import": "./loader/index.js",
      "require": "./loader/index.cjs.js",
      "types": "./loader/index.d.ts"
    }
  },
  "files": [
    "dist/",
    "loader/"
  ],
  "scripts": {
    "build": "stencil build",
    "start": "stencil build --dev --watch --serve",
    "test": "stencil test --spec",
    "test:watch": "stencil test --spec --watchAll",
    "test:e2e": "stencil test --e2e",
    "generate": "stencil generate",
    "lint": "eslint src/**/*.{ts,tsx}",
    "clean": "rm -rf dist loader"
  },
  "dependencies": {
    "@stencil/core": "^4.17.0"
  },
  "devDependencies": {
    "@stencil/angular-output-target": "^0.8.0",
    "@stencil/eslint-plugin": "^0.4.0",
    "@stencil/sass": "^3.0.0",
    "@types/jest": "^29.5.0",
    "@typescript-eslint/eslint-plugin": "^7.0.0",
    "@typescript-eslint/parser": "^7.0.0",
    "eslint": "^8.56.0",
    "jest": "^29.5.0",
    "jest-cli": "^29.5.0",
    "puppeteer": "^21.0.0",
    "typescript": "^5.3.0"
  },
  "license": "MIT"
}
```

#### 2.2 Stencil Configuration (stencil.config.ts)
```typescript
import { Config } from '@stencil/core';
import { angularOutputTarget } from '@stencil/angular-output-target';
import { sass } from '@stencil/sass';

export const config: Config = {
  namespace: 'todo-design-system',
  taskQueue: 'async',
  sourceMap: true,
  plugins: [
    sass({
      injectGlobalPaths: ['src/styles/globals.scss']
    })
  ],
  outputTargets: [
    // Custom Elements Bundle for Angular
    {
      type: 'dist-custom-elements',
      customElementsExportBehavior: 'auto-define-custom-elements',
      externalRuntime: false,
      generateTypeDeclarations: true,
    },
    // Distribution build
    {
      type: 'dist',
      esmLoaderPath: '../loader',
    },
    // Angular Output Target
    angularOutputTarget({
      componentCorePackage: '@todo-design-system/core',
      outputType: 'standalone',
      directivesProxyFile: '../todo-app/src/app/stencil-generated/components.ts',
      directivesArrayFile: '../todo-app/src/app/stencil-generated/index.ts',
    }),
    // Documentation
    {
      type: 'docs-readme',
    },
    // Web Types for IDE support
    {
      type: 'dist-types',
      typesDir: 'dist/types'
    }
  ],
  testing: {
    browserHeadless: "new",
    collectCoverageFrom: [
      'src/**/*.{ts,tsx}',
      '!src/**/*.d.ts',
      '!src/**/*.e2e.ts',
      '!src/**/*.spec.ts'
    ],
    coverageThreshold: {
      global: {
        branches: 80,
        functions: 80,
        lines: 80,
        statements: 80
      }
    }
  }
};
```

#### 2.3 Design System tsconfig.json
```json
{
  "extends": "../../tsconfig.json",
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
    "allowSyntheticDefaultImports": true,
    "allowUnreachableCode": false,
    "declaration": true,
    "experimentalDecorators": true,
    "lib": ["DOM", "ES2017"],
    "moduleResolution": "node",
    "module": "esnext",
    "target": "es2017",
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "jsx": "react",
    "jsxFactory": "h"
  },
  "include": [
    "src",
    "types/jsx.d.ts"
  ],
  "exclude": [
    "dist",
    "loader"
  ]
}
```

#### 2.4 Design System src/index.ts
```typescript
// Export all components for consumption
export { Components, JSX } from './components';

// Export utilities if any
export * from './utils';
```

### Step 3: Angular Application Customization

Since we used `ng new` with the `--standalone` flag, most configuration is already set up correctly. We just need to make a few customizations:

#### 3.1 Update Angular package.json for Workspace Integration
```bash
# Navigate to Angular app
cd packages/todo-app

# Add Stencil design system dependency
npm install --save @todo-design-system/core@workspace:*
```

#### 3.2 Update Angular angular.json for Stencil Integration
Add the following to the "build" -> "options" section in `packages/todo-app/angular.json`:

```json
{
  "allowedCommonJsDependencies": [
    "@todo-design-system/core"
  ]
}
```

#### 3.3 Update main.ts for Stencil Integration (Preparation)
The Angular CLI created `packages/todo-app/src/main.ts`. Update it to prepare for Stencil integration:

```typescript
import { bootstrapApplication } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';

// TODO: Import and register Stencil components in Phase 3
// import { defineCustomElements } from '@todo-design-system/core/loader';
// defineCustomElements();

bootstrapApplication(AppComponent, {
  providers: [
    // TODO: Add providers in Phase 3
  ]
}).catch(err => console.error(err));
```

#### 3.4 Update index.html with Custom Meta Tags
Update `packages/todo-app/src/index.html`:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Todo App - Angular 19 + Stencil Design System</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="description" content="A modern todo application built with Angular 19 and Stencil design system components">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
  
  <!-- Preconnect for performance -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  
  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
</head>
<body>
  <app-root></app-root>
</body>
</html>
```

#### 3.5 Update Global Styles
Update `packages/todo-app/src/styles.scss`:

```scss
/* Global Styles */
:root {
  /* Design System Variables - Will be populated in Phase 2 */
  --font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

/* Reset */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: var(--font-family);
  line-height: 1.6;
  color: #333;
  background-color: #f9fafb;
}

/* TODO: Import design system styles in Phase 2 */
```

---

## ✅ Success Criteria

### Phase 1 Complete When:
- [ ] **Workspace Structure**: All directories and files created correctly
- [ ] **Dependencies**: All npm packages install without errors
- [ ] **Build Scripts**: Root workspace scripts execute without errors
- [ ] **TypeScript**: All tsconfig files validate successfully
- [ ] **Stencil**: `npm run start -w packages/design-system` runs without errors
- [ ] **Angular**: `npm run start -w packages/todo-app` runs without errors (placeholder app)

### Validation Commands:
```bash
# Test workspace setup
npm install
npm run build

# Test individual packages
npm run dev:design-system
npm run dev:todo-app

# Verify TypeScript configuration
npx tsc --noEmit --project packages/design-system
npx tsc --noEmit --project packages/todo-app
```

---

## 🔧 Installation & Setup Commands

```bash
# 1. Initialize root workspace
npm init -y
# Update package.json with workspaces configuration

# 2. Create Angular application with CLI (recommended approach)
npx @angular/cli@19 new todo-app \
  --routing=true \
  --style=scss \
  --standalone=true \
  --skip-git=true \
  --package-manager=npm \
  --directory=packages/todo-app

# 3. Create Stencil design system package
mkdir -p packages/design-system/src/{components,styles,utils}
cd packages/design-system
npm init -y
npm install @stencil/core @stencil/angular-output-target

# 4. Install workspace dependencies
cd ../../  # Back to root
npm install

# 5. Verify setup
npm run build
npm run dev:design-system &  # Stencil dev server (port 3333)
npm run dev:todo-app &       # Angular dev server (port 4200)

# 6. Test applications
curl http://localhost:3333   # Stencil dev server
curl http://localhost:4200   # Angular dev server
```

---

## 🚨 Common Issues & Solutions

### Issue: npm workspace not recognized
**Solution**: Ensure Node.js 16+ and npm 7+ are installed
```bash
node --version  # Should be 18+
npm --version   # Should be 9+
```

### Issue: TypeScript compilation errors
**Solution**: Verify all tsconfig.json files are valid JSON
```bash
npx tsc --noEmit --project .
```

### Issue: Angular CLI not found
**Solution**: Install Angular CLI globally or use npx
```bash
npm install -g @angular/cli@19
# Or use: npx ng --version
```

### Issue: Stencil build fails
**Solution**: Ensure @stencil/core version compatibility
```bash
npm list @stencil/core
npm update @stencil/core
```

---

## 📋 Phase 1 Checklist

- [ ] Initialize root workspace with npm workspaces
- [ ] **Create Angular application using Angular CLI** (`ng new` with `--standalone`)
- [ ] Configure Angular app for workspace integration
- [ ] Create Stencil design system package structure
- [ ] Configure Stencil build and development setup
- [ ] Set up root TypeScript configuration and workspace scripts
- [ ] Install all workspace dependencies
- [ ] Verify Angular dev server runs (`ng serve`)
- [ ] Verify Stencil dev server runs (`stencil build --dev --watch --serve`)
- [ ] Test workspace build scripts work correctly
- [ ] Commit initial workspace setup

**Key Advantage**: Using `ng new --standalone` automatically sets up:
✅ Proper Angular 19 project structure  
✅ Standalone components by default  
✅ Correct TypeScript configurations  
✅ Testing setup (Jasmine + Karma)  
✅ Build configurations  
✅ ESLint and other tooling

---

## 🎯 Next Steps

Once Phase 1 is complete:
1. **Proceed to Phase 2**: Stencil Design System Development
2. **Verify Integration**: Ensure Angular can import Stencil components
3. **Test Development Workflow**: Both dev servers should run concurrently

**Phase 2 Prerequisites Met:**
✅ Stencil workspace ready for component development  
✅ Angular workspace ready for Stencil integration  
✅ Build pipeline established  
✅ Development servers operational