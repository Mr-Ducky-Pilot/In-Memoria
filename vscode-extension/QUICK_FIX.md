# QUICK FIX: Extension Not Working

## ❌ What You're Probably Doing Wrong

You're likely trying to:
- Install the extension as a .vsix file
- Or just opening VS Code in a project folder
- The commands aren't registered because the extension isn't running in development mode

## ✅ Correct Way to Test the Extension

### Step 1: Open the Extension Source in VS Code

```bash
# Navigate to the extension source folder
cd /home/user/In-Memoria/vscode-extension

# Open VS Code HERE (in the extension folder, not your project)
code .
```

**Important:** You must open the `vscode-extension` folder itself, not your project!

### Step 2: Press F5 to Launch Extension Development Host

1. In VS Code (with vscode-extension folder open)
2. Press `F5` (or go to Run → Start Debugging)
3. This will open a **NEW VS Code window** with title `[Extension Development Host]`
4. The extension will ONLY work in that new window

### Step 3: Open Your Project in the Extension Development Host

In the **NEW window** (Extension Development Host):
```
File → Open Folder → Select your project folder
```

### Step 4: Check the Extension is Active

In the Extension Development Host window:
1. Look for the In Memoria icon in the Activity Bar (left sidebar)
2. Check the status bar (bottom right) - should show "✓ In Memoria: Connected"
3. Try Command Palette (Ctrl+Shift+P) → Type "In Memoria"

## 🔍 Debugging

### Check Console Logs

**In the ORIGINAL VS Code window** (not Extension Development Host):
1. Help → Toggle Developer Tools
2. Go to Console tab
3. Look for messages like:
   ```
   In Memoria Visualizer: Starting activation...
   In Memoria Visualizer: Commands registered successfully
   In Memoria Visualizer: Activation complete!
   ```

If you see errors, that's your problem!

### Check if MCP Server is Running

**In a terminal:**
```bash
# The server should be running
npx in-memoria server
```

You should see:
```
🚀 Starting In Memoria MCP Server
✅ Local embedding pipeline ready
```

**Keep this terminal open** while testing the extension!

## 📋 Complete Testing Workflow

### Terminal 1: MCP Server
```bash
cd /home/user/In-Memoria  # Or your project folder
npx in-memoria server
```

### Terminal 2: Launch Extension
```bash
cd /home/user/In-Memoria/vscode-extension
code .
# Then press F5 in VS Code
```

### In Extension Development Host:
```
File → Open Folder → /home/user/In-Memoria
```

Now the extension should work!

## 🚨 Common Mistakes

1. **Opening your project folder directly** ❌
   - This won't run the extension
   - You need to open vscode-extension folder and press F5

2. **Not pressing F5** ❌
   - Just opening VS Code doesn't activate the extension
   - You MUST press F5 to launch Extension Development Host

3. **MCP server not running** ❌
   - The extension needs the server running
   - Start it BEFORE launching the extension

4. **Looking in the wrong VS Code window** ❌
   - Commands only work in Extension Development Host
   - Not in the original VS Code window

## ✅ How to Know It's Working

### In Original VS Code Window (vscode-extension folder):
- Developer Tools console shows activation logs
- No errors in console

### In Extension Development Host:
- In Memoria icon visible in Activity Bar
- Status bar shows "✓ In Memoria: Connected"
- Tree views show your learned data
- Commands work from Command Palette

## 🔧 If Still Not Working

1. **Close everything**
2. **Compile:**
   ```bash
   cd /home/user/In-Memoria/vscode-extension
   npm run compile
   ```
3. **Start MCP server:**
   ```bash
   cd /home/user/In-Memoria
   npx in-memoria server
   ```
4. **Launch extension:**
   ```bash
   cd /home/user/In-Memoria/vscode-extension
   code .
   # Press F5
   ```
5. **In Extension Development Host:**
   ```
   File → Open Folder → /home/user/In-Memoria
   ```

## 📝 What F5 Does

When you press F5 in the vscode-extension folder:
1. ✅ Compiles the TypeScript code
2. ✅ Launches a new VS Code instance (Extension Development Host)
3. ✅ Loads your extension in that new instance
4. ✅ Activates the extension and registers commands
5. ✅ Shows console logs for debugging

**Without F5, the extension doesn't load at all!**

## 🎯 TL;DR

```bash
# Terminal 1
cd /home/user/In-Memoria
npx in-memoria server

# Terminal 2
cd /home/user/In-Memoria/vscode-extension
code .
# Then press F5

# In the NEW window that opens:
# File → Open Folder → /home/user/In-Memoria
```

That's it! The extension ONLY works in the Extension Development Host window.
