# Troubleshooting Guide

## Error: "command 'inMemoria.treeView.refresh' not found"

This error occurs when the extension hasn't been properly compiled or activated.

### Solution:

1. **Ensure the extension is compiled:**
   ```bash
   cd vscode-extension
   npm run compile
   ```

2. **Check the Output panel for activation errors:**
   - Open VS Code
   - Go to View → Output
   - Select "Extension Host" or "In Memoria Visualizer" from the dropdown
   - Look for error messages during activation

3. **Verify the MCP server is running:**
   ```bash
   # In a separate terminal
   cd /path/to/your/project
   npx in-memoria learn .    # Learn the codebase first
   npx in-memoria server      # Start the server
   ```

4. **Launch the Extension Development Host properly:**
   - Open the `vscode-extension` folder in VS Code
   - Press F5 (or Run → Start Debugging)
   - This will compile and launch the extension

### Common Causes:

1. **Extension not compiled** - Run `npm run compile` first
2. **Dist folder outdated** - Delete `dist/` and recompile
3. **Activation error** - Check the console logs (see step 2 above)
4. **MCP server not running** - Start the server before testing

## Error: "No data showing in views"

This means the codebase hasn't been learned yet.

### Solution:

1. **Learn your codebase:**
   ```bash
   npx in-memoria learn .
   ```

   Expected output:
   ```
   ✅ Learned 234 concepts from codebase
   ✅ Pattern learning completed: 87 patterns discovered
   📁 Files: 156
   ```

2. **Verify intelligence was created:**
   ```bash
   ls -la in-memoria.db
   ```

3. **Refresh the extension views:**
   - Command Palette → `In Memoria: Refresh Intelligence`
   - Or click the refresh icon in the tree view toolbar

## Error: "Could not connect to In Memoria server"

The extension can't connect to the MCP server.

### Solution:

1. **Start the MCP server:**
   ```bash
   npx in-memoria server
   ```

   You should see:
   ```
   🚀 Starting In Memoria MCP Server
   ✅ Local embedding pipeline ready
   ```

2. **Check the server command in settings:**
   - Settings → Extensions → In Memoria Visualizer
   - Verify `inMemoria.serverCommand` is correct (default: `npx in-memoria server`)

3. **Disable auto-connect and connect manually:**
   - Settings → `inMemoria.autoConnect` → false
   - Start the MCP server in a terminal
   - Reload the extension window

## Debugging Tips

### Enable verbose logging:

1. Open the Developer Tools:
   - Help → Toggle Developer Tools (in Extension Development Host)

2. Check the Console tab for detailed logs:
   ```
   In Memoria Visualizer: Starting activation...
   In Memoria Visualizer: MCP client initialized
   In Memoria Visualizer: Tree view providers initialized
   In Memoria Visualizer: Tree views registered
   In Memoria Visualizer: Registering commands...
   In Memoria Visualizer: Commands registered successfully
   In Memoria Visualizer: Attempting to connect to MCP server...
   In Memoria Visualizer: Connected successfully
   In Memoria Visualizer: Activation complete!
   ```

3. If activation fails, you'll see:
   ```
   In Memoria Visualizer: Activation failed: [error details]
   ```

### Clean rebuild:

```bash
cd vscode-extension
rm -rf dist node_modules package-lock.json
npm install
npm run compile
```

### Test with a simple codebase first:

Instead of testing with an empty project, test with the In Memoria codebase itself:

```bash
cd /home/user/In-Memoria
npx in-memoria learn .
npx in-memoria server

# In another terminal
cd vscode-extension
code .
# Press F5 to launch
```

## Still having issues?

1. Check the [TESTING_GUIDE.md](./TESTING_GUIDE.md) for step-by-step instructions
2. Look at the Extension Development Host console logs
3. Verify you're testing with a real codebase (not an empty folder)
4. Ensure In Memoria found files when you ran `learn` (check the output)

## Quick Checklist

- [ ] Extension is compiled (`npm run compile`)
- [ ] dist/ folder exists and is recent
- [ ] Codebase was learned (found files with `npx in-memoria learn .`)
- [ ] MCP server is running (`npx in-memoria server`)
- [ ] Extension launched via F5 (not just opening VS Code)
- [ ] Checked console logs for errors
- [ ] Verified intelligence database exists (in-memoria.db)
