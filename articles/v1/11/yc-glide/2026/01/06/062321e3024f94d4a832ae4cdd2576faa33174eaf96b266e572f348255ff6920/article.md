---
schema_version: "1.0.0"
document_id: "062321e3024f94d4a832ae4cdd2576faa33174eaf96b266e572f348255ff6920"
company_key: "yc-glide"
company: "Glide"
source_id: "yc-glide-news-import-b50025bbdd3a"
canonical_url: "https://www.glideapps.com/blog/excel-agent-mode"
published_at: "2026-01-27T12:00:00+00:00"
first_seen_at: "2026-07-21T21:51:19.848750+00:00"
fetched_at: "2026-07-28T22:23:04.727589+00:00"
content_hash: "sha256:8454d7b1d17bd685a84e4831bf2eb7eb7488aea2c5cebe4a013bba843a191442"
---

# Intro to the new Agent Mode in Excel

Microsoft Excel just got significantly smarter with the general availability of[Agent Mode](https://support.microsoft.com/en-us/office/agent-mode-in-excel-a2fd6fe4-97ac-416b-b89a-22f4d1357c7a) . With Agent Mode, Copilot adds an AI-powered chat feature to Excel that can build, analyze, and modify spreadsheets through natural language commands. Instead of just answering questions about your data, Agent Mode can create complete workbooks, build financial models, and generate complex reports from a simple prompt.


Here's what you need to know about Agent Mode, when to use it, when not to use it, and what alternatives, like the AI-powered no-code platform[Glide](https://www.glideapps.com/) , exist when you need to extend your data’s AI capabilities beyond a single Excel spreadsheet or collaborate more safely with your Excel data.


Read Microsoft's[Agent Mode announcement post](https://techcommunity.microsoft.com/blog/excelblog/agent-mode-in-excel-is-now-generally-available-on-desktop/4457408) or[watch the video](https://www.youtube.com/watch?v=uDv8B_N_V1A) for more info.


## What Is Agent Mode?


Agent Mode is an AI capability within Microsoft 365 Copilot that works directly inside Excel. Unlike traditional Copilot features that respond to single requests, Agent Mode takes a more autonomous approach: it plans multi-step tasks, executes them, checks its work, and iterates until the results meet your request.


For example, instead of manually creating a budget tracker with input fields, formulas, charts, and conditional formatting, you can tell Agent Mode: "Create a budget tracker with planned vs. actual spending, calculate variances, apply conditional formatting, and add a donut chart showing category breakdown." Agent Mode will build the entire workbook for you.


The key difference is, Agent Mode doesn't just assist. It acts. It creates tables, writes formulas, builds PivotTables, generates charts, applies formatting, and even pulls external data from the web when needed. You can watch its reasoning in real time and steer it as it works.


## A Brief History of Copilot’s Agent Mode


**September 2025:** Microsoft announced Agent Mode as part of "vibe working"—a new pattern of human-AI collaboration focused on iterative, side-by-side work in Office apps.


**October-November 2025:** Agent Mode launched in public preview for Excel on the web through Microsoft's Frontier early access program. Microsoft also added two major capabilities:


- Web search integration for pulling external data into spreadsheets
- Support for Anthropic's Claude models alongside OpenAI models


**December 2025:** Agent Mode became generally available for Excel on the web for users with Microsoft 365 Copilot licenses or Microsoft 365 Premium subscriptions.


**January 27, 2026:** Agent Mode became generally available on Excel for Windows and Mac ([today's announcement](https://x.com/msexcel/status/2016175955232838034?s=46&t=od5ex59zlSmrAe6YgWBYIg) ).


## How to Use Agent Mode


**Requirements:**


- Microsoft 365 Copilot license (commercial) OR Microsoft 365 Premium subscription OR Microsoft 365 Personal/Family subscription (using AI credits)
- Excel for Web, Windows, or Mac
- Currently not available in EU or UK


**Steps:**


1. **Open Excel** (web, Windows, or Mac version)
2. **Open or create a workbook** where you want Agent Mode to work
3. **Open Copilot** from the Home tab
4. **Select Tools > Agent Mode** from the Copilot menu
5. **Choose your model** (optional): You can select between OpenAI (GPT 5.2), Anthropic (Claude Opus 4.5), or Auto mode (Copilot chooses for you)
6. **Enter your prompt** using natural language
7. **Watch Agent Mode work** : You'll see its reasoning process in the Copilot pane as it plans, executes, and validates each step
8. **Iterate and refine** : Give additional instructions to modify the work ("Add a trends line chart," "Change variances to percentages," etc.)


**Example prompts that work well:**


- "Create an annual financial report for a retail business with monthly revenue breakdown, expenses, profit margins, and year-over-year growth charts. Use dummy data."
- "Build a loan calculator that shows monthly payments based on loan amount, interest rate, and term. Include an amortization schedule."
- "Analyze this sales data. I need to understand important insights to help make business decisions. Make it visual."
- "Create a household budget tracker with categories for rent, groceries, utilities, entertainment, transportation, and savings. Add spending visualizations."


Agent Mode analyzes your request, creates a step-by-step plan, executes that plan directly in your workbook, and validates the results. The entire process is visible. You can see what it's thinking and what it's doing.


## What to use Agent Mode for


Agent Mode excels at complex, multi-step tasks that would normally require significant Excel expertise:


**Workbook creation from scratch:**


- Financial models and forecasts
- Budget trackers and expense reports
- Data analysis dashboards
- Loan calculators and amortization schedules
- Inventory tracking systems
- Project timelines and resource planners


**Data transformation and analysis:**


- Reshaping messy data into structured tables
- Merging data from multiple sheets
- Creating summary reports with calculations and visualizations
- Building PivotTables and pivot charts
- Applying complex formulas across datasets


**Visualization and reporting:**


- Generating charts and graphs from data
- Creating formatted reports with multiple visual elements
- Building interactive dashboards
- Applying conditional formatting rules


**External data integration:**


- Pulling current information from the web (GDP data, stock prices, statistics)
- Creating tables with up-to-date external data
- Source citations for transparency


**Tasks requiring iteration:**


- Projects where you need to refine the output through conversation
- Scenarios where you're not entirely sure what the final result should look like
- Exploratory data analysis, where you want to try different approaches


Agent Mode excels at more complex and labor-intensive tasks. If you'd normally spend 30 minutes building something manually, Agent Mode can often do it in 2-3 minutes.


## What Agent Mode is not good for


Despite its capabilities, Agent Mode has clear limitations you should understand:


**Simple, one-step tasks:**


- Inserting a single chart or PivotTable (use Excel's built-in Recommended Charts or Recommended PivotTables instead—they're faster)
- Basic formatting changes
- Quick calculations


**Conversational help:**


- Questions about how to do something in Excel
- Explaining Excel concepts
- General troubleshooting (Use regular Microsoft 365 Copilot Chat for these)


**Working with shared or sensitive files:**


This is a critical limitation. Agent Mode makes direct changes to your workbook in real-time. There's no preview mode where you review changes before they're applied. While you can see what it's doing and can stop it at any time, the modifications happen immediately in the file.


This creates issues when:


- **Multiple people collaborate** on the same file. Agent Mode changes can conflict with others' work
- **Files contain sensitive data** you don't want modified without careful review
- **Workbooks are templates** or standardized formats that shouldn't be altered
- **Files require approval** before changes (budgets, financial reports, compliance documents)
- **You're working on the only copy** of important data without backups


**Cross-file or cross-system work:**


Agent Mode only works with the currently open workbook. It cannot:


- Access data from other Excel files
- Pull information from your emails
- Connect to enterprise databases or business systems
- Work with data from CRMs, ERPs, or other business applications
- Sync information across multiple spreadsheets


If your workflow involves data from multiple sources or systems, Agent Mode won't be able to help.


**Mobile-first scenarios:**


While Agent Mode works in Excel on the web (which is accessible on mobile devices), it's not designed for mobile-first workflows where field teams need simplified interfaces optimized for phones and tablets.


**Scenarios requiring approval workflows:**


Agent Mode doesn't have built-in approval processes. If you need changes reviewed before they're finalized, you'll need to manage that separately.


## When Agent Mode isn't the right AI tool, use Glide instead


For scenarios where Agent Mode's limitations create problems, Glide offers a different approach to using AI with Excel data. It is also one that addresses Agent Mode's key weaknesses. Glide is a[no-code platform](https://www.glideapps.com/blog/no-code) that can be used to[turn an Excel spreadsheet into an app](https://www.glideapps.com/blog/excel-spreadsheet-to-app) . You can use[Glide AI](https://www.glideapps.com/ai) to apply AI actions and automations to your Excel data in very distinct ways from Agent Mode’s workbook-based processes.


How to turn an Excel spreadsheet into an app


[Learn how](https://www.glideapps.com/blog/excel-spreadsheet-to-app)


### When is Glide a helpful AI tool to use with Excel?


**1. Safe AI for shared and sensitive files**


Unlike Agent Mode, which makes direct changes to your workbook, Glide creates AI-powered applications on top of your Excel data without modifying the underlying spreadsheet unless you explicitly want it to.


How this helps:


- **Work on data outside of the original spreadsheet** : Users interact with AI through the Glide app, preserving the original Excel file and its data.
- **Controlled access** : You can give team members view-only access through the app while restricting who can modify the actual Excel file.
- **Audit trails** : Track who made what changes and when.
- **Selective permissions** : Different users can see different data. Customers see only their records. Managers see their department's data.


Example: A finance team needs to track expenses in Excel, but they don't want field employees making direct changes to the master spreadsheet. With Glide:


- Employees submit expenses through a mobile app
- AI extracts data from receipt photos
- Changes are validated before being added to Excel
- Finance team reviews and approves before Excel updates
- No risk of accidental changes to formulas or formatting


**2. AI that unites multiple Data Sources**


Agent Mode only sees the currently open workbook. Glide AI can work with Excel data alongside information from dozens of other platforms:


**Data sources Glide connects:**


- Google Sheets and Excel
- [QuickBooks (accounting data)](https://www.glideapps.com/blog/no-code-quickbooks-integration)
- [Salesforce (CRM data)](https://www.glideapps.com/blog/salesforce-and-no-code)
- Airtable (databases)
- [SQL databases](https://www.glideapps.com/blog/intro-to-sql)
- [REST APIs](https://www.glideapps.com/blog/introduction-to-apis)
- Email (incoming emails can trigger AI workflows)
- Webhooks from external systems


Example use case: A sales manager wants AI insights combining Excel sales data, QuickBooks revenue, and Salesforce pipeline information. Agent Mode can't do this as it's limited to the Excel file. Glide AI can:


- Pull data from all three sources
- Use AI to analyze trends across systems
- Generate reports combining information from Excel, QuickBooks, and Salesforce
- Update Excel based on changes in other systems


**3. AI-powered workflows beyond a spreadsheet**


While Agent Mode works inside Excel,[Glide Workflows](https://www.glideapps.com/docs/automation/workflows) let you build sophisticated AI automations that trigger without anyone opening Excel:


**Schedule-triggered AI:**


- Every Monday at 9am, AI analyzes Excel sales data and emails a summary to leadership
- Daily inventory checks that use AI to flag low-stock items and auto-order from vendors
- Monthly cleanup where AI categorizes and archives old Excel entries


**Email-triggered AI:**


- Forward invoices via email → AI extracts vendor, amount, due date → adds to Excel
- Email receipt photos → AI reads amounts and categories → logs expenses in Excel
- Customer inquiries via email → AI parses details → creates Excel records


**Webhook-triggered AI:**


- When Shopify processes an order → AI structures the data → adds to Excel order tracker
- When Stripe receives payment → AI matches to invoice → updates Excel accounting
- When contract is signed in DocuSign → AI extracts terms → logs in Excel


Example: An architecture firm receives vendor invoices via email. Instead of manually entering details into Excel:


1. They forward invoices to a dedicated Glide email address
2. AI extracts invoice number, vendor, amount, due date from the PDF
3. AI categorizes the expense (materials, labor, equipment)
4. Conditional logic routes high-value invoices to CFO for approval
5. Approved invoices automatically add to Excel accounts payable
6. Slack notification alerts accounting team


This entire process runs automatically. No one needs to open Excel or run Agent Mode.


**4. Mobile-first AI interfaces**


Agent Mode works directly within Excel, which isn't ideal for mobile field work. Glide builds mobile-friendly apps from Excel data with AI capabilities that are more optimized for smartphones and tablets:


**Mobile AI features Glide can enable with your Excel sheets:**


- **AI receipt scanning** : Photograph receipts, AI extracts amounts and vendors, logs in Excel
- [Barcode scanning](https://www.glideapps.com/blog/barcode-scanner) : Scan items, AI looks up details, updates Excel inventory
- **Voice-to-data** : Speak notes in the field, AI transcribes and structures into Excel
- **Photo analysis** : Take equipment photos, Glide’s[AI inspection tool](https://www.glideapps.com/agents/inspections?utm_source=blog) identifies issues and logs in Excel
- **Location tracking** : GPS stamps entries, AI categorizes by location


Example: Construction crews track daily progress in Excel, but they're on job sites without laptops. With Glide:


- Crew members use phones to photograph completed work
- AI identifies task completion from photos
- Voice notes about issues get transcribed by AI
- GPS automatically tags which job site
- All data flows back to Excel automatically
- Project managers see updates in real-time without manual entry


### When to select Glide AI vs. Excel Agent Mode


**Choose Glide if you need:**


- AI that works with Excel AND other business systems (QuickBooks, Salesforce, databases)
- Safe AI for shared files where direct changes create risks
- Approval workflows before Excel modifications happen
- Mobile-first interfaces for field teams
- Automated AI workflows triggered by schedules, emails, or external events
- Different permission levels (customers see only their data, employees see only their department)
- AI that processes incoming emails or external system events


**Choose Agent Mode if you need:**


- Quick creation of Excel workbooks from scratch
- Complex analysis of data already in a single Excel file
- AI assistance while actively working in Excel
- Desktop Excel workflow (though Glide works with desktop Excel data via cloud sync)


**Use Both:**


Many organizations use Agent Mode for Excel-based analysis and modeling, while using Glide for operational workflows involving Excel data:


- Agent Mode creates financial models and analysis workbooks
- Glide handles data collection from field teams and syncs to Excel
- Agent Mode analyzes the data Glide collected
- Glide creates executive dashboards from the analysis


### Getting started with Glide AI for Excel


1. **Connect your Excel file** to Glide (via OneDrive or upload as CSV)
2. **Build a mobile app** from your Excel data using Glide's no-code interface
3. **Add AI capabilities** :


- Receipt scanning for expense tracking
- Data extraction from emails
- Automated categorization and tagging
- Smart forms that validate data before adding to Excel


4. **Set up workflows** (schedule, email, or webhook triggers)
5. **Deploy to your team** via web link or mobile app


Glide offers a free tier to test these capabilities before committing financially.


## Get the most out of AI for your Excel sheets


Agent Mode is a powerful addition to Excel that makes complex spreadsheet work more accessible. It's excellent for creating workbooks, analyzing data, and building reports, all through natural language conversation.


For scenarios where Agent Mode doesn’t work, tools like Glide complement it by providing AI capabilities that work safely with shared Excel files, connect to multiple data sources, and enable mobile-first workflows with sophisticated automation.


The key is matching the AI tool to your specific needs. When you need to perform complex Excel analysis in a single file, use Agent Mode. If you’re tackling operational workflows with Excel data involving multiple systems, mobile teams, or automated processes, use Glide. For businesses leaning into AI, both tools work well together.


As AI continues to evolve in Excel and related tools, understanding which tool fits which scenario will help you get the most value from AI while avoiding running into limitations. Knowing how to use both Agent Mode and Glide will boost your Excel proficiency at work. If you want to get started,[Glide University](https://learn.glideapps.com/) has all the resources you need to build your first Excel application.


Create a Glide app in five minutes, for free.


[Sign Up](https://os.glideapps.dev/?utm_source=blog&utm_campaign=Intro+to+the+new+Agent+Mode+in+Excel)


[Wren Noble](https://www.glideapps.com/blog/author/wren-noble)


Leading Glide’s content, including[The Column](https://www.glideapps.com/blog) and[Video Content](https://www.youtube.com/glideapps) , Wren’s expertise lies in no code technology, business tools, and software marketing. She is a writer, artist, and documentary photographer based in NYC.


### Related Articles


[AI engineering is the next wave after no-code: Here is how business leaders can get started](https://www.glideapps.com/blog/no-code-to-ai)[Gideon Lahav](https://www.glideapps.com/blog/author/gideon-lahav) 28 Jul 2026


[How to build custom software for e-commerce operations with AI](https://www.glideapps.com/blog/build-ecommerce-software)[Wren Noble](https://www.glideapps.com/blog/author/wren-noble) 27 Jul 2026


[What is AI citizen development? A guide for 2026](https://www.glideapps.com/blog/ai-citizen-development)[Wren Noble](https://www.glideapps.com/blog/author/wren-noble) 24 Jul 2026
