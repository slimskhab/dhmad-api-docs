# DHMAD API Documentation

This folder contains the Mintlify documentation for the DHMAD API.

## Structure

- `mint.json` - Mintlify configuration file
- `*.mdx` - Documentation pages in MDX format
- Organized by sections: Introduction, Getting Started, API Reference, Guides, etc.

## Local Development

To preview the documentation locally with Mintlify:

1. Install Mintlify CLI:
   ```bash
   npm i -g mintlify
   ```

2. Navigate to the docs folder:
   ```bash
   cd docs
   ```

3. Start the preview server:
   ```bash
   mintlify dev
   ```

4. Open http://localhost:3000 in your browser

## Deployment

Mintlify doesn't have a `deploy` command. Deployment happens automatically when you:

1. **Connect your GitHub repository** to Mintlify via the [Mintlify Dashboard](https://app.mintlify.com)
2. **Push changes** to your repository - Mintlify automatically detects changes and deploys
3. **Manual deployment** can be triggered from the Mintlify dashboard if needed

### Steps to Deploy:

1. Go to [app.mintlify.com](https://app.mintlify.com) and sign in
2. Create a new documentation site or connect your existing repository
3. Install the Mintlify GitHub App on your repository
4. Push your changes to GitHub - Mintlify will automatically build and deploy

The documentation will be available at the URL provided by Mintlify (e.g., `your-docs.mintlify.app`)

## Adding New Pages

1. Create a new `.mdx` file in the appropriate folder
2. Add the page to `mint.json` navigation
3. Use Mintlify components for rich formatting

## Components Available

- `<Card>` - Information cards
- `<CardGroup>` - Group of cards
- `<CodeGroup>` - Code examples with tabs
- `<Warning>` - Warning callouts
- `<Info>` - Information callouts
- `<Note>` - Note callouts
- `<Accordion>` - Collapsible content
- `<Endpoint>` - API endpoint documentation
- `<ParamField>` - Parameter documentation

See [Mintlify Documentation](https://mintlify.com/docs) for more components and features.
