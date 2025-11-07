# Extension Not Activating - Diagnostic Steps

## Current Status
You're seeing the Extension Host console, but NO "In Memoria Visualizer" logs.
This means the extension isn't activating.

## 🔍 Step 1: Check Where You're Looking

**IMPORTANT:** There are TWO places to check for logs:

### A. Extension Host Console (Developer Tools)
In the **Extension Development Host** window (the NEW window that opened):
1. Help → Toggle Developer Tools (or Ctrl+Shift+I)
2. Go to **Console** tab
3. Look for: `In Memoria Visualizer: Starting activation...`

### B. Output Panel
In the **Extension Development Host** window:
1. View → Output (or Ctrl+Shift+U)
2. Select **Extension Host** from dropdown
3. Look for activation logs or errors

## 🔍 Step 2: Verify Extension is Loaded

In the **Extension Development Host** window:
1. View → Extensions (Ctrl+Shift+X)
2. Type "@installed" in search
3. Look for "In Memoria Visualizer"
4. It should show as "Installed" or "Running"

If you DON'T see it, the extension isn't loading at all.

## 🔧 Step 3: Force Activation

Try manually activating the extension:

1. In **Extension Development Host** window
2. Press **Ctrl+Shift+P** (Command Palette)
3. Type: `Developer: Show Running Extensions`
4. Look for "in-memoria-visualizer"
5. Check its status:
   - ✅ "Activated" - Good!
   - ⚠️ "Not activated" - Problem!
   - ❌ Not in list - Extension didn't load!

## 🐛 Step 4: Check for Errors

### In Developer Console:
Look for red error messages like:
```
Error: Cannot find module...
Error activating extension...
```

### In Extension Host Output:
Look for:
```
Activating extension 'in-memoria-visualizer' failed...
```

## 🔨 Step 5: Common Fixes

### Fix 1: Clean Rebuild
```bash
cd /home/user/In-Memoria/vscode-extension
rm -rf dist
npm run compile
```

### Fix 2: Check File Permissions
```bash
ls -la dist/extension.js
# Should be readable (r-- or better)
```

### Fix 3: Verify VS Code Version
In Extension Development Host:
- Help → About
- Version should be ≥ 1.85.0

### Fix 4: Launch Configuration
Check `.vscode/launch.json` exists and has:
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Run Extension",
      "type": "extensionHost",
      "request": "launch",
      "args": ["--extensionDevelopmentPath=${workspaceFolder}"],
      "outFiles": ["${workspaceFolder}/dist/**/*.js"]
    }
  ]
}
```

## 🎯 Expected Logs

### If Extension Activates Successfully:
```
In Memoria Visualizer: Starting activation...
In Memoria Visualizer: MCP client initialized
In Memoria Visualizer: Status bar created
In Memoria Visualizer: Tree view providers initialized
In Memoria Visualizer: Tree views registered
In Memoria Visualizer: Registering commands...
In Memoria Visualizer: Commands registered successfully
In Memoria Visualizer: Attempting to connect to MCP server...
In Memoria Visualizer: Connected successfully
In Memoria Visualizer: Activation complete!
```

### If Connection Fails (but extension activated):
```
In Memoria Visualizer: Starting activation...
...
In Memoria Visualizer: Commands registered successfully
In Memoria Visualizer: Attempting to connect to MCP server...
In Memoria Visualizer: Connection failed: [error]
```

### If Extension Doesn't Activate:
```
(No logs at all)
```

## 🚨 If Still No Logs

### Check the Original VS Code Window
The one where you pressed F5 (NOT Extension Development Host):
1. Help → Toggle Developer Tools
2. Console tab
3. Look for errors about loading the extension

### Restart Everything
1. Close ALL VS Code windows
2. Kill any running processes:
   ```bash
   pkill -f "Code.*extensionHost"
   ```
3. Start fresh:
   ```bash
   cd /home/user/In-Memoria/vscode-extension
   npm run compile
   code .
   # Press F5
   ```

## 📋 Checklist for Extension to Activate

- [ ] Opened vscode-extension folder in VS Code
- [ ] Pressed F5 (started debugging)
- [ ] Extension Development Host window opened
- [ ] Compiled recently (`npm run compile`)
- [ ] dist/extension.js exists and is recent
- [ ] Checked Developer Console in Extension Development Host
- [ ] Checked Output panel (Extension Host channel)
- [ ] VS Code version ≥ 1.85.0

## 🎬 Try This Step-by-Step

1. **Close all VS Code windows**

2. **Clean and compile:**
   ```bash
   cd /home/user/In-Memoria/vscode-extension
   rm -rf dist
   npm run compile
   ls -la dist/extension.js  # Verify it exists
   ```

3. **Open in VS Code:**
   ```bash
   code .
   ```

4. **In VS Code window, press F5**

5. **In the NEW window (Extension Development Host):**
   - Press Ctrl+Shift+I (Developer Tools)
   - Go to Console tab
   - Press Ctrl+R to reload window
   - Watch console for activation logs

6. **If still no logs, in Extension Development Host:**
   - Ctrl+Shift+P → "Developer: Show Running Extensions"
   - Look for "in-memoria-visualizer"
   - Check its status

## 📤 What to Report

If still not working, please share:

1. **Output from:**
   ```bash
   ls -la /home/user/In-Memoria/vscode-extension/dist/
   ```

2. **Screenshot or copy of:**
   - Developer Console (Ctrl+Shift+I → Console)
   - Output panel (View → Output → Extension Host)
   - Running Extensions (Ctrl+Shift+P → Show Running Extensions)

3. **Your VS Code version:**
   - Help → About

4. **Any error messages** in red in the console
