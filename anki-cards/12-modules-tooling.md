# JavaScript Modules & Development Tooling - Anki Cards

## JavaScript Modules

### What are JavaScript Modules?
**Q:** What are JavaScript modules and why are they important?

**A:** JavaScript modules are reusable pieces of code that can be exported from one file and imported into another. They help organize code, prevent global namespace pollution, and enable code reusability.

**Benefits:**
- **Encapsulation**: Keep code organized and private
- **Reusability**: Share code across different files
- **Maintainability**: Easier to debug and update
- **Dependency Management**: Clear dependencies between files
- **Namespace Protection**: Avoid variable name conflicts

```javascript
// Without modules - global namespace pollution
var userName = "John";
var userAge = 25;

// With modules - encapsulated
// user.js
export const user = {
  name: "John",
  age: 25
};
```

### ES6 Module Syntax - Export
**Q:** How do you export variables, functions, and classes in ES6 modules?

**A:** ES6 provides multiple ways to export code from modules:

**Named Exports:**
```javascript
// math.js
export const PI = 3.14159;
export const E = 2.71828;

export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

export class Calculator {
  add(a, b) { return a + b; }
  subtract(a, b) { return a - b; }
}

// Export after declaration
const subtract = (a, b) => a - b;
const divide = (a, b) => a / b;
export { subtract, divide };

// Export with alias
const power = (base, exp) => Math.pow(base, exp);
export { power as pow };
```

**Default Exports:**
```javascript
// calculator.js - one default export per module
export default class Calculator {
  add(a, b) { return a + b; }
  subtract(a, b) { return a - b; }
}

// Or
class Calculator {
  // ... methods
}
export default Calculator;

// For functions
export default function greet(name) {
  return `Hello, ${name}!`;
}

// For values
export default {
  name: "MyApp",
  version: "1.0.0"
};
```

### ES6 Module Syntax - Import
**Q:** How do you import modules in ES6 JavaScript?

**A:** ES6 provides various import syntaxes for different use cases:

**Named Imports:**
```javascript
// Import specific exports
import { add, multiply, PI } from './math.js';
console.log(add(5, 3)); // 8
console.log(PI); // 3.14159

// Import with alias
import { pow as power } from './math.js';
console.log(power(2, 3)); // 8

// Import multiple with aliases
import { 
  add as sum, 
  multiply as product,
  Calculator as Calc 
} from './math.js';

// Import all named exports
import * as MathUtils from './math.js';
console.log(MathUtils.add(5, 3)); // 8
console.log(MathUtils.PI); // 3.14159
```

**Default Imports:**
```javascript
// Import default export
import Calculator from './calculator.js';
const calc = new Calculator();

import greet from './greetings.js';
console.log(greet("John")); // Hello, John!

// You can name default imports anything
import MyCalculator from './calculator.js';
import sayHello from './greetings.js';
```

**Mixed Imports:**
```javascript
// Import both default and named exports
import Calculator, { PI, add } from './math.js';

// Import default with namespace
import Calculator, * as MathUtils from './math.js';
```

### Dynamic Imports
**Q:** What are dynamic imports and how do you use them?

**A:** Dynamic imports allow you to import modules conditionally and asynchronously at runtime, returning a Promise.

```javascript
// Basic dynamic import
async function loadMath() {
  const mathModule = await import('./math.js');
  console.log(mathModule.add(5, 3)); // 8
}

// Conditional loading
async function loadFeature(featureName) {
  if (featureName === 'advanced') {
    const { AdvancedCalculator } = await import('./advanced-calc.js');
    return new AdvancedCalculator();
  } else {
    const { default: BasicCalculator } = await import('./basic-calc.js');
    return new BasicCalculator();
  }
}

// Error handling with dynamic imports
async function safeImport() {
  try {
    const module = await import('./optional-feature.js');
    module.initialize();
  } catch (error) {
    console.log('Optional feature not available:', error.message);
  }
}

// Using .then() syntax
import('./math.js')
  .then(mathModule => {
    console.log(mathModule.PI);
  })
  .catch(error => {
    console.error('Failed to load module:', error);
  });

// Lazy loading based on user interaction
document.getElementById('loadButton').addEventListener('click', async () => {
  const { heavyFeature } = await import('./heavy-feature.js');
  heavyFeature.init();
});
```

### CommonJS Modules (Node.js)
**Q:** How do CommonJS modules work in Node.js?

**A:** CommonJS is the module system used in Node.js before ES6 modules were widely supported.

**Exporting:**
```javascript
// math.js
const PI = 3.14159;

function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

class Calculator {
  add(a, b) { return a + b; }
}

// Multiple exports
module.exports = {
  PI,
  add,
  multiply,
  Calculator
};

// Or export individually
exports.PI = PI;
exports.add = add;
exports.multiply = multiply;

// Single export
module.exports = Calculator;

// Mixed approach
module.exports = Calculator;
module.exports.PI = PI;
module.exports.add = add;
```

**Importing:**
```javascript
// Import entire module
const math = require('./math.js');
console.log(math.add(5, 3));
console.log(math.PI);

// Destructuring import
const { add, multiply, PI } = require('./math.js');
console.log(add(5, 3));

// Import default export
const Calculator = require('./calculator.js');
const calc = new Calculator();

// Require built-in modules
const fs = require('fs');
const path = require('path');
const http = require('http');

// Require npm packages
const express = require('express');
const lodash = require('lodash');
```

### Module Resolution
**Q:** How does JavaScript resolve module paths?

**A:** Module resolution determines how import/require statements find the actual files.

**Relative Paths:**
```javascript
// Same directory
import utils from './utils.js';
import { helper } from './helper.js';

// Parent directory
import config from '../config.js';

// Nested directories
import validator from './validators/email.js';
import { authenticator } from '../auth/index.js';
```

**Absolute Paths (Node.js):**
```javascript
// From node_modules
import express from 'express';
import _ from 'lodash';

// Built-in modules
import fs from 'fs';
import path from 'path';

// Package with path
import { Router } from 'express/lib/router/index.js';
```

**Resolution Algorithm:**
1. **Exact match**: `./file.js` looks for `file.js`
2. **Add extensions**: `./file` tries `file.js`, `file.json`, `file.node`
3. **Index files**: `./folder` tries `folder/index.js`, `folder/package.json`
4. **Node modules**: `package` searches in `node_modules` directories up the tree

```javascript
// package.json main field
{
  "name": "my-package",
  "main": "lib/index.js",
  "module": "es/index.js",
  "exports": {
    ".": {
      "import": "./es/index.js",
      "require": "./lib/index.js"
    },
    "./utils": "./lib/utils.js"
  }
}
```

## Development Tooling

### Package Managers (npm, yarn, pnpm)
**Q:** What are JavaScript package managers and how do you use them?

**A:** Package managers help install, update, and manage JavaScript dependencies.

**npm (Node Package Manager):**
```bash
# Initialize project
npm init
npm init -y  # Skip questions

# Install dependencies
npm install express
npm install lodash --save-dev  # Dev dependency
npm install -g nodemon  # Global install

# Install from package.json
npm install
npm ci  # Clean install for CI/CD

# Update packages
npm update
npm outdated  # Check for updates

# Remove packages
npm uninstall express
npm prune  # Remove unused packages

# Run scripts
npm run build
npm start
npm test
```

**Yarn:**
```bash
# Initialize project
yarn init

# Install dependencies
yarn add express
yarn add lodash --dev
yarn global add nodemon

# Install from yarn.lock
yarn install
yarn install --frozen-lockfile

# Update packages
yarn upgrade
yarn outdated

# Remove packages
yarn remove express

# Run scripts
yarn build
yarn start
yarn test
```

**package.json Example:**
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My JavaScript application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "build": "webpack --mode=production",
    "test": "jest",
    "lint": "eslint src/",
    "format": "prettier --write src/"
  },
  "dependencies": {
    "express": "^4.18.0",
    "lodash": "^4.17.21"
  },
  "devDependencies": {
    "nodemon": "^2.0.15",
    "jest": "^28.0.0",
    "webpack": "^5.70.0"
  }
}
```

### Build Tools (Webpack, Vite, Parcel)
**Q:** What are JavaScript build tools and when would you use them?

**A:** Build tools process, bundle, and optimize JavaScript code for production.

**Webpack Configuration:**
```javascript
// webpack.config.js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
    clean: true
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },
      {
        test: /\.(png|jpg|gif)$/,
        type: 'asset/resource'
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  devServer: {
    contentBase: './dist',
    hot: true
  }
};
```

**Vite Configuration:**
```javascript
// vite.config.js
import { defineConfig } from 'vite';

export default defineConfig({
  root: './src',
  build: {
    outDir: '../dist',
    rollupOptions: {
      input: {
        main: './src/index.html',
        admin: './src/admin.html'
      }
    }
  },
  server: {
    port: 3000,
    hot: true
  }
});
```

### Transpilation (Babel)
**Q:** What is Babel and why is it used in JavaScript development?

**A:** Babel is a transpiler that converts modern JavaScript code into backwards-compatible versions for older browsers.

**Babel Configuration:**
```javascript
// babel.config.js
module.exports = {
  presets: [
    ['@babel/preset-env', {
      targets: {
        browsers: ['> 1%', 'last 2 versions']
      },
      useBuiltIns: 'usage',
      corejs: 3
    }]
  ],
  plugins: [
    '@babel/plugin-proposal-optional-chaining',
    '@babel/plugin-proposal-nullish-coalescing-operator'
  ]
};
```

**Before Transpilation (ES6+):**
```javascript
// Modern JavaScript
const greet = (name = 'World') => {
  return `Hello, ${name}!`;
};

class User {
  constructor(name) {
    this.name = name;
  }
  
  async fetchData() {
    const response = await fetch('/api/user');
    return response?.json?.() ?? {};
  }
}

const numbers = [1, 2, 3];
const doubled = numbers.map(n => n * 2);
```

**After Transpilation (ES5):**
```javascript
// Transpiled to ES5
"use strict";

function greet() {
  var name = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : 'World';
  return "Hello, " + name + "!";
}

var User = function() {
  function User(name) {
    this.name = name;
  }
  
  User.prototype.fetchData = function fetchData() {
    return regeneratorRuntime.async(function fetchData$(_context) {
      while (1) {
        switch (_context.prev = _context.next) {
          case 0:
            _context.next = 2;
            return regeneratorRuntime.awrap(fetch('/api/user'));
          case 2:
            response = _context.sent;
            return _context.abrupt("return", response && response.json && response.json() || {});
        }
      }
    });
  };
  
  return User;
}();
```

### Linting (ESLint)
**Q:** What is ESLint and how do you configure it?

**A:** ESLint is a static analysis tool that identifies and fixes problems in JavaScript code.

**ESLint Configuration:**
```javascript
// .eslintrc.js
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true
  },
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended'
  ],
  parserOptions: {
    ecmaVersion: 12,
    sourceType: 'module'
  },
  rules: {
    'indent': ['error', 2],
    'linebreak-style': ['error', 'unix'],
    'quotes': ['error', 'single'],
    'semi': ['error', 'always'],
    'no-unused-vars': 'error',
    'no-console': 'warn',
    'prefer-const': 'error',
    'no-var': 'error'
  },
  ignorePatterns: ['dist/', 'node_modules/']
};
```

**Common ESLint Rules:**
```javascript
// Good practices enforced by ESLint
'use strict';

// prefer-const rule
const name = 'John'; // ✓ Good
let age = 25; // ✓ Good (value changes)
// var name = 'John'; // ✗ Bad (no-var rule)

// no-unused-vars rule
function greet(name) {
  return `Hello, ${name}!`; // ✓ Good
  // const unused = 'test'; // ✗ Bad (unused variable)
}

// quotes rule
const message = 'Hello World'; // ✓ Good (single quotes)
// const message = "Hello World"; // ✗ Bad (double quotes not allowed)

// semi rule
const result = add(5, 3); // ✓ Good (semicolon required)
// const result = add(5, 3) // ✗ Bad (missing semicolon)
```

### Code Formatting (Prettier)
**Q:** What is Prettier and how does it work with ESLint?

**A:** Prettier is an opinionated code formatter that automatically formats code for consistency.

**Prettier Configuration:**
```json
// .prettierrc
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "avoid"
}
```

**Before Prettier:**
```javascript
// Inconsistent formatting
function    greet(name,age){
return   `Hello ${name}, you are ${age} years old`
}

const numbers=[1,2,3,4,5]
const doubled=numbers.map(n=>n*2)

const user={name:"John",age:25,city:"New York"}
```

**After Prettier:**
```javascript
// Consistent formatting
function greet(name, age) {
  return `Hello ${name}, you are ${age} years old`;
}

const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);

const user = {
  name: 'John',
  age: 25,
  city: 'New York',
};
```

**Integration with ESLint:**
```javascript
// .eslintrc.js
module.exports = {
  extends: [
    'eslint:recommended',
    'prettier' // Disables conflicting ESLint rules
  ],
  plugins: ['prettier'],
  rules: {
    'prettier/prettier': 'error' // Show Prettier issues as ESLint errors
  }
};
```

### Testing Tools (Jest, Mocha, Cypress)
**Q:** What are the main types of JavaScript testing tools and frameworks?

**A:** JavaScript testing tools fall into categories: unit testing, integration testing, and end-to-end testing.

**Jest (Unit Testing):**
```javascript
// math.test.js
const { add, multiply, divide } = require('./math');

describe('Math utilities', () => {
  test('adds 1 + 2 to equal 3', () => {
    expect(add(1, 2)).toBe(3);
  });

  test('multiplies 3 × 4 to equal 12', () => {
    expect(multiply(3, 4)).toBe(12);
  });

  test('throws error when dividing by zero', () => {
    expect(() => divide(10, 0)).toThrow('Cannot divide by zero');
  });

  test('handles async operations', async () => {
    const user = await fetchUser(1);
    expect(user).toHaveProperty('name');
    expect(user.name).toBe('John Doe');
  });
});

// Mock functions
const mockCallback = jest.fn();
mockCallback.mockReturnValue(42);
expect(mockCallback()).toBe(42);

// Spy on methods
const spy = jest.spyOn(console, 'log');
console.log('test');
expect(spy).toHaveBeenCalledWith('test');
```

**Mocha with Chai:**
```javascript
// test/math.spec.js
const { expect } = require('chai');
const { add, multiply } = require('../math');

describe('Math utilities', function() {
  it('should add two numbers correctly', function() {
    expect(add(2, 3)).to.equal(5);
  });

  it('should handle async operations', async function() {
    const result = await asyncOperation();
    expect(result).to.be.an('object');
    expect(result).to.have.property('success', true);
  });

  beforeEach(function() {
    // Setup before each test
  });

  afterEach(function() {
    // Cleanup after each test
  });
});
```

**Cypress (E2E Testing):**
```javascript
// cypress/integration/app.spec.js
describe('App E2E Tests', () => {
  beforeEach(() => {
    cy.visit('http://localhost:3000');
  });

  it('should display welcome message', () => {
    cy.contains('Welcome to our app');
    cy.get('[data-testid="welcome-message"]')
      .should('be.visible')
      .and('contain', 'Welcome');
  });

  it('should handle user login', () => {
    cy.get('[data-testid="email-input"]').type('user@example.com');
    cy.get('[data-testid="password-input"]').type('password123');
    cy.get('[data-testid="login-button"]').click();
    
    cy.url().should('include', '/dashboard');
    cy.get('[data-testid="user-profile"]').should('be.visible');
  });

  it('should handle form submission', () => {
    cy.intercept('POST', '/api/users', { fixture: 'user.json' }).as('createUser');
    
    cy.get('[data-testid="name-input"]').type('John Doe');
    cy.get('[data-testid="submit-button"]').click();
    
    cy.wait('@createUser');
    cy.get('[data-testid="success-message"]').should('be.visible');
  });
});
```

### Development Servers and Hot Reloading
**Q:** What are development servers and how does hot reloading work?

**A:** Development servers provide a local environment for testing applications with features like hot reloading, which updates the browser automatically when code changes.

**Webpack Dev Server:**
```javascript
// webpack.config.js
module.exports = {
  // ... other config
  devServer: {
    contentBase: './dist',
    hot: true, // Enable hot module replacement
    port: 3000,
    open: true, // Open browser automatically
    historyApiFallback: true, // For SPA routing
    proxy: {
      '/api': 'http://localhost:8080' // Proxy API calls
    }
  }
};

// In your JavaScript
if (module.hot) {
  module.hot.accept('./module.js', function() {
    // Handle hot update
    console.log('Module updated!');
  });
}
```

**Live Server (Simple HTML/JS):**
```bash
# Install globally
npm install -g live-server

# Run in project directory
live-server --port=3000 --open=index.html
```

**Node.js with Nodemon:**
```bash
# Install nodemon
npm install -g nodemon

# Run with auto-restart
nodemon server.js

# Or in package.json
{
  "scripts": {
    "dev": "nodemon server.js",
    "start": "node server.js"
  }
}
```

**Express with Hot Reloading:**
```javascript
// server.js
const express = require('express');
const app = express();

app.use(express.static('public'));

app.get('/api/users', (req, res) => {
  res.json({ users: ['John', 'Jane'] });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

// nodemon.json configuration
{
  "watch": ["server.js", "routes/", "models/"],
  "ext": "js,json",
  "ignore": ["node_modules/", "public/"],
  "delay": 1000
}
```

### Version Control Integration
**Q:** How do JavaScript tools integrate with Git and version control?

**A:** JavaScript development tools integrate with Git for automated workflows, code quality checks, and deployment.

**Git Hooks with Husky:**
```javascript
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "pre-push": "npm test",
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  },
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write",
      "git add"
    ],
    "*.{json,css,md}": [
      "prettier --write",
      "git add"
    ]
  }
}
```

**.gitignore for JavaScript Projects:**
```gitignore
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Build outputs
dist/
build/
.next/
.nuxt/

# Environment variables
.env
.env.local
.env.production

# IDE files
.vscode/
.idea/
*.swp
*.swo

# OS files
.DS_Store
Thumbs.db

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
.nyc_output/

# Cache directories
.cache/
.parcel-cache/
```

**GitHub Actions Workflow:**
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [14.x, 16.x, 18.x]
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    - run: npm ci
    - run: npm run lint
    - run: npm test
    - run: npm run build
    
    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v1
```
