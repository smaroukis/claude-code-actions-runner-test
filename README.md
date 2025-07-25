# Claude Code GitHub Actions + Netlify Demo

This repository demonstrates how to set up Claude Code to automatically run in GitHub Actions when users mention `@claude` on issues, with automatic deployment to Netlify.

## How It Works

1. **Issue Trigger**: When someone mentions `@claude` followed by a request in GitHub issues or comments
2. **Claude Responds**: GitHub Actions runs Claude Code to process the request
3. **Auto Deploy**: Changes are automatically deployed to Netlify for live preview

## Setup Instructions

### 1. GitHub Repository Setup

1. **Fork or clone this repository**
2. **Set up Claude Code OAuth Token**:
   - Install the Claude Code GitHub App: `/install-github-app` (see Authentication Notes below)
   - Or manually add the OAuth token:
     - Go to your repository → Settings → Secrets and variables → Actions
     - Click "New repository secret"
     - Name: `CLAUDE_CODE_OAUTH_TOKEN`
     - Value: Your Claude Code OAuth token (you can choose to link either your chat/anthropic account or api/console account)

3. **Enable GitHub Actions**:
   - Ensure Actions are enabled in your repository settings
   - The workflow file `.github/workflows/claude.yml` will automatically trigger on `@claude` mentions

### 2. Netlify Deployment Setup

1. **Connect to Netlify**:
   - Go to [netlify.com](https://netlify.com) and sign in
   - Click "Add new site" → "Import an existing project"
   - Connect your GitHub account and select this repository

2. **Configure build settings**:
   - Build command: (leave empty - static files only)
   - Publish directory: `/` (root directory)
   - Click "Deploy site"

3. **Optional: Custom domain**:
   - In Netlify dashboard → Site settings → Domain management
   - Add your custom domain

### 3. Usage

1. **Create an issue** in your GitHub repository with `@claude` in the title/body, OR comment with `@claude`:
   - **In issue body**: Create issue with `@claude Please create a simple calculator function`
   - **In comment**: Comment `@claude Please create a simple calculator function`
2. **Claude will respond** by creating a new branch with the requested changes
3. **Changes auto-deploy** to your Netlify site for immediate preview

## Example Requests

Try adding these to issue bodies or comments:

- `@claude Create a simple todo list app`
- `@claude Add a contact form to the website`
- `@claude Generate a responsive navigation bar`
- `@claude Create a dark mode toggle`

## Authentication Notes

**GitHub CLI Authentication Issues:**
- The main authentication challenge occurs when using `/install-github-app` to install the Claude Code GitHub App
- You may need to authenticate the GitHub CLI client first before installation works properly
- Run `gh auth login` if you encounter authentication issues during app installation

**Account Linking Options:**
- You can choose to link Claude Code to either your **chat/anthropic account** or your **api/console account**
- Both options work - choose based on your preference and existing Anthropic account setup
- The choice affects billing and usage tracking but not functionality

## Configuration Options

You can customize the GitHub Action behavior in `.github/workflows/claude.yml`:

```yaml
- uses: anthropics/claude-code-action@beta
  with:
    claude_code_oauth_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
    trigger_phrase: "@claude"           # Change trigger phrase (default: @claude)
    model: "claude-sonnet-4-20250514"   # Specify model (default: Sonnet 4)
    allowed_tools: "Read,Bash(git *:*)" # Restrict tool usage
    custom_instructions: |              # Add project-specific instructions
      Follow our coding standards
      Ensure all new code has tests
```

## File Structure

```
├── .github/workflows/
│   ├── claude.yml          # Main Claude Code workflow
│   └── claude-code-review.yml  # Automated PR reviews (optional)
├── index.html              # Demo webpage
├── generated.js            # Generated by Claude (initially empty)
└── README.md               # This file
```

## Security Notes

- The action runs with limited permissions (`contents: write, pull-requests: write`)
- Claude operates in a sandboxed environment
- All changes are made in separate branches for review
- Consider restricting tool usage for additional security

## Troubleshooting

**Claude doesn't respond to comments:**
- Check that your `CLAUDE_CODE_OAUTH_TOKEN` secret is set correctly
- Ensure the comment contains `@claude` (mentioning Claude)
- Verify GitHub Actions are enabled

**Netlify deployment fails:**
- Check build logs in Netlify dashboard
- Ensure repository is connected and accessible
- Verify file permissions and structure

## Support

For Claude Code issues: [github.com/anthropics/claude-code/issues](https://github.com/anthropics/claude-code/issues)