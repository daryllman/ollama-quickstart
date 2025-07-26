# Ollama Tutorial

A comprehensive guide to getting started with Ollama - the powerful local AI model runner.

## What is Ollama?

Ollama is an open-source tool that allows you to run large language models (LLMs) locally on your machine. It provides a simple way to download, run, and manage AI models without needing cloud services or complex setup.

## Installation

### macOS
```bash
# Using Homebrew
brew install ollama

# Or download from the official website
# Visit https://ollama.ai/download
```

### Linux
```bash
# Using curl
curl -fsSL https://ollama.ai/install.sh | sh

# Or using wget
wget -O - https://ollama.ai/install.sh | sh
```

### Windows
- Download the installer from [https://ollama.ai/download](https://ollama.ai/download)
- Run the installer and follow the setup wizard

## Getting Started

### 1. Start Ollama
After installation, start the Ollama service:

```bash
ollama serve
```

This will start the Ollama server in the background. You should see output indicating the server is running.

### 2. Download Your First Model
Ollama comes with a variety of models. Let's start with a popular one:

```bash
# Download Llama 2 (7B parameters)
ollama pull llama2

# Or try a smaller, faster model
ollama pull llama2:7b
```

### 3. Run Your First Chat
Start an interactive chat session:

```bash
ollama run llama2
```

You'll see a prompt where you can start chatting with the model. Type your questions and press Enter to get responses.

## Managing Ollama Service

### Background Service
On macOS, Ollama typically runs as a background service that starts automatically. This means you don't need to manually run `ollama serve` each time.

### Check if Ollama is Running
```bash
# Check if Ollama is already running
ps aux | grep ollama

# Or test the API
curl http://localhost:11434/api/tags
```

### Stop Ollama Service
If you need to stop Ollama, you have several options:

**Method 1: Kill the process directly**
```bash
# Find the process ID
ps aux | grep ollama

# Kill the process (replace PID with actual process ID)
kill <PID>
```

**Method 2: Use pkill**
```bash
# Kill all ollama processes
pkill ollama
```

**Method 3: Stop via Ollama app (macOS)**
- Open the Ollama app from Applications
- Use the app's interface to stop the service

### Restart Ollama Service
```bash
# Stop the service first
pkill ollama

# Then start it again
ollama serve
```

### Common Service Issues
- **"Address already in use" error**: Ollama is already running, no need to start it again
- **Service not responding**: Restart the service using the methods above
- **Port 11434 busy**: Another instance of Ollama is running

## Basic Usage

### Interactive Chat
```bash
# Start a chat session
ollama run llama2

# You'll see something like:
# >>> Send a message (/? for help)
# >>> Hello! How are you today?
```

### Single Query
```bash
# Ask a single question
ollama run llama2 "What is the capital of France?"
```

### Using Different Models
```bash
# Try different models
ollama run codellama "Write a Python function to calculate fibonacci numbers"
ollama run mistral "Explain quantum computing in simple terms"
ollama run llama2:13b "Write a short story about a robot"
```

## Model Management

### List Available Models
```bash
# See all models you have downloaded
ollama list
```

### Download More Models
```bash
# Popular models to try
ollama pull codellama    # Great for programming
ollama pull mistral      # Good general purpose
ollama pull llama2:13b   # Larger, more capable version
ollama pull llama2:70b   # Very large, very capable
```

### Remove Models
```bash
# Remove a model to free up space
ollama rm llama2:70b
```

### Model Information
```bash
# Get details about a specific model
ollama show llama2
```

## Advanced Features

### Custom Model Configuration
Create a `Modelfile` to customize model behavior:

```bash
# Create a Modelfile
cat > Modelfile << EOF
FROM llama2
PARAMETER temperature 0.7
PARAMETER top_p 0.9
SYSTEM You are a helpful coding assistant.
EOF

# Build your custom model
ollama create my-coding-assistant -f Modelfile

# Use your custom model
ollama run my-coding-assistant
```

### API Usage
Ollama provides a REST API for integration:

```bash
# Start the server (if not already running)
ollama serve

# Make API calls
curl -X POST http://localhost:11434/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama2",
    "prompt": "Hello, how are you?",
    "stream": false
  }'
```

### Python Integration
```python
import requests

def ask_ollama(prompt, model="llama2"):
    response = requests.post(
        "http://localhost:11434/api/generate",
        json={
            "model": model,
            "prompt": prompt,
            "stream": False
        }
    )
    return response.json()["response"]

# Usage
answer = ask_ollama("What is machine learning?")
print(answer)
```

## Tips and Best Practices

### 1. Choose the Right Model Size
- **Small models (7B)**: Fast, good for simple tasks, less memory usage
- **Medium models (13B)**: Better quality, balanced performance
- **Large models (70B)**: Best quality, requires more resources

### 2. System Requirements
- **RAM**: 8GB minimum, 16GB+ recommended for larger models
- **Storage**: Models can be 4-40GB each
- **GPU**: Optional but recommended for faster inference

### 3. Model Parameters
```bash
# Adjust temperature (creativity vs consistency)
ollama run llama2 --temperature 0.1  # More focused
ollama run llama2 --temperature 0.9  # More creative

# Adjust top_p (nucleus sampling)
ollama run llama2 --top-p 0.9
```

### 4. Memory Management
```bash
# Check model sizes
ollama list

# Remove unused models
ollama rm model-name

# Pull specific model variants to save space
ollama pull llama2:7b  # Instead of the full model
```

## Troubleshooting

### Common Issues

**Model not found:**
```bash
# Make sure you've pulled the model first
ollama pull model-name
```

**Out of memory:**
```bash
# Try a smaller model
ollama pull llama2:7b
# Or close other applications to free up RAM
```

**Server not responding:**
```bash
# Restart the Ollama service
ollama serve
```

### Getting Help
```bash
# Built-in help
ollama --help
ollama run --help

# Check server status
curl http://localhost:11434/api/tags
```

## Useful Commands Reference

```bash
# Basic commands
ollama serve          # Start the server
ollama list          # List models
ollama pull <model>  # Download model
ollama run <model>   # Run model
ollama rm <model>    # Remove model

# Advanced commands
ollama create <name> -f Modelfile  # Create custom model
ollama show <model>                # Show model info
ollama cp <source> <destination>   # Copy model
ollama push <model>                # Push to registry
```

## Next Steps

1. **Explore different models**: Try CodeLlama for programming, Mistral for general use
2. **Create custom models**: Build specialized assistants for your needs
3. **Integrate with applications**: Use the API to add AI to your projects
4. **Join the community**: Visit [https://ollama.ai](https://ollama.ai) for updates and community

## Resources

- **Official Website**: [https://ollama.ai](https://ollama.ai)
- **GitHub Repository**: [https://github.com/ollama/ollama](https://github.com/ollama/ollama)
- **Model Library**: [https://ollama.ai/library](https://ollama.ai/library)
- **Documentation**: [https://github.com/ollama/ollama/blob/main/docs/README.md](https://github.com/ollama/ollama/blob/main/docs/README.md)

---

Happy coding with Ollama! 🚀 