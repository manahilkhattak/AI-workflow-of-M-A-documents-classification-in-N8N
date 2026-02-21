# AI-workflow-of-M&A-documents-classification-in-N8N
*AutoMate Multi-Agent LLM Workflow*

An AI-powered document intelligence system built in n8n that automatically detects M&A activity, extracts companies, classifies industries, and flags IT/technology relevance with justification.
*Problem Statement*
Financial institutions receive hundreds of documents daily — press releases, SEC filings, annual reports, regulatory notices. Manually identifying which documents relate to mergers and acquisitions, and which companies involved are technology firms, is slow, inconsistent, and doesn't scale.
This workflow solves that problem with a fully automated, auditable pipeline.

*What It Does*

This n8n workflow analyzes any PDF document and outputs:

M&A Detection: Identifies whether the document discusses merger or acquisition activity
Company Extraction: Finds all companies mentioned and classifies their role (Acquirer, Target, Advisor)
Industry Classification: Maps each company to industry categories (SaaS, AI/ML, Fintech, Cybersecurity, etc.)
Technology Relevance Flags: Scores each company 0-10 on tech relevance with plain-English justification
Structured Report: Complete JSON output with analysis summary and recommendations

End-to-end execution time: < 60 seconds
Manual steps required: 0

*Architecture*

The workflow uses parallel processing with two paths: one for M&A documents and one for non-M&A documents.
Main Pipeline:
1. Start Node
Manual trigger to execute the workflow

2. Google Drive → Download File
Automatically downloads the target PDF from Google Drive
No manual file upload required

3. Extract Text from File
Uses n8n's built-in PDF parser
Extracts all text content from the document

4. Chunk & Score M&A
JavaScript node splits text into paragraphs
Scores each chunk by M&A keyword frequency (merger, acquisition, target company, deal value, synergies, regulatory approval, due diligence)
Sorts chunks by relevance score descending
Path A: M&A Document Processing (when M&A indicators detected)

5. LLM — M&A Detector (Groq API)
Analyzes scored chunks using AI
Determines if document contains genuine M&A activity
Returns structured decision: M&A relevant or not

6. If_ma Node (Decision Router)
Routes to M&A analysis path if true
Routes to non-M&A report if false

7. Parse JSON
Parses LLM output into structured format
Cleans escaped characters and validates JSON

8. Extract Companies & Roles (JavaScript)
Pattern-based company name detection
Role classification: Acquirer, Target, Financial Advisor, Other
Context-aware analysis using surrounding text

9. Industry Classification (JavaScript)
Maps each company to curated taxonomy
Coverage: SaaS, AI/ML, Fintech, Cybersecurity, Cloud Computing, Logistics, Supply Chain, and more
Outputs primary industry + sub-industries array

10. IT Tech Flags (JavaScript)
Scores each company 0-10 on technology relevance
Sets boolean is_tech_relevant flag
Assigns relevance level: High, Medium, Low
Cites specific indicators found (SaaS, cloud, AI/ML, etc.)

11. LLM — Justification Generator (Groq API)
Takes all classification data
Generates human-readable justification for each company's tech score
Explains why the company was flagged or not flagged

12. Parse Justification JSON
Parses AI-generated justifications
Validates and structures the output

13. Merge Node
Combines classification data with AI-generated justifications
Creates unified company profiles

14. Final M&A Report (JavaScript)
Aggregates all data into final structured output
Generates analysis summary with statistics
Creates actionable recommendations array

*Tech Stack*

Workflow Engine: n8n (open-source workflow automation)
AI Model: Groq API (Llama 3)
Core Logic: JavaScript (chunking, scoring, filtering, classification)
Document Source: Google Drive
Output Format: JSON

*Requirements*

To run this workflow, you need:
n8n installed (self-hosted or n8n Cloud)
Installation: https://docs.n8n.io/hosting/installation/

Google Drive credentials configured in n8n

Groq API key (free tier available)
Sign up: https://console.groq.com/

*Installation*

Step 1: Import the Workflow
Open n8n
Click "Workflows" → "Import from File"
Select AutoMate Multi-Agent LLM Workflow.json
Click "Import"

Step 2: Configure Credentials
Google Drive Node:
Click the node
Add your Google Drive OAuth2 credentials
Test the connection


Groq AI Agent Node:
Click the node
Add your Groq API key
Select model: llama3-70b-8192 (recommended)



Step 3: Set Your Document Path
Click the Google Drive node
Update the file path or file ID to point to your test PDF
Save

Step 4: Execute
Click "Execute Workflow" and watch the nodes light up green one by one.

*Built By*

This workflow was built in 24 hours during a hackathon:

Manahil Khattak (GitHub)
Abu Bakar
Unza Munaf

*Issues & Contributions*

Found a bug? Have an improvement idea? Open an issue or submit a pull request.

This workflow is maintained and we're happy to help troubleshoot.

