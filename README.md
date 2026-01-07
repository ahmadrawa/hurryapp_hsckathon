# Hurry App Hackathon

This repository contains hackathon challenge solutions with VS Code configuration for Claude AI integration.

## 🚀 Claude Opus Support

This workspace is now configured with **Claude 3 Opus** support in VS Code!

### Quick Start

1. **Open in VS Code**:
   ```bash
   code .
   ```

2. **Install Recommended Extensions**:
   - VS Code will prompt you to install recommended extensions
   - Or manually install: `Anthropic Claude`, `GitHub Copilot`

3. **Configure Your API Key**:
   - Get your API key from [Anthropic Console](https://console.anthropic.com/)
   - Add to your VS Code settings or environment variables

### Available Models

The workspace is configured with three Claude models:

| Model | Description | Best For |
|-------|-------------|----------|
| **Claude 3 Opus** | Most powerful | Complex reasoning, advanced analysis |
| **Claude 3.5 Sonnet** | Balanced | General-purpose tasks |
| **Claude 3 Haiku** | Fastest | Quick responses, simple tasks |

**Default Model**: Claude 3 Opus (as requested)

### Configuration Files

- `.vscode/settings.json` - Model configurations and workspace settings
- `.vscode/extensions.json` - Recommended VS Code extensions
- `.vscode/README.md` - Detailed configuration documentation

## 📁 Project Structure

```
.
├── Q1/           # Challenge 1: Password Policy Checker
├── Q2/           # Challenge 2: Cross-Log Correlation
├── Q3/           # Challenge 3: Bracket Checker
├── Q4/           # Challenge 4: Domain Filter Validator
├── Q5/           # Challenge 5: Gilgamesh Battleground
└── .vscode/      # VS Code configuration with Claude Opus
```

## 🔧 VS Code Settings

The workspace includes:
- ✅ Claude 3 Opus as default model
- ✅ All Claude models (Opus, Sonnet, Haiku)
- ✅ Format on save enabled
- ✅ Code actions on save

## 📖 Documentation

For detailed information about Claude model configuration, see [.vscode/README.md](.vscode/README.md)

## 🐛 Troubleshooting

**Claude Opus not showing up?**
1. Restart VS Code
2. Ensure extensions are installed
3. Check API key configuration
4. Verify workspace settings are loaded

## 📝 License

Hackathon Project
