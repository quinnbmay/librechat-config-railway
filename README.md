# LibreChat MCP Integration for Combined Memory

This repository contains the configuration for integrating all of Quinn's MCP servers with LibreChat on Railway.

## Railway Project
- **Project**: Combined Memory - LibreChat 🪶
- **URL**: https://chat.combinedmemory.com
- **Railway Project ID**: 5d383695-8e2b-41b2-a1dd-77e0ce6021f0

## MCP Servers Included

### Development & Project Management
- **Railway**: Project and deployment management
- **GitHub**: Repository and workflow management  
- **Notion**: Project tracking and documentation

### Web Development & Design
- **Webflow**: Website management and CMS operations
- **Puppeteer**: Browser automation and testing

### Workflow Automation
- **n8n-Railway**: Workflow automation and integrations

### Data & Research
- **Firecrawl**: Web scraping and data extraction
- **Supabase**: Database operations and management
- **GoMarble**: Analytics and data processing

### Marketing & Ads
- **Apify FB Ads**: Facebook ad library scraping

### Memory & AI Enhancement
- **Quinn's Memory**: Personal memory and context storage
- **Sequential Thinking**: Enhanced reasoning capabilities

### Documentation & Knowledge
- **DocFork**: Library documentation access
- **Upstash Context7**: Documentation and code examples

### Migration & Development Tools
- **Smithery AI Migration Guide**: MCP migration assistance

### Utilities
- **Time MCP**: Date and time operations
- **Jotdown**: Note-taking and documentation

## Usage

1. **Local Development**: Use `librechat.yaml` for local testing
2. **Production**: Deploy via Railway using the CONFIG_PATH environment variable
3. **Updates**: Push changes to trigger Railway redeployment

## Deployment Workflow

1. Edit `librechat.yaml` locally
2. Commit and push changes
3. Update Railway's CONFIG_PATH to point to your raw GitHub file
4. Redeploy Railway service

## Environment Variables Required

All API keys and secrets are managed through Railway environment variables:

### AI Model API Keys
- GROQ_API_KEY
- MISTRAL_API_KEY  
- OPENROUTER_KEY
- FIREWORKS_API_KEY
- TOGETHERAI_API_KEY
- PERPLEXITY_API_KEY

### MCP Server API Keys
- APIFY_API_KEY (for Facebook Ads scraping)
- N8N_MCP_API_KEY (for n8n workflow automation)
- QUINNS_MEMORY_API_KEY (for memory server access)
- QUINNS_MEMORY_PROFILE (for memory server profile)

### Additional Keys (as configured in Railway)
- ANTHROPIC_API_KEY
- OPENAI_API_KEY
- GOOGLE_KEY