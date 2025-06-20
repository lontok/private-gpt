# 🍎 PrivateGPT Mac Setup Guide

A straightforward guide to get PrivateGPT running on your Mac with developer tools in 20-25 minutes.

## 📑 Table of Contents

- [Prerequisites](#-prerequisites)
- [Complete Setup Guide](#-complete-setup-guide)
  - [Steps 1-2: Cursor IDE Setup](#step-1-install-cursor-ide-2-minutes)
  - [Steps 3-7: Install Dependencies](#step-3-check-prerequisites-in-cursor-terminal)
  - [Steps 8-9: Fork and Clone](#step-8-fork-the-repository-30-seconds)
  - [Steps 10-14: PrivateGPT Setup](#step-10-install-privategpt-dependencies-3-minutes)
  - [Step 15: Claude Code](#step-15-set-up-claude-code-1-minute)
- [Troubleshooting](#-troubleshooting)
- [Quick Commands Reference](#-quick-commands-reference)
- [Saving Your Changes with Git](#-saving-your-changes-with-git)
- [Optional Enhancements](#-optional-enhancements)
- [Understanding the Codebase with Claude Code](#-understanding-the-codebase-with-claude-code)
- [Next Steps](#-next-steps)
- [Need Help?](#-need-help)

## 📋 Prerequisites

### System Requirements
- macOS 11.0 or later
- 8GB RAM minimum (16GB recommended)
- 10GB free disk space

### Quick Check - What's Already Installed?

We'll check these after installing Cursor IDE, using its integrated terminal.

## 🚀 Complete Setup Guide

### Step 1: Install Cursor IDE (2 minutes)

**What is Cursor?** An AI-powered IDE based on VS Code, designed for pair programming with AI. It understands your codebase and provides intelligent suggestions.

**Why install it first?**
- Use its integrated terminal for all setup steps
- Get AI assistance while running commands
- Seamless transition from setup to development
- Better error messages and debugging

**Installation:**
1. Go to https://cursor.sh
2. Click "Download for Mac"
3. Open the downloaded `.dmg` file
4. Drag Cursor to your Applications folder
5. Launch Cursor from Applications

### Step 2: Open Cursor and Its Terminal (1 minute)

1. Open Cursor
2. Click "New Window" or press `Cmd+Shift+N`
3. Open the integrated terminal: `View → Terminal` or press `Ctrl+` ` (backtick)
4. The terminal opens at the bottom of the window

**💡 Tip**: You can drag the terminal divider to make it larger. All following commands will be run in this terminal.

### Step 3: Check Prerequisites in Cursor Terminal

Now let's check what's already installed:

```bash
# Check Python version (need 3.11.x)
python3 --version

# Check if Homebrew is installed
brew --version

# Check if Poetry is installed
poetry --version

# Check if Ollama is installed
ollama --version
```

### Step 4: Install Homebrew (2 minutes) - Skip if already installed

In the Cursor terminal:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Step 5: Install Python 3.11 (2 minutes)

```bash
# Install Python 3.11
brew install python@3.11

# Verify installation
python3.11 --version
```

### Step 6: Install Poetry (1 minute)

```bash
# Install Poetry
curl -sSL https://install.python-poetry.org | python3 -

# Add to PATH (for zsh - default on modern Macs)
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Verify installation
poetry --version
```

### Step 7: Install Ollama (1 minute)

```bash
# Install Ollama using Homebrew
brew install ollama

# Start Ollama service
brew services start ollama

# Verify it's installed
ollama --version
```

### Step 8: Fork the Repository (30 seconds)

1. Go to https://github.com/zylon-ai/private-gpt
2. Click the "Fork" button in the top-right corner
3. Select your GitHub account as the destination
4. Wait for the fork to complete

### Step 9: Clone Your Fork and Open in Cursor (2 minutes)

**First, let's create a dedicated folder for your projects:**

In the Cursor terminal:

```bash
# Create a projects directory in your home folder (if it doesn't exist)
mkdir -p ~/projects

# Navigate to the projects directory
cd ~/projects

# Show where you are (this will display something like /Users/yourname/projects)
pwd
```

**Now clone your fork in this location:**

```bash
# Clone your fork (replace YOUR_USERNAME with your GitHub username)
git clone https://github.com/YOUR_USERNAME/private-gpt.git

# Navigate into the project
cd private-gpt
```

**💡 Why organize projects this way?**
- All your coding projects will be in `~/projects`
- Easy to find: just open Finder and go to your home folder → projects
- Simple, clear folder name
- Your project will be at: `~/projects/private-gpt`

**Now open the project in Cursor:**
1. Click `File → Open Folder`
2. Select the `private-gpt` folder you just cloned
3. Cursor will reload with the project open
4. Open the terminal again: `Ctrl+` ` (backtick)

### Step 10: Install PrivateGPT Dependencies (3 minutes)

**Configure Python Interpreter first:**
1. Press `Cmd+Shift+P` to open command palette
2. Type "Python: Select Interpreter"
3. Choose Python 3.11

**Install dependencies in Cursor terminal:**

```bash
# Install with Ollama support and UI
poetry install --extras "ui llms-ollama embeddings-ollama vector-stores-qdrant"
```

### Step 11: Download Llama 3 Model (5-10 minutes)

```bash
# Pull the Llama 3 model (4.7GB download)
ollama pull llama3

# Verify it's downloaded
ollama list
```

### Step 12: Configure for Llama 3 (30 seconds)

```bash
# Update the Ollama settings to use llama3
sed -i '' 's/llm_model: llama3.1/llm_model: llama3/' settings-ollama.yaml
```

### Step 13: Start PrivateGPT (1 minute)

```bash
# Start the application
PGPT_PROFILES=ollama make run
```

### Step 14: Access the UI

Open your browser and go to: **http://localhost:8001**

🎉 **That's it!** You can now upload documents and chat with them privately.

**💡 Pro tip**: Keep PrivateGPT running in the Cursor terminal while you explore the code in the editor above!

### Step 15: Set Up Claude Code (1 minute)

**What is Claude Code?** An AI assistant specifically designed to help developers understand and navigate codebases. It can analyze architecture, explain components, and guide you through the code.

**Why use it for PrivateGPT?**
- Quickly understand the RAG pipeline architecture
- Get explanations of how components interact
- Find the right files for specific features
- Debug issues with AI assistance

**Setup:**
1. Go to https://claude.ai/code
2. Sign in or create an account
3. Click "New Project" and select your private-gpt folder
4. Claude Code will analyze the codebase

### ⏱️ Total Time: 20-25 minutes
- Cursor IDE setup (Steps 1-2): 3 minutes
- Prerequisites check: 1 minute
- Software installation (Steps 4-7): 6 minutes
- PrivateGPT setup (Steps 8-13): 11-16 minutes
- Claude Code setup (Step 15): 1 minute
- Most time is spent downloading the 4.7GB Llama 3 model

## 🔧 Troubleshooting

### Issue: "command not found: poetry"

```bash
# Make sure Poetry is in your PATH
export PATH="$HOME/.local/bin:$PATH"
```

### Issue: "Python 3.11 is required"

```bash
# Use Python 3.11 specifically
poetry env use python3.11
```

### Issue: "Connection refused on port 11434"

```bash
# Start Ollama service
ollama serve

# Or restart the Ollama app from Applications
```

### Issue: "Model not found"

```bash
# Make sure Llama 3 is downloaded
ollama pull llama3
```

### Issue: Port 8001 already in use

```bash
# Find what's using port 8001
lsof -i :8001

# Kill the process (replace PID with actual number)
kill -9 PID

# Or run on different port
PGPT_PROFILES=ollama PORT=8002 make run
```

## 🎯 Quick Commands Reference

```bash
# Start PrivateGPT
PGPT_PROFILES=ollama make run

# Run in background
nohup poetry run python -m private_gpt > privategpt.log 2>&1 &

# Stop PrivateGPT
pkill -f "python -m private_gpt"

# View logs
tail -f privategpt.log

# Upload documents via CLI
make ingest /path/to/documents

# Clear all data
make wipe
```

## 💾 Saving Your Changes with Git

As you modify PrivateGPT, you'll want to save your changes to your GitHub fork. Here's how to commit and push your work.

### When to Commit and Push

**Commit your changes when:**
- ✅ You complete a feature or fix a bug
- ✅ Your code is working (even if not perfect)
- ✅ Before taking a break or ending your coding session
- ✅ Before trying something experimental (so you can revert if needed)
- ✅ At least once per day when actively developing

**Don't wait until:**
- ❌ Everything is "perfect" (it never will be!)
- ❌ You have massive changes (harder to review and debug)

### Method 1: Using Git Commands in Cursor Terminal

**Basic workflow:**

```bash
# 1. Check what files have changed
git status

# 2. Stage all changes (or specific files)
git add .                          # Add all changes
# OR
git add settings-ollama.yaml       # Add specific file

# 3. Commit with a descriptive message
git commit -m "Add Llama 3 configuration and update embeddings"

# 4. Push to your fork
git push origin main
```

**First-time push (sets upstream):**
```bash
git push -u origin main
```

**Good commit messages:**
```bash
# Good: Clear and specific
git commit -m "Fix Ollama connection timeout issue"
git commit -m "Add support for PDF password protection"

# Bad: Too vague
git commit -m "Fix bug"
git commit -m "Update files"
```

### Method 2: Using Claude Code

Claude Code can help you commit with better messages and even handle the Git commands for you.

**Simple prompts:**
```
"Commit and push my changes"

"Commit my changes with a message about adding Llama 3 support"

"Show me what changes I'm about to commit"
```

**Detailed prompts for complex changes:**
```
"I've modified the Ollama configuration to use Llama 3 and updated the temperature settings. Create a commit with a detailed message and push it"

"Review my changes and suggest a good commit message, then commit and push"

"I've been working on document ingestion improvements. Help me create multiple commits to organize my changes logically"
```

**Benefits of using Claude Code:**
- 📝 Generates descriptive commit messages
- 🔍 Reviews changes before committing
- 📚 Follows Git best practices
- 🎯 Suggests logical commit groupings

### Common Git Scenarios

**Forgot to add a file:**
```bash
git add forgotten-file.py
git commit --amend --no-edit  # Adds to previous commit
git push --force              # Update the remote
```

**Sync with the original repository:**
```bash
# Get latest changes from original repo
git fetch upstream
git merge upstream/main

# Resolve any conflicts, then push
git push origin main
```

**Undo last commit (before pushing):**
```bash
git reset --soft HEAD~1  # Keeps changes, removes commit
```

### Best Practices

1. **Commit often**: Small, focused commits are easier to understand
2. **Test before committing**: Make sure your code runs
3. **Write clear messages**: Future you will thank present you
4. **Pull before push**: If working with others, sync first
5. **Use branches**: For major features, create a new branch

**Example of good commit history:**
```
feat: Add Llama 3 model support
fix: Resolve PDF parsing memory leak  
docs: Update setup guide with troubleshooting
refactor: Simplify embedding generation logic
```

## 🚀 Optional Enhancements

### Run in Background

```bash
# Start in background
cd /path/to/private-gpt
nohup sh -c 'PGPT_PROFILES=ollama poetry run python -m private_gpt' > ~/privategpt.log 2>&1 &

# Check if running
ps aux | grep private_gpt

# View logs
tail -f ~/privategpt.log
```

### Auto-Start on Login

1. Create a launch script:

```bash
cat > ~/start-privategpt.sh << 'EOF'
#!/bin/bash
cd /path/to/private-gpt
export PATH="$HOME/.local/bin:$PATH"
PGPT_PROFILES=ollama poetry run python -m private_gpt
EOF

chmod +x ~/start-privategpt.sh
```

2. Add to Login Items:
   - System Settings → General → Login Items
   - Click + and add the script

### Performance Tips for Mac

1. **For Apple Silicon (M1/M2/M3):**
   ```bash
   # Ollama automatically uses Metal acceleration
   # No additional configuration needed
   ```

2. **Adjust context window for better performance:**
   ```yaml
   # In settings-ollama.yaml
   llm:
     context_window: 2048  # Reduce from 3900 for faster responses
   ```

3. **Use smaller models for speed:**
   ```bash
   # Try Mistral for faster responses
   ollama pull mistral
   
   # Update settings-ollama.yaml
   # llm_model: mistral
   ```

## 🤖 Understanding the Codebase with Claude Code

Claude Code can help you navigate and understand PrivateGPT's architecture. Here are useful prompts organized by what you want to learn:

### Architecture Understanding
```
"Explain the high-level architecture of PrivateGPT and how the main components interact"

"How does the RAG (Retrieval Augmented Generation) pipeline work in this codebase?"

"What is the flow when a user uploads a document and asks a question about it?"
```

### Component Exploration
```
"Show me how the Ollama LLM component is implemented"

"Where is the document ingestion logic and what file types are supported?"

"How does the vector store (Qdrant) integration work?"

"Explain the embedding component and how documents are converted to vectors"
```

### Configuration & Setup
```
"What are the different configuration profiles and how do they work?"

"How can I add a new LLM provider to the system?"

"Where are the API endpoints defined and what do they do?"
```

### Debugging & Troubleshooting
```
"Help me understand why my documents aren't being ingested properly"

"Where should I add logging to debug the chat completion flow?"

"How can I monitor the performance of document retrieval?"
```

### Feature Development
```
"How would I add support for a new document type like CSV?"

"Guide me through adding a new API endpoint for bulk document upload"

"How can I modify the chunking strategy for better retrieval?"
```

### Best Practices with Claude Code
1. **Start broad**: Ask about architecture before diving into specific files
2. **Be specific**: Include file names or component names when you know them
3. **Iterate**: Use follow-up questions to go deeper
4. **Cross-reference**: Use Claude Code findings with Cursor for implementation

## 📚 Next Steps

1. **Explore the Code**: Use Cursor and Claude Code to understand the architecture
2. **Upload Documents**: Test the UI with various document types
3. **Customize Settings**: Modify `settings-ollama.yaml` for your needs
4. **Try Different Models**: Experiment with Mistral or other Ollama models
5. **Build Features**: Use the developer tools to add new capabilities
6. **Join Community**: Discord link in the main README

## 🆘 Need Help?

- Check the logs: `tail -f privategpt.log`
- Enable debug mode: `PGPT_LOG_LEVEL=DEBUG PGPT_PROFILES=ollama make run`
- Visit the [documentation](https://docs.privategpt.dev/)
- Ask on [Discord](https://discord.gg/bK6mRVpErU)

---

**Pro Tip**: For the best experience, use Safari or Chrome. The UI works great with macOS native features like drag-and-drop for file uploads!