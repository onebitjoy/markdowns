Using **`tsc`** (TypeScript Compiler) to build a Node.js backend application with TypeScript is straightforward. Below are the steps to set up and use `tsc` for your project.

---

### **Steps to Set Up Node.js Backend with TypeScript Using `tsc`**

#### 1. **Initialize Your Project**
```bash
mkdir my-node-backend
cd my-node-backend
npm init -y
```

This creates a `package.json` file to manage your project dependencies.

---

#### 2. **Install TypeScript and Node.js Types**
```bash
npm install --save-dev typescript @types/node
```

- `typescript`: Installs the TypeScript compiler (`tsc`).
- `@types/node`: Provides TypeScript definitions for Node.js.

---

#### 3. **Generate a TypeScript Configuration File**
Run:
```bash
npx tsc --init
```

This creates a `tsconfig.json` file. Update it to suit your Node.js backend project:
```json
{
  "compilerOptions": {
    "target": "ESNext",                 // Target modern JavaScript
    "module": "CommonJS",               // Use CommonJS for Node.js
    "outDir": "./dist",                 // Output folder for compiled files
    "rootDir": "./src",                 // Source files location
    "strict": true,                     // Enable all strict type-checking options
    "esModuleInterop": true,            // Allow default imports from CommonJS modules
    "skipLibCheck": true,               // Skip type-checking of declaration files
    "forceConsistentCasingInFileNames": true // Enforce consistent casing
  },
  "include": ["src/**/*.ts"],           // Include all TypeScript files in `src`
  "exclude": ["node_modules", "dist"]   // Exclude unnecessary files
}
```

---

#### 4. **Set Up Your Project Structure**
Create the following directory structure:
```
my-node-backend/
├── src/
│   ├── index.ts       // Entry point of your backend app
├── dist/              // Will contain compiled JavaScript files (generated)
├── package.json       // NPM dependencies and scripts
├── tsconfig.json      // TypeScript compiler configuration
├── .gitignore         // Ignored files (optional)
```

---

#### 5. **Write Your Backend Code**
In `src/index.ts`, write a simple HTTP server:
```typescript
import http from 'http';

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify({ message: 'Hello, TypeScript!' }));
});

server.listen(3000, () => {
  console.log('Server is running at http://localhost:3000');
});
```

---

#### 6. **Add Build and Start Scripts**
In your `package.json`, add the following scripts:
```json
{
  "scripts": {
    "build": "tsc",                   // Compiles TypeScript to JavaScript
    "start": "node dist/index.js",    // Runs the compiled JavaScript file
    "dev": "tsc --watch"              // Rebuilds files on changes (useful during development)
  }
}
```

---

#### 7. **Build and Run Your Project**
- To **build** the project (transpile TypeScript to JavaScript):
  ```bash
  npm run build
  ```
  This compiles all files in the `src/` directory and outputs them to the `dist/` folder.

- To **run** the compiled code:
  ```bash
  npm run start
  ```

- For **development mode** (automatic rebuilds on changes):
  ```bash
  npm run dev
  ```

---

### **Add `.gitignore` File**
Create a `.gitignore` file to ignore unnecessary files:
```plaintext
# Node modules
node_modules/

# Build output
dist/

# TypeScript build info
*.tsbuildinfo
```

---

### **Folder After Setup**
Your project should now look like this:
```
my-node-backend/
├── src/
│   ├── index.ts          // Your TypeScript code
├── dist/                 // Compiled JavaScript code (after build)
├── node_modules/         // Dependencies
├── tsconfig.json         // TypeScript configuration
├── package.json          // NPM scripts and dependencies
├── .gitignore            // Ignored files
```

---

### **Advantages of Using `tsc`:**
1. **Built-in Type Checking**:
   - `tsc` validates your code against TypeScript types, which is helpful for catching bugs early.
2. **Customizable Configuration**:
   - You can fine-tune `tsconfig.json` for different needs (e.g., output formats, strictness).
3. **No Additional Tools Required**:
   - Works directly with Node.js, no need for external bundlers like Webpack or Vite.

---