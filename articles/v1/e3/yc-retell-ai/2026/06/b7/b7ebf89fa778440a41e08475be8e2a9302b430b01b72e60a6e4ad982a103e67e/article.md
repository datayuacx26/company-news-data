---
schema_version: "1.0.0"
document_id: "b7ebf89fa778440a41e08475be8e2a9302b430b01b72e60a6e4ad982a103e67e"
company_key: "yc-retell-ai"
company: "Retell AI"
source_id: "yc-retell-ai-news-import-48ab15cc20a2"
canonical_url: "https://www.retellai.com/blog/prevent-toll-fraud-on-your-ai-voice-agents-with-retell-ai"
published_at: "2025-09-10T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:02.485527+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:ade14fbc86a27b33ea76156f878e48d1433b36bf211143484fb33e30079ea19a"
---

# Toll Fraud in AI Voice Agents: How to Prevent It with Retell AI | Retell AI

Toll fraud is one of the biggest risks for modern[AI voice agents](https://www.retellai.com/blog/how-to-use-ai-phone-agents-for-multilingual-communication) , and it can drain your budget overnight.


Imagine launching your new[voice AI system](https://www.retellai.com/) , only to see your monthly bill jump from $500 to $15,000 in a matter of days. The culprit? Automated attackers exploiting your unprotected call flows to generate unauthorized calls, often to expensive international or premium-rate numbers.


These attacks don’t just cost money; they tie up your agents, block real customers, and damage trust in your service. The good news is that **you can stop toll fraud before it starts** .


Retell AI has introduced a powerful security upgrade that combines[Public Keys with Google reCAPTCHA](https://docs.retellai.com/accounts/public-keys#public-keys) . This dual-layer protection ensures only legitimate users can access your AI voice agents,[keeping costs predictable](https://www.retellai.com/pricing) , customers happy, and your infrastructure safe.


In this guide, we’ll explain what toll fraud is, why it matters, and show you step-by-step how to implement Retell AI’s security features.


## The Security Risks of Unprotected AI Voice Agents


Consider these common attack scenarios:


**Bot Farms:** Automated scripts bombard your[voice agent](https://www.retellai.com/blog/connect-any-ai-voice-agent-to-mcp-with-retell-ai-mcp-node) with thousands of simultaneous calls, consuming your minutes and overwhelming your system


**Premium Number Routing:** Fraudsters use your AI agent to dial expensive international or premium-rate numbers, with you footing the bill


**Resource Exhaustion:** Spam attacks prevent legitimate customers from reaching your AI agent, damaging your brand reputation


**Data Harvesting:** Attackers probe your voice agent for vulnerabilities or sensitive information


For businesses running[AI voice agents at scale](https://www.retellai.com/features/batch-call) , these attacks can cost tens of thousands of dollars per incident, not to mention the operational disruption and customer frustration.


## Retell AI's Security Upgrade: Public Keys + Google reCAPTCHA


Retell AI has developed **a powerful security feature** that combines **public key authentication with Google reCAPTCHA protection** .


This dual-layer approach ensures that only legitimate users can access your AI voice agents while maintaining a seamless customer experience.


### How Public Keys and reCAPTCHA Can Prevent Toll Fraud


**Public Keys** act as your first line of defense, creating a secure authentication layer between your website and Retell AI's infrastructure. Think of it as a digital handshake that verifies legitimate requests before any call is initiated.


**Google reCAPTCHA** adds intelligent human verification, distinguishing between real users and automated attacks. The beauty of modern reCAPTCHA is its invisibility, legitimate users won't even notice it's there, while bots are stopped in their tracks.


Together, these technologies create a barrier against toll fraud while preserving the instant, frictionless experience your customers expect.


## Step-by-Step Guide: Implementing Public Keys and reCAPTCHA in Retell AI


Let's walk through the complete setup process to secure your AI voice agents:


### Step 1: Access Retell Documentation and Create reCAPTCHA Keys


1. Go to the["Google reCAPTCHA" guide](https://docs.retellai.com/accounts/public-keys#public-keys) on the Retell AI documentation


2. Access Google's API and Services console


3. Create a new project (or select an existing one)


### Step 2: Configure Your Domain Settings


1. Set up a descriptive label for your reCAPTCHA implementation


2. Add all domains where your chat widget will be hosted. Remember to include both production and staging domains


3. Submit the configuration


**Important:** Copy both the Site Key and Secret Key, you'll need these shortly


### Step 3: Configure Public Keys in Retell AI


1. Log into your Retell AI dashboard


2. Navigate to Settings → Public Keys


3. Click "Add New Public Key"


4. Ensure the domains match exactly what you entered in reCAPTCHA


5. Enable abuse protection settings


6. Enter your Secret Key from Google reCAPTCHA


7. Save the configuration


8. Copy the generated Site Key for the next step


### Step 4: Implement the Secure Chat Widget


1. Return to Retell documentation →[Chat Widget section](https://docs.retellai.com/deploy/chat-widget)


2. Copy the example widget code


3. Access your website or web service provider (e.g., WordPress, Go High Level, custom CMS)


4. Create or edit the page where you want the widget


5. Open the code editor or HTML block


### Step 5: Customize and Deploy the Widget Code


1. Paste the copied widget code


2. Replace placeholder values:


3. Insert your Site Key (from reCAPTCHA)


4. Insert your Public Key (from Retell AI)


5. Add your Agent ID (found in your Retell AI agent settings)


6. Save all changes


7. Publish or deploy your updates


### Step 6: Test and Verify


1. Visit your website in an incognito browser window


2. Interact with the chat widget


3. Verify that legitimate users can access the voice agent


4. Test from different devices and networks


5. Monitor your Retell AI dashboard for successful authentications


## Troubleshooting Common Issues


### **Widget Not Appearing:**


• Verify all keys are correctly copied and pasted


• Check browser console for JavaScript errors


• Ensure domains match exactly between reCAPTCHA and Retell AI


### **Legitimate Users Blocked:**


• Review reCAPTCHA sensitivity settings


• Check if users are behind VPNs or corporate firewalls


• Consider implementing a fallback contact method


### **Configuration Errors:**


• Double-check domain formatting (no trailing slashes or protocols)


• Ensure secret keys are kept confidential and not exposed in client-side code


• Verify agent ID is correct and agent is active


## The ROI of Security: Why This Matters for Your Business


Implementing Public Keys and reCAPTCHA isn't just about preventing attacks, it's about enabling sustainable growth. Consider these real-world impacts:


### 1. Cost Savings


Stop toll fraud before it starts. Businesses report saving thousands of dollars monthly by preventing unauthorized calls to premium numbers and eliminating bot-driven usage spikes.


### 2. Enhanced Customer Trust


When customers know their interactions are secure, they're more likely to engage with your AI voice agents. This security layer demonstrates your commitment to protecting both their data and your service quality.


### 3. Improved System Reliability


By filtering out malicious traffic, your AI voice agents remain available for legitimate customers. No more system overloads or degraded performance during attack attempts.


### 4. Scalability Without Risk


Deploy AI voice agents across multiple domains and touchpoints without multiplying your security risks. Each implementation remains protected by the same robust authentication system.


### 5. Compliance and Peace of Mind


Meet security requirements for industries with strict compliance standards. Public key authentication provides an audit trail and demonstrates due diligence in protecting your voice infrastructure.


## Get Started Today


Don't wait for an attack to realize the importance of securing your AI voice agents. With Retell AI's Public Keys and reCAPTCHA integration, you can implement bank-grade security in under 30 minutes.


### Next Steps:


[Sign up for Retell AI](https://dashboard.retellai.com/) if you haven't already


**Follow this guide** to implement security for your existing agents


**Monitor your protected agents** and enjoy peace of mind


Remember: In the world of AI voice agents, security isn't optional, it's essential. Protect your investment, your customers, and your reputation with proper authentication from day one.


‍
