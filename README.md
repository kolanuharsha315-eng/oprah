# oprah
AI powered B2B Outreach Platform 
AI-Powered Business Growth Workspace

Open your web

Oprah is a full-stack AI business-growth workspace designed for small businesses that want to discover opportunities, create personalized marketing content, and answer common customer questions from one focused dashboard.

The current demonstration workspace is configured for Royal Spice Catering, a sample catering business in Hyderabad. The product is presented in Demo Mode so users can safely explore the end-to-end experience without sending real emails, scraping private data, publishing advertisements, or placing real phone calls.

Why Oprah exists

Small-business growth work is often scattered across spreadsheets, email drafts, social-media tools, and repetitive customer conversations. Oprah brings these workflows together in a simple operating workspace:

Growth need
Oprah workflow
Discover relevant prospects and event opportunities
Lead Finder
Prepare personalized first-touch messages
AI Outreach
Produce campaign-ready promotional copy
Ad Studio
Answer recurring customer questions consistently
AI Receptionist Prototype
Keep business facts and activity organized
Business Profile and Overview dashboard


Core features

Growth dashboard

The Overview dashboard presents the business workspace, Demo Mode status, growth KPIs, opportunity status, and recent activity in a responsive SaaS interface.

Lead Finder

Lead Finder provides curated demo leads for event planners, conference organizers, and other potential catering partners. Users can search records, inspect lead status, select up to ten leads, and start an outreach workflow.

Demo leads are intentionally curated sample data. A production implementation should connect a legitimate, permissioned lead-data source rather than scrape private information.

AI Outreach

AI Outreach generates personalized email drafts using the business profile, services, organization name, and event type. Users can review the subject and body, copy the draft, or open it in an email client. Oprah does not claim to send the email automatically.

Ad Studio

Ad Studio turns a short campaign brief into four content formats:

•
Instagram advertisement copy.

•
WhatsApp promotional text.

•
Google-style headline and description.

•
Poster copy for print or digital placement.

Each result indicates the active AI provider so users can distinguish live generation from Demo Mode fallback behavior.

AI Receptionist Prototype

The AI Receptionist Prototype accepts typed customer questions and simulated speech input. It uses the business knowledge base and conversation history to generate answers about topics such as opening hours, bulk orders, services, pricing, and bookings.

This is a prototype for the conversational workflow. It is not a live telephone system and does not currently connect to telephony infrastructure.

Business Profile

The Business Profile section stores the business name, category, description, location, contact details, opening hours, and services. These values provide context for the outreach, advertising, and receptionist workflows.

Mandatory AI provider: Featherless

Featherless is the required AI provider for Oprah's live generation workflows. It is used server-side for:

1.
Personalized outreach email generation.

2.
Instagram, WhatsApp, Google-style, and poster-copy generation.

3.
AI Receptionist responses grounded in Royal Spice Catering's knowledge base.

The configured model is:

Qwen/Qwen3.5-9B



The application checks for both FEATHERLESS_API_KEY and FEATHERLESS_MODEL. When both values are available, the server sends requests to the Featherless OpenAI-compatible chat-completions endpoint. The integration uses the documented chat_template_kwargs option with enable_thinking: false so short business-copy requests return usable content efficiently rather than spending the response budget on a visible reasoning process.

The API key is kept on the server and is never placed in client-side code. Requests also include an application title and referring project URL for provider-side identification.

Featherless environment variables

Create a local environment configuration with:

Plain Text


FEATHERLESS_API_KEY=your_featherless_api_key
FEATHERLESS_MODEL=Qwen/Qwen3.5-9B



The API key must be active and have access to the selected model. The model identifier must match a model available through the Featherless catalog and the account's plan.

Provider fallback behavior

If Featherless credentials are unavailable, or a live request fails, Oprah switches to a clearly labeled Mock AI · Demo Mode provider. This fallback keeps the demonstration usable and makes the provider state visible instead of pretending that a live model response was generated.

Technology architecture

Oprah uses a typed full-stack architecture:

Layer
Responsibility
React frontend
Responsive dashboard, forms, cards, tables, speech-input controls, and workflow navigation
TypeScript server
Server-side business logic and AI provider abstraction
tRPC procedures
Typed contracts between the frontend and backend
Relational database
Business profiles, leads, drafts, advertisements, knowledge-base entries, and conversations
Featherless API
Live AI generation for outreach, advertising, and receptionist responses
Browser Speech Recognition
Optional simulated transcription for receptionist testing




The central provider abstraction lives in server/aiService.ts. It exposes the provider state and keeps Featherless request handling separate from the UI and business procedures.

Data model

The application includes database tables for:

•
Business profiles.

•
Leads and lead statuses.

•
Outreach drafts.

•
Advertisement generations.

•
Knowledge-base entries.

•
Receptionist conversations.

Royal Spice Catering demo leads are seeded on first use so Lead Finder can demonstrate both the curated sample workflow and stored-record retrieval.

Local setup

Requirements

Install the following before starting:

Node.js 22 or newer.

•
pnpm 10 or newer.

•
A MySQL-compatible relational database.

•
A Featherless API key with access to the configured model.

Install dependencies

Bash


pnpm install



Configure environment variables

Set the required database, authentication, and Featherless variables in the local environment. At minimum, the AI provider requires:

Plain Text


FEATHERLESS_API_KEY=your_featherless_api_key
FEATHERLESS_MODEL=Qwen/Qwen3.5-9B



Do not commit API keys or other secrets to source control.

Prepare the database

Generate and apply the database migration using the project's database workflow, then start the development server:

Bash


pnpm drizzle-kit generate
pnpm dev



Validate the project

Bash


pnpm check
pnpm test



The test suite covers authentication behavior, provider behavior, persisted demo procedures, seeded lead retrieval, outreach, advertising, receptionist responses, knowledge-base access, and live Featherless credential validation.

Three-minute demonstration

1.
Open the live workspace using Open your web.

2.
Confirm the Demo Mode active indicator and review the Overview KPI cards.

3.
Open Lead Finder, search for a wedding-related opportunity, and select a lead.

4.
Generate an AI Outreach draft and use the copy or open-email-client action.

5.
Open Ad Studio, enter an offer such as Weekend catering, and generate the four ad formats.

6.
Open AI Receptionist, ask Do you take bulk orders?, and review the saved conversation.

7.
Try the microphone control to simulate a spoken customer question if browser speech recognition is available.

8.
Open Business Profile to review the Royal Spice Catering context used by the AI workflows.

Demo Mode and production boundaries

Demo Mode is a deliberate product-safety boundary. It uses Royal Spice Catering sample data and does not represent a live business account. The current release does not send email, publish advertisements, scrape private lead data, make phone calls, or provide a production-grade customer-support guarantee.

Before using Oprah with real businesses, add authenticated multi-business workspaces, authorization policies, audit logging, a verified lead provider, production email delivery, usage controls, stronger data-retention rules, and a real telephony provider for the receptionist experience.

client/
  src/pages/Home.tsx       Dashboard and workflow UI
  src/components/          Reusable interface components
  src/index.css            Global visual system
server/
  aiService.ts             Featherless and Mock AI provider abstraction
  routers.ts               Typed application procedures
  db.ts                    Database access helpers
dizzle/
  schema.ts                Relational data model
OPRAH_DEMO_GUIDE.md        Detailed setup 
todo.md                    Implementation history and verification checklist

License and responsible use

This project is intended as a product demonstration and foundation for further development. Keep credentials private, obtain permission before contacting leads, use only lawful and permissioned data sources, and review AI-generated content before publishing or sending it.



