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
          mongo_db_uri: ${{ secrets.MONGO_DB_URI }}
          db_name: 'your_database_name'
          google_cloud_project: ${{ secrets.GOOGLE_CLOUD_PROJECT }}
          sa_key_json: ${{ secrets.GCP_SA_KEY }}
```

## ⚙️ Configuration

| Input | Required | Description | Default |
|-------|----------|-------------|---------|
| `document_repo_url` | ✅ | Target documentation repository (`owner/repo`) | - |
| `document_repo_token` | ✅ | GitHub token with write access to docs repository | - |
| `agent_release_token` | ✅ | GitHub token with read access to agent releases | - |
| `mongo_db_uri` | ✅ | MongoDB connection URI | - |
| `db_name` | ✅ | MongoDB database/collection name | - |
| `google_genai_use_vertexai` | ❌ | Use Vertex AI (`true`) or Gemini API key (`false`) | `true` |
| `google_cloud_project` | ❌ | Google Cloud project name (required if using Vertex AI) | - |
| `google_cloud_location` | ❌ | Google Cloud project location | `global` |
| `sa_key_json` | ❌ | Service Account key JSON (used to set `GOOGLE_APPLICATION_CREDENTIALS`) | - |
| `google_api_key` | ❌ | Gemini API key (required if not using Vertex AI) | - |
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

### 3. Configure Google AI Services

You have two options for Google AI authentication:

#### Option A: Google Vertex AI (Recommended)

- Set up a Google Cloud Project
- Enable the Vertex AI API
- Create a service account with appropriate permissions
- Download the service account key (JSON file)

#### Option B: Google Gemini API Key

- Navigate to [Google AI Studio](https://aistudio.google.com/apikey)
- Create a new API key
- Set `google_genai_use_vertexai: 'false'` in your workflow

### 4. Get Agent Release Access Token

Request a GitHub token with read access to the agent releases from the dev team.

### 5. Configure GitHub Secrets

Create an environment in your GitHub repository (`Settings > Environments`) and add these secrets:

| Secret | Description | Required When |
|--------|-------------|---------------|
| `DOCUMENT_REPO_TOKEN` | GitHub token with write access to your documentation repository | Always |
| `AGENT_RELEASE_TOKEN` | GitHub token with read access to the agent's latest release | Always |
| `MONGO_DB_URI` | MongoDB connection string | Always |
| `GOOGLE_CLOUD_PROJECT` | Google Cloud project name | Using Vertex AI |
| `GCP_SA_KEY` | Google service account key JSON content | Using Vertex AI |
| `GOOGLE_API_KEY` | Google Gemini API key | Using Gemini API |

### Configuration Examples

#### Using Vertex AI (Default)

```yaml
with:
  document_repo_url: 'your-org/your-docs-repo'
  document_repo_token: ${{ secrets.DOCUMENT_REPO_TOKEN }}
  agent_release_token: ${{ secrets.AGENT_RELEASE_TOKEN }}
  mongo_db_uri: ${{ secrets.MONGO_DB_URI }}
  db_name: 'your_database_name'
  google_cloud_project: ${{ secrets.GOOGLE_CLOUD_PROJECT }}
  sa_key_json: ${{ secrets.GCP_SA_KEY }}
```

Behind the scenes, the action writes `sa_key_json` to a temp file and exports:

```bash
GOOGLE_APPLICATION_CREDENTIALS=$RUNNER_TEMP/gcp-key.json
```

This makes Application Default Credentials available to the agent without you managing files yourself.

#### Using Gemini API Key

```yaml
with:
  document_repo_url: 'your-org/your-docs-repo'
  document_repo_token: ${{ secrets.DOCUMENT_REPO_TOKEN }}
  agent_release_token: ${{ secrets.AGENT_RELEASE_TOKEN }}
  mongo_db_uri: ${{ secrets.MONGO_DB_URI }}
  db_name: 'your_database_name'
  google_genai_use_vertexai: 'false'
  google_api_key: ${{ secrets.GOOGLE_API_KEY }}
```

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
