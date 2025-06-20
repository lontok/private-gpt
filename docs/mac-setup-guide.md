# 🍎 PrivateGPT Mac Setup Guide

A straightforward guide to get PrivateGPT running on your Mac in under 10 minutes.

## 📋 Prerequisites

### System Requirements
- macOS 11.0 or later
- 8GB RAM minimum (16GB recommended)
- 10GB free disk space

### Quick Check - What's Already Installed?

Open Terminal and run these commands:

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

## 🚀 Step-by-Step Installation

### Step 1: Install Homebrew (if not installed)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Step 2: Install Python 3.11

```bash
# Install Python 3.11
brew install python@3.11

# Verify installation
python3.11 --version
```

### Step 3: Install Poetry

```bash
# Install Poetry
curl -sSL https://install.python-poetry.org | python3 -

# Add to PATH (for zsh - default on modern Macs)
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Verify installation
poetry --version
```

### Step 4: Install Ollama

**Option 1: Install via Homebrew (Recommended)**
```bash
# Install Ollama using Homebrew
brew install ollama

# Start Ollama service
brew services start ollama

# Verify it's installed
ollama --version
```

**Option 2: Download the Mac App**
1. Go to https://ollama.com/download
2. Click "Download for macOS"
3. Open the downloaded file and drag Ollama to Applications
4. Launch Ollama from Applications (it will run in the menu bar)

## 💨 Quick Start (5 Minutes)

### 1. Clone the Repository

```bash
# Clone PrivateGPT
git clone https://github.com/zylon-ai/private-gpt.git
cd private-gpt
```

### 2. Install Dependencies

```bash
# Install with Ollama support and UI
poetry install --extras "ui llms-ollama embeddings-ollama vector-stores-qdrant"
```

### 3. Download Llama 3 Model

```bash
# Pull the Llama 3 model (4.7GB download)
ollama pull llama3

# Verify it's downloaded
ollama list
```

### 4. Configure for Llama 3

```bash
# Update the Ollama settings to use llama3
sed -i '' 's/llm_model: llama3.1/llm_model: llama3/' settings-ollama.yaml
```

### 5. Run PrivateGPT

```bash
# Start the application
PGPT_PROFILES=ollama make run
```

### 6. Access the UI

Open your browser and go to: **http://localhost:8001**

🎉 **That's it!** You can now upload documents and chat with them privately.

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

## 📚 Next Steps

1. **Upload Documents**: Use the UI to upload PDFs, Word docs, or text files
2. **Ask Questions**: Start chatting with your documents
3. **Explore Settings**: Check `settings-ollama.yaml` for customization
4. **Join Community**: Discord link in the main README

## 🆘 Need Help?

- Check the logs: `tail -f privategpt.log`
- Enable debug mode: `PGPT_LOG_LEVEL=DEBUG PGPT_PROFILES=ollama make run`
- Visit the [documentation](https://docs.privategpt.dev/)
- Ask on [Discord](https://discord.gg/bK6mRVpErU)

---

**Pro Tip**: For the best experience, use Safari or Chrome. The UI works great with macOS native features like drag-and-drop for file uploads!