# In Memoria VS Code Extension - Testing Guide

## 🎯 Understanding the Architecture

In Memoria has 3 components that work together:

```
┌─────────────────────┐
│   1. CLI Tool       │  ← Learns from source code files
│   (npx in-memoria)  │     Creates intelligence database
└─────────────────────┘
          ↓
┌─────────────────────┐
│  2. MCP Server      │  ← Serves intelligence via MCP protocol
│ (in-memoria server) │     Provides tools for AI assistants
└─────────────────────┘
          ↓
┌─────────────────────┐
│ 3. VS Code Extension│  ← Visualizes intelligence data
│  (This extension)   │     Connects to MCP server
└─────────────────────┘
```

## ❌ Common Mistake

You can't visualize data if there's nothing learned yet!

```bash
# ❌ WRONG: Learning an empty folder
npx in-memoria learn ./my-project  # Returns 0 files

# ✅ RIGHT: Learn an actual codebase with source files
npx in-memoria learn /path/to/real/project
```

## 📋 Prerequisites

In Memoria analyzes **source code files** using Tree-sitter. It supports:
- TypeScript/JavaScript
- Python
- PHP
- Rust, Go, Java
- C/C++, C#
- Svelte
- SQL

**You need an actual codebase with these files to learn from!**

## 🚀 Step-by-Step: Testing Locally

### Method 1: Test with the In Memoria Codebase Itself

1. **Navigate to the In Memoria root** (not vscode-extension):
   ```bash
   cd /home/user/In-Memoria
   ```

2. **Install In Memoria globally** (or use npx):
   ```bash
   npm install -g in-memoria
   # OR just use: npx in-memoria
   ```

3. **Learn the In Memoria codebase**:
   ```bash
   # This will analyze the In Memoria project's TypeScript/JavaScript files
   npx in-memoria learn .
   ```

   You should see output like:
   ```
   🧠 Starting intelligent learning from: .
   📁 Database path resolved to: /home/user/In-Memoria/in-memoria.db
   ✅ Learned 234 concepts from codebase
   ✅ Pattern learning completed: 87 patterns discovered
   🗺️ Features: 45
   📁 Files: 156
   ```

4. **Start the MCP server**:
   ```bash
   npx in-memoria server
   ```

   Should output:
   ```
   🚀 Starting In Memoria MCP Server
   ✅ Local embedding pipeline ready
   ```

5. **Open VS Code in the project**:
   ```bash
   code .
   ```

6. **Test the Extension** (press F5):
   - A new VS Code window opens with `[Extension Development Host]`
   - The extension auto-connects to the MCP server
   - Check the In Memoria sidebar to see the learned data

### Method 2: Test with Any Project

1. **Choose a project with source code**:
   ```bash
   # Example: A Next.js app, Python project, etc.
   cd ~/projects/my-nextjs-app
   ```

2. **Learn it**:
   ```bash
   npx in-memoria learn .
   ```

3. **Start the server in that directory**:
   ```bash
   npx in-memoria server
   ```

4. **Open VS Code there**:
   ```bash
   code .
   ```

5. **Launch the extension** (F5) and explore!

## 🔧 Testing the Extension Without Installing In Memoria

If you don't want to install In Memoria globally:

1. **Open only the vscode-extension folder**:
   ```bash
   cd /home/user/In-Memoria/vscode-extension
   code .
   ```

2. **Press F5** to launch Extension Development Host

3. **In the new window, configure the MCP server path**:
   - Settings → Extensions → In Memoria Visualizer
   - Set `inMemoria.serverCommand` to: `node /path/to/in-memoria/index.cjs server`

4. **In a terminal, start the server manually**:
   ```bash
   cd /home/user/In-Memoria
   node index.cjs server
   ```

## 🎨 What You'll See

Once In Memoria has learned a codebase, the extension will show:

### Tree Views
- **Project Intelligence**: Tech stack, entry points, architecture
- **Patterns**: High/medium/low confidence coding patterns
- **Work Session**: Current files, tasks, decisions
- **AI Insights**: Contributed insights from AI agents

### Commands
- `In Memoria: Show Intelligence Dashboard` - Overview with metrics
- `In Memoria: Show Relationship Graph` - Interactive concept graph
- `In Memoria: Show Pattern Explorer` - Deep dive into patterns
- `In Memoria: Show File Intelligence` - Analyze specific files
- `In Memoria: Show Feature Router` - Route features to files

## 🐛 Troubleshooting

### "Found 0 files" when learning

**Cause**: The directory doesn't have source code files In Memoria recognizes.

**Fix**:
```bash
# Check if there are actual source files
ls -la src/  # Look for .ts, .js, .py, .go, .rs, etc.

# Make sure you're in the right directory
pwd
```

### Extension shows "No data"

**Cause**: You haven't learned the codebase yet.

**Fix**:
1. Learn the codebase first: `npx in-memoria learn .`
2. Restart the MCP server: `npx in-memoria server`
3. Refresh the extension: Command Palette → `In Memoria: Refresh Intelligence`

### Server won't connect

**Cause**: MCP server isn't running or path is wrong.

**Fix**:
```bash
# Terminal 1: Start server
npx in-memoria server

# Terminal 2: Check it's running
# You should see: "✅ Local embedding pipeline ready"
```

## 🧪 Quick Test Checklist

- [ ] Navigate to a project with source code
- [ ] Run `npx in-memoria learn .`
- [ ] Verify it found files (check output)
- [ ] Run `npx in-memoria server`
- [ ] Open VS Code: `code .`
- [ ] Press F5 to launch Extension Development Host
- [ ] Check In Memoria sidebar for data
- [ ] Test commands from Command Palette

## 💡 Pro Tips

1. **Use In Memoria itself for testing** - It's a TypeScript/JavaScript project with good complexity
2. **Check the output** - The CLI tells you exactly how many files/concepts/patterns were found
3. **Start simple** - Test with a small project first (~50-100 files)
4. **Use auto-connect** - The extension auto-connects if `inMemoria.autoConnect` is true (default)

## 📚 Next Steps

Once the extension is working:
1. Test each command from the Command Palette
2. Verify the tree views show data correctly
3. Open the Dashboard and Relationship Graph panels
4. Try the Feature Router with natural language queries
5. Check File Intelligence for a specific source file

---

**Remember**: In Memoria learns from CODE, not from thin air! You need actual source files to analyze. 🎯
