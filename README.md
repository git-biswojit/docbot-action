# README Automation GitHub Action

## ✨ Features

Get the feature details from the [main repo](https://github.com/git-biswojit/readme_automation/blob/main/README.md)

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
    env: specify the env you're using (eg:test)
    steps:
      - name: Update Documentation
        uses: git-biswojit/docbot-action@v1
        with:
          document_repo_url: 'your-org/your-docs-repo'
          document_repo_token: ${{ secrets.DOCUMENT_REPO_TOKEN }}
          agent_release_token: ${{ secrets.AGENT_RELEASE_TOKEN }}
          google_api_key: ${{ secrets.GOOGLE_API_KEY }}
          mongo_db_uri: ${{ secrets.MONGO_DB_URI }}
          db_name: 'your_db_name'
```

## ⚙️ Inputs

| Input | Required | Description | Default |
|-------|----------|-------------|---------|
| `document_repo_url` | ✅ | Target docs repo (`owner/repo`) to clone/push to | - |
| `document_repo_token` | ✅ | Token with write access to docs repo | - |
| `agent_release_token` | ✅ | Token to download the latest release wheel | - |
| `google_api_key` | ✅ | Google Gemini API key | - |
| `mongo_db_uri` | ✅ | MongoDB connection URI | - |
| `db_name` | ✅ | MongoDB database name | - |
| `source_repo_path` | ❌ | Checkout path for source repo | `api` |
| `readme_repo_path` | ❌ | Checkout path for docs repo | `readme-doc` |

## 🔧 Setup

### 1. Secrets


Add these secrets to your :

| Secret | Description |
|--------|-------------|
| `DOCUMENT_REPO_TOKEN` | GitHub token with write access to your documentation repository |
| `AGENT_RELEASE_TOKEN` | Github token with read access to the agent's latest release |
| `GOOGLE_API_KEY` | Google Gemini API key |
| `MONGO_DB_URI` | MongoDB connection string |

### 2. Documentation Repository

Create a separate repository for your documentation (e.g., `your-org/your-docs-repo`). This repository will contain:

- Generated OpenAPI specifications
- readme.com compatible documentation
- Sidebar configuration

### 3. MongoDB Database

Set up a MongoDB database to store:

- Generated OpenAPI specifications
- Change tracking information
- Processing state

## 🛠️ Troubleshooting

### Common Issues

#### Action Fails to Start

- Check all required secrets are set
- Check you are providing the respective environment to the run
- Verify repository permissions
- Ensure MongoDB is accessible

#### Documentation Not Updated

- Check if controllers have changed
- Verify Google API key is valid
- Review MongoDB connection

#### Permission Errors

- Ensure `DOCS_REPO_TOKEN` has write access
- Check repository visibility settings
- Verify token permissions

### Debug Mode

Enable debug logging by setting the `ACTIONS_STEP_DEBUG` secret to `true`:

```yaml
env:
  ACTIONS_STEP_DEBUG: true
```

### Access the [Main Repository](https://github.com/git-biswojit/readme_automation) for source code and local development

Made with ❤️ and craft 🪄 by some Incubees
