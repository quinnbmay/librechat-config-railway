# LibreChat Custom Endpoint Configuration Guide

**Combined Memory - LibreChat Configuration Documentation**  
**Version: 1.2.8**  
**Last Updated: January 2025**

## Table of Contents
1. [Overview](#overview)
2. [Custom Endpoint Structure](#custom-endpoint-structure)
3. [Configuration Examples](#configuration-examples)
4. [Environment Variables](#environment-variables)
5. [API Provider Integration](#api-provider-integration)
6. [Speech & TTS Configuration](#speech--tts-configuration)
7. [MCP Servers](#mcp-servers)
8. [Troubleshooting](#troubleshooting)
9. [Best Practices](#best-practices)

## Overview

This LibreChat instance is configured for Combined Memory with integrated AI models, MCP servers, and enhanced speech capabilities. The configuration follows LibreChat's custom endpoint documentation standards for maximum compatibility and functionality.

## Custom Endpoint Structure

### Standard Custom Endpoint Object

```yaml
custom:
  - name: 'Provider Name'           # Display name in UI
    apiKey: '${API_KEY_VAR}'       # Environment variable reference
    baseURL: 'https://api.url'     # Provider API base URL
    models:
      default:                     # Model list
        - 'model-name-1'
        - 'model-name-2'
      fetch: true                  # Auto-fetch available models
    titleConvo: true               # Auto-generate conversation titles
    titleModel: 'model-for-titles' # Model used for title generation
    modelDisplayLabel: 'Label'     # Custom label in model selector
    dropParams: ['param1']         # Parameters to exclude from API calls
    timeout: 60000                 # Request timeout in milliseconds
```

### Required Fields
- **name**: String - Provider display name
- **apiKey**: String - API key (preferably as environment variable)
- **baseURL**: String - Provider's API endpoint

### Optional Configuration Fields
- **models**: Object - Model configuration
  - **default**: Array - List of available models
  - **fetch**: Boolean - Auto-discover models from API
- **titleConvo**: Boolean - Enable automatic conversation titles
- **titleModel**: String - Specific model for title generation
- **modelDisplayLabel**: String - Custom provider label
- **dropParams**: Array - API parameters to exclude
- **timeout**: Number - Request timeout in milliseconds
- **headers**: Object - Custom HTTP headers
- **addParams**: Object - Additional parameters for all requests

## Configuration Examples

### OpenAI Direct Integration
```yaml
openAI:
  apiKey: '${OPENAI_API_KEY}'
  organizationId: '${OPENAI_ORG_ID}'
  models:
    default: 
      - "gpt-4o"
      - "gpt-4o-mini"
      - "gpt-4-turbo"
      - "gpt-3.5-turbo"
  titleConvo: true
  titleModel: "gpt-3.5-turbo"
  summarize: false
  summaryModel: "gpt-3.5-turbo"
  forcePrompt: false
  modelDisplayLabel: "OpenAI"
```

### Mistral Direct API Integration
```yaml
custom:
  - name: "Mistral"
    apiKey: "${MISTRAL_API_KEY}"
    baseURL: "https://api.mistral.ai/v1"
    models:
      default: 
        - "mistral-tiny"
        - "mistral-small" 
        - "mistral-medium"
        - "mistral-large-latest"
      fetch: true
    titleConvo: true
    titleModel: "mistral-tiny"
    modelDisplayLabel: "Mistral"
    dropParams: ["stop", "user", "frequency_penalty", "presence_penalty"]
```

### Parameter Handling Best Practices

#### dropParams Usage
Different API providers support different parameters. Use `dropParams` to exclude unsupported parameters:

- **OpenAI Compatible**: Usually accepts all standard parameters
- **Mistral**: Drop `["stop", "user", "frequency_penalty", "presence_penalty"]`
- **Anthropic**: Drop `["stop", "frequency_penalty", "presence_penalty"]`
- **Custom APIs**: Test and adjust based on provider documentation

## Environment Variables

### Required API Keys
```bash
# OpenAI Integration
OPENAI_API_KEY=sk-...
OPENAI_ORG_ID=org-...

# Mistral AI Integration  
MISTRAL_API_KEY=...

# Google Generative AI Integration
GOOGLE_API_KEY=...

# Speech Services
TTS_API_KEY=...                    # ElevenLabs API key

# MCP Server Authentication
QUINNS_MEMORY_API_KEY=...
QUINNS_MEMORY_PROFILE=...
JINA_API_KEY=...
STEEL_API_KEY=...
WEBFLOW_TOKEN=...
RAILWAY_TOKEN=...
```

### Environment Variable Security
- Store sensitive keys in Railway environment variables
- Use `${VARIABLE_NAME}` syntax in YAML configuration
- Never commit actual API keys to version control
- Rotate keys regularly for security

## API Provider Integration

### Supported Provider Types

#### 1. OpenAI Compatible Providers
- **OpenAI Direct**: Native OpenAI API integration
- **OpenRouter**: Unified access to multiple models (removed in current config)
- **Custom OpenAI-compatible**: Any API following OpenAI format

#### 2. Native Provider APIs
- **Mistral**: Direct Mistral AI API integration
- **Google**: Direct Google Generative AI integration with Gemini models
- **Anthropic**: Claude API integration (future implementation)

### Model Selection Guidelines

#### OpenAI Models (Current Configuration)
```yaml
models:
  default:
    # GPT-4 Family - Production Ready
    - "gpt-4o"              # Latest GPT-4 Omni
    - "gpt-4o-mini"         # Efficient GPT-4 variant
    - "gpt-4-turbo"         # High-performance GPT-4
    - "gpt-4"               # Standard GPT-4
    - "gpt-3.5-turbo"       # Cost-effective option
```

#### Mistral Models (Current Configuration)
```yaml
models:
  default:
    - "mistral-tiny"        # Fastest, most economical
    - "mistral-small"       # Balanced performance/cost
    - "mistral-medium"      # Higher capability
    - "mistral-large-latest" # Most capable model
```

#### Google Gemini Models (Current Configuration)
```yaml
models:
  default:
    - "gemini-2.0-flash-exp"    # Latest experimental Gemini
    - "gemini-1.5-pro-latest"   # Most capable production model
    - "gemini-1.5-flash"        # Balanced performance model
    - "gemini-1.5-flash-8b"     # Efficient variant
    - "gemini-pro"              # Standard production model
```

## Speech & TTS Configuration

### Enhanced Speech Tab Features
```yaml
speech:
  speechTab:
    conversationMode: true          # Enable conversation mode
    advancedMode: true              # Show advanced controls
    speechToText:
      engineSTT: "external"         # Use external STT provider
      languageSTT: "English (US)"   # Speech recognition language
      autoTranscribeAudio: true     # Auto-transcribe audio input
      decibelValue: -45             # Voice activation threshold
      autoSendText: 0               # Auto-send delay (0 = disabled)
    textToSpeech:
      engineTTS: "external"         # Use external TTS provider
      voice: "Rachel"               # Default voice selection
      languageTTS: "en"             # TTS output language
      automaticPlayback: true       # Auto-play TTS responses
      playbackRate: 1.0             # Speech playback speed
      cacheTTS: true                # Cache TTS audio files
```

### STT Configuration (OpenAI Whisper)
```yaml
stt:
  openai:
    apiKey: '${OPENAI_API_KEY}'     # Same key as OpenAI models
    model: 'whisper-1'              # Whisper model for transcription
```

### TTS Configuration (ElevenLabs)
```yaml
tts:
  elevenlabs:
    apiKey: '${TTS_API_KEY}'        # ElevenLabs API key
    model: 'eleven_multilingual_v2' # Multilingual TTS model
    voices: ['21m00Tcm4TlvDq8ikWAM'] # Rachel voice ID
    voice_settings:
      similarity_boost: 0.75        # Voice similarity enhancement
      stability: 0.50               # Voice stability setting
      style: 0.0                    # Style variation (0 = natural)
      use_speaker_boost: true       # Enhanced speaker clarity
```

### Voice Configuration Notes
- **Rachel Voice ID**: `21m00Tcm4TlvDq8ikWAM` (tested and working)
- **Avoid problematic voices**: Some voice IDs may cause 400 errors
- **Voice settings optimized**: Current settings provide natural, clear speech
- **Multilingual support**: `eleven_multilingual_v2` supports multiple languages

## MCP Servers

### Integrated MCP Servers
```yaml
mcpServers:
  # Development & Project Management
  railway:
    type: stdio
    command: npx
    args: ['@jasontanswe/railway-mcp']
    
  # Workflow Automation  
  n8n-railway:
    type: stdio
    command: npx
    args: ['-y', 'mcp-remote', 'https://n8n-mcp-url/mcp']
    
  # Memory & AI Enhancement
  quinns-memory:
    type: http
    url: https://server.smithery.ai/@quinnbmay/quinns-memory-mcp-server/mcp
    
  # Document Processing
  jina-mcp-server:
    type: stdio
    command: npx
    args: ['-y', 'mcp-remote', 'https://mcp.jina.ai/sse']
    
  # Database & Page Management
  notion-mcp:
    type: http
    url: https://server.smithery.ai/@smithery/notion/mcp
    
  # Code Examples & Documentation
  upstash-context7:
    type: http
    url: https://server.smithery.ai/@upstash/context7-mcp/mcp
    
  # Repository Management
  github:
    type: http  
    url: https://server.smithery.ai/@smithery-ai/github/mcp
    
  # Live Browser Sessions
  steel-browser:
    type: stdio
    command: npx
    args: ['-y', 'steel-browser-mcp-server']
```

## Troubleshooting

### Common Issues and Solutions

#### 1. "No Funds" / "Insufficient Credits" Errors
**Problem**: Balance API showing false insufficient funds errors
**Solution**: Disable balance checking in configuration
```yaml
balance:
  enabled: false  # Disable unreliable balance checking
```

#### 2. TTS Voice Errors (400 Bad Request)
**Problem**: ElevenLabs returning 400 errors for certain voices
**Solution**: Use tested voice IDs like Rachel (`21m00Tcm4TlvDq8ikWAM`)
```yaml
voices: ['21m00Tcm4TlvDq8ikWAM']  # Rachel voice (tested)
```

#### 3. Model Not Available Errors
**Problem**: Requested model not found or accessible
**Solution**: 
- Verify API key has access to the model
- Check model name spelling in configuration
- Use `fetch: true` to auto-discover available models

#### 4. API Parameter Errors
**Problem**: Provider rejecting certain parameters
**Solution**: Use `dropParams` to exclude unsupported parameters
```yaml
dropParams: ["stop", "user", "frequency_penalty", "presence_penalty"]
```

#### 5. Environment Variable Issues
**Problem**: API keys not loading properly
**Solution**:
- Verify environment variables are set in Railway
- Use exact variable names in `${VARIABLE_NAME}` format
- Restart service after environment variable changes

### Debugging Steps
1. **Check LibreChat logs** for specific error messages
2. **Verify API keys** are valid and have sufficient credits
3. **Test API endpoints** independently to isolate issues
4. **Validate YAML syntax** using online YAML validators
5. **Check Railway deployment status** and environment variables

## Best Practices

### Security
- **Never commit API keys** to version control
- **Use environment variables** for all sensitive data
- **Rotate API keys** regularly
- **Monitor API usage** to detect unauthorized access
- **Use least-privilege access** for API keys when possible

### Performance
- **Enable caching** for TTS to reduce API calls
- **Use appropriate models** for each task (e.g., cheap models for titles)
- **Set reasonable timeouts** to prevent hanging requests
- **Monitor API rate limits** and implement backoff strategies

### Configuration Management  
- **Test changes** in development before production
- **Keep backups** of working configurations
- **Document custom modifications** for future reference
- **Use version control** for configuration files (without sensitive data)
- **Validate YAML syntax** before deployment

### Model Selection
- **Start with smaller models** for testing
- **Use cost-effective models** for simple tasks
- **Reserve powerful models** for complex reasoning
- **Consider latency requirements** when choosing models
- **Monitor usage costs** across different providers

---

## Configuration File Structure

```
LibreChat-Config/
├── librechat.yaml          # Main configuration file
├── README.md               # This documentation
├── .env.example            # Environment variable template
└── backups/                # Configuration backups
    └── librechat.yaml.bak  # Backup before changes
```

---

**Note**: This configuration is deployed to Railway via GitHub raw URL. Any changes to `librechat.yaml` trigger automatic redeployment of the LibreChat instance.

**Support**: For issues or questions, refer to the [LibreChat Documentation](https://docs.librechat.ai) or the Combined Memory project documentation.