# n8n Workflow Templates

A collection of production n8n workflow templates for AI automation, content pipelines,
email infrastructure, and system integrations.

All workflows are sanitized — webhook URLs, API keys, and credentials are replaced with
placeholders. Import the JSON, configure your credentials, and run.

---

## Workflows

### Content Pipeline

**`content-pipeline/article-importer.json`**
Watches for new WordPress posts and pulls article content via the WordPress REST API.
Triggers the content generation subworkflow with the extracted text and metadata.

**`content-pipeline/post-generator.json`**
Takes article content and generates platform-specific social media posts using an LLM.
Outputs formatted posts for Facebook, LinkedIn, and Twitter/X.

**`content-pipeline/image-subworkflow.json`**
Generates or fetches a matching image for a social post. Integrates with an image
generation API and returns the image URL to the parent workflow.

**`content-pipeline/approval-handler.json`**
Sends generated posts to a Telegram bot for human review before publishing.
On approval, triggers the Buffer publishing step. On rejection, discards the post.

---

### AI Agent Bridge

**`agent-bridge/sql-webhook-proxy.json`**
Receives SQL queries from an AI agent, validates them against 8 security rules
(SELECT-only, no DDL, no system tables, etc.), executes against your database,
and returns rows as JSON. This is the n8n side of the zero-credential agent pattern.

**`agent-bridge/document-indexer.json`**
Indexes documents into a pgvector database for semantic search. Uses a watermark
pattern to only process new documents since the last run. Handles PDF, DOCX, and
other formats via Apache Tika extraction.

---

### Email Infrastructure

**`email/domain-warmup-sequence.json`**
Manages a gradual domain warming sequence for new sending domains. Increments daily
send volume on a schedule and logs delivery metrics.

**`email/zepto-migration.json`**
Migrates transactional email sending from an existing provider to ZeptoMail.
Handles template mapping and delivery confirmation logging.

---

## How to Use

1. Open your n8n instance
2. Go to Workflows > Import from File
3. Select the JSON file for the workflow you want
4. Configure credentials in each node (API keys, DB connections, webhook secrets)
5. Update webhook URLs to match your n8n instance
6. Activate the workflow

---

## Environment Setup

Each workflow has a matching `env-notes.md` in its folder explaining what credentials
and configuration values it needs.

---

Built by [Abdelrahman Ali](https://rewireweb.com)
