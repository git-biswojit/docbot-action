# README Automation GitHub Action

Automatically generate and update your API documentation using AI-powered analysis of your codebase.

## ✨ Features

For detailed feature information, visit the [main repository](https://github.com/git-biswojit/readme_automation/blob/main/README.md).

## 🚀 Quick Start

### Basic Usage

Add this action to your workflow:

```yaml
name: Update Documentation

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  update-docs:
    runs-on: ubuntu-latest
    environment: your-environment-name
    steps:
      - name: Update Documentation
        uses: git-biswojit/docbot-action@v1
        with:
          document_repo_url: 'your-org/your-docs-repo'
          document_repo_token: ${{ secrets.DOCUMENT_REPO_TOKEN }}
          agent_release_token: ${{ secrets.AGENT_RELEASE_TOKEN }}
          google_api_key: ${{ secrets.GOOGLE_API_KEY }}
          mongo_db_uri: ${{ secrets.MONGO_DB_URI }}
          db_name: 'your_database_name'
```

## ⚙️ Configuration

| Input | Required | Description | Default |
|-------|----------|-------------|---------|
| `document_repo_url` | ✅ | Target documentation repository (`owner/repo`) | - |
| `document_repo_token` | ✅ | GitHub token with write access to docs repository | - |
| `agent_release_token` | ✅ | GitHub token with read access to agent releases | - |
| `google_api_key` | ✅ | Google Gemini API key | - |
| `mongo_db_uri` | ✅ | MongoDB connection URI | - |
| `db_name` | ✅ | MongoDB database name | - |
| `source_repo_path` | ❌ | Local path for source repository | `api` |
| `readme_repo_path` | ❌ | Local path for documentation repository | `readme-doc` |

## 🔧 Setup Guide

### 1. Create Documentation Repository

Create a separate repository for your documentation (e.g., `your-org/your-docs-repo`). This repository will contain:

- Generated OpenAPI specifications
- Readme.com compatible documentation
- Sidebar configuration

Connect your Readme.com project to this repository.

### 2. Set Up MongoDB Database

Create a MongoDB database to store:

- Generated OpenAPI specifications
- Change tracking information
- Processing state

### 3. Obtain Google Gemini API Key

- Navigate to [Google AI Studio](https://aistudio.google.com/apikey)
- Create a new API key

### 4. Get Agent Release Access Token

Request a GitHub token with read access to the agent releases from the dev team.

### 5. Configure GitHub Secrets

Create an environment in your GitHub repository (`Settings > Environments`) and add these secrets:

| Secret | Description |
|--------|-------------|
| `DOCUMENT_REPO_TOKEN` | GitHub token with write access to your documentation repository |
| `AGENT_RELEASE_TOKEN` | GitHub token with read access to the agent's latest release |
| `GOOGLE_API_KEY` | Google Gemini API key |
| `MONGO_DB_URI` | MongoDB connection string |

## 🛠️ Troubleshooting

### Common Issues

#### Action Fails to Start

- Verify all required secrets are configured
- Ensure you're specifying the correct environment in your workflow
- Check repository permissions
- Confirm MongoDB is accessible

#### Documentation Not Updated

- Check if your API controllers have changed
- Verify your Google API key is valid and active
- Review MongoDB connection settings

#### Permission Errors

- Ensure `DOCUMENT_REPO_TOKEN` has write access to your documentation repository
- Check repository visibility settings
- Verify token permissions are correctly configured

### Debug Mode

Enable debug logging by setting the `ACTIONS_STEP_DEBUG` environment variable:

```yaml
env:
  ACTIONS_STEP_DEBUG: true
```

## 📚 Additional Resources

- [Main Repository](https://github.com/git-biswojit/readme_automation) - Source code and local development
- [Readme.com Documentation](https://docs.readme.com/) - Learn more about Readme.com integration

---

Made with ❤️ and craft 🪄 by some Incubees
