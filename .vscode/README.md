# VS Code Configuration for Claude AI

This directory contains VS Code workspace settings that enable Claude Opus and other Claude models.

## Available Claude Models

### Claude 3 Opus
- **Model ID**: `claude-3-opus-20240229`
- **Description**: Most powerful model for complex tasks
- **Context Window**: 200,000 tokens
- **Max Output**: 4,096 tokens
- **Best for**: Complex reasoning, advanced analysis, and challenging tasks

### Claude 3.5 Sonnet
- **Model ID**: `claude-3-5-sonnet-20241022`
- **Description**: Balanced performance for most tasks
- **Context Window**: 200,000 tokens
- **Max Output**: 4,096 tokens
- **Best for**: General-purpose tasks with good balance of speed and capability

### Claude 3 Haiku
- **Model ID**: `claude-3-haiku-20240307`
- **Description**: Fastest model for simple tasks
- **Context Window**: 200,000 tokens
- **Max Output**: 4,096 tokens
- **Best for**: Quick responses and simple tasks

## How to Use

1. **Install Required Extensions**:
   - Open VS Code
   - Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac)
   - Type "Extensions: Show Recommended Extensions"
   - Install the recommended extensions

2. **Configure API Key**:
   - Get your API key from [Anthropic Console](https://console.anthropic.com/)
   - Add it to your VS Code settings or environment variables

3. **Select Model**:
   - The default model is set to `claude-3-opus-20240229` (Claude 3 Opus)
   - You can change it in `settings.json` by modifying the `claude.defaultModel` setting

4. **Start Using**:
   - Open any file in your workspace
   - Use the Claude extension commands from the command palette (Ctrl+Shift+P / Cmd+Shift+P)
   - GitHub Copilot is also available and works independently with its own models

## Configuration Files

- **settings.json**: Contains workspace-specific settings for Claude models
  - Uses multiple configuration formats for compatibility with different Claude extensions
  - `anthropic.*` settings for official Anthropic extensions
  - `claude.*` settings for community Claude extensions
  - Default model is set to Claude 3 Opus across all configurations
- **extensions.json**: Lists recommended VS Code extensions for AI development
- **README.md**: Detailed documentation on configuration and usage

## Troubleshooting

If Claude Opus is not appearing:
1. Ensure you have the latest version of the Claude VS Code extension
2. Check that your API key is correctly configured
3. Verify that `settings.json` is in the `.vscode` directory of your workspace
4. Restart VS Code after making configuration changes

## Support

For issues with Claude models, visit:
- [Anthropic Documentation](https://docs.anthropic.com/)
- [Claude VS Code Extension](https://marketplace.visualstudio.com/items?itemName=anthropics.claude-vscode)
