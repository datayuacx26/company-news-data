---
schema_version: "1.0.0"
document_id: "904c5f218c05da6226d58a91561c7edacb02e6c5b319629697abc8560a605e90"
company_key: "yc-memberstack"
company: "Memberstack"
source_id: "yc-memberstack-news-import-456c233bea3a"
canonical_url: "https://www.memberstack.com/blog/how-to-debug-while-vibe-coding-a-step-by-step-guide-for-non-developers"
published_at: null
first_seen_at: "2026-07-22T04:01:55.650331+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:e6ecaa849d34017ee60f0a6ce2dc2c1f1f92becd3266dda4c636463cf9f6eb98"
---

# How to Debug While Vibe Coding: A Step-by-Step Guide for Non-Developers

**TL;DR**


- Debugging AI-coded projects requires a systematic five-step workflow: encounter the bug, save a backup, collect detailed context (console errors, logs, screenshots), send comprehensive information to your AI assistant, and iterate until resolved. The key is gathering thorough context rather than prescribing solutions, as complex bugs often require multiple rounds to fix. Always maintain backups so you can revert if a fix makes things worse.


## **The Debugging Workflow**


If you have tried vibe coding (building projects with AI assistance like Claude, Cursor, or Windsurf), you know the feeling: everything seems to be working, then suddenly you hit a bug. Maybe it is a console error. Maybe something just will not load. If you are not a developer by trade, that moment can feel intense and overwhelming.


Here is the secret: even experienced developers encounter bugs on a regular basis. The difference is not in avoiding bugs. It is about having a reliable system for fixing them. In this guide, we will walk through a proven debugging workflow that has helped successfully launch numerous vibe-coded projects. You will learn exactly what to do when things go wrong, how to gather the right information, and how to work with AI to solve problems efficiently.


*This tutorial is based on Julian Galluzzo's debugging workflow. Watch the original video[here](https://www.youtube.com/watch?v=R55rZjcF1SQ) on the Memberstack YouTube Channel!*


## **Understanding the Debugging Workflow**


Before we dive into practical examples, let us understand the debugging workflow. This is a simple flowchart you can follow every time you encounter a bug:


### **Step 1: You Encounter a Bug**


- First, accept that bugs happen. They happen to everyone, in every project, all the time. Whether you are a seasoned developer or someone just starting with vibe coding, bugs are a normal part of the process. The key is not to panic. You have a systematic way to handle them, or will if you keep reading!


### **Step 2: Save a Backup**


**This is the most critical habit for confident vibe coding: save backups frequently.**


Here is when to save backups:


- Whenever your project is working, even if it is not finished
- When you encounter a small bug (before trying to fix it)
- After successfully fixing any bug
- Before making significant changes


Why backup before fixing even a small bug? Because debugging can sometimes make things worse. If you have a backup of the ‘less broken’ state, you can always revert and try a different approach.


### **Step 3: Collect Context and Send to AI**


This is where many people go wrong. They tell the AI "it is not working" without providing enough information. Here is what you need to gather:


**Types of Context to Collect:**


- **Console errors** - Open your browser's developer console (right-click, then Inspect, then Console tab) and copy any error messages
- **Server logs** - If you are running a backend, check for errors in your terminal
- **Screenshots** - Visual evidence of what is wrong (if something displays incorrectly)
- **Code snippets** - The specific section where you suspect the problem is
- **What you expected vs. what happened** - A clear description of the issue


**The Golden Rule:** Do not tell AI how to fix the problem unless you are 100% sure. Instead, give AI all the information and let it diagnose the issue. Your job is to be a detective who gathers clues, not the doctor who prescribes the treatment.


Here is an example of a good prompt to the AI:


### **Step 4: The Debugging Loop**


About 50% of the time, the AI will fix the bug on the first try. Great! Save a backup and continue building.


But the other 50% of the time, you will enter what we call the "debugging loop":


1. Send context to AI
2. AI suggests a fix
3. Bug persists or gets worse
4. Collect MORE context
5. Send to AI again
6. Repeat until bug is fixed


**Important mindset shift:** This loop can take 30 minutes, an hour, or even a full day. That is okay. Accept it. Take a deep breath. The fact that you (someone without formal coding education) can build functional applications with AI is absolutely phenomenal. This is the tradeoff, and it is worth it.


**When to revert:** If the bug gets significantly worse (way more errors than before), revert to your backup and start fresh with a different approach.


## **Real-World Debugging Examples**


Let us walk through actual debugging sessions from a real project. You will see exactly how to apply this workflow in practice.


### **Example 1: The Raw HTML Bug**


**The Problem:** After setting up a new Next.js project with Shadcn UI components, the page displays raw HTML instead of styled components.


**Step 1: Encounter the Bug** The application loaded, but instead of seeing beautifully styled components, there is just plain, unstyled HTML text on the screen.


**Step 2: Save a Backup** Since this is the very beginning of the project, the starting point itself is the backup. No need to save separately yet.


**Step 3: Collect Context** First, check the console for errors. Right-click on the page, select "Inspect," and open the Console tab. In this case: no errors. This is actually important information. The absence of console errors is context too.


**Step 4: Send to AI** Without console errors to guide us, we describe what we are seeing:


Notice the approach: we are not guessing at the solution. We are simply stating the symptom and asking AI to investigate.


**The Result:** The AI (Claude Code in this case) ran in plan mode, analyzed the setup, and identified the issue: missing Tailwind v4 configuration file. It created the necessary configuration, and the problem was solved.


**Step 5: Save a Backup** Now that the homepage is working and authentication pages look good, push to main on GitHub (or save a backup in your tool of choice).


### **Example 2: The Local Storage Error**


**The Problem:** After the AI finished implementing a feature, the application shows an error: "localStorage is not defined."


**Step 1: Encounter the Bug** The error appears in the console: "1 out of 1 error: Local storage is not defined."


**Step 2: Save a Backup** We just saved a backup after the previous fix, so we are good.


**Step 3: Collect Context and Send to AI** Copy the error message and paste it into your AI tool. Keep it simple. The error itself is the context.


**Step 4: The Debugging Loop Begins** AI attempts a fix, but a new error appears. This is the debugging loop in action. Do not get frustrated. Collect the new error and send it back.


After the second attempt, the AI fixes the issue. No more console errors, and everything works.


**Step 5: Save a Backup and Continue** Push to main (or save your backup) and move forward.


**Pro Tip:** Keep your AI chat sessions focused. When one task is complete and you are moving to something new, exit and then start a fresh chat. This prevents the AI from getting confused by too much context from previous, unrelated tasks.


### **Example 3: The Authentication Bug (The Deep Dive)**


This is where debugging gets interesting, and where you will really learn the workflow.


**The Problem:** Authentication is not working. When trying to sign up, users see "Signups have been temporarily disabled."


**Step 1: Encounter the Bug** Tried to test the authentication flow. The signup form appears to submit, but returns an error message instead of creating an account.


**Step 2: Save a Backup** Just saved one after the previous fix.


**Step 3: Collect Context** Check the console. There are several errors. Copy them all.


**Step 4: Send to AI**


**The Debugging Loop, Round 1:** AI responds that it found the root cause: Memberstack signups are disabled in the dashboard.


But wait. That does not make sense. The developer knows signups were not disabled manually, and Memberstack does not even have an option to do that.


This is a crucial moment: **you need to read what AI is telling you and verify if it makes sense.** Do not blindly accept solutions.


**Response to AI:**


**The Debugging Loop, Round 2:** AI adds additional console logging throughout the authentication flow. This creates more detailed error messages.


Now there is a new small error in the implementation of the logging itself. Fix that first before proceeding (debugging within debugging: it happens!).


**The Debugging Loop, Round 3:** Try the signup again with the new logging in place. Now the console shows a wealth of new information. Multiple errors that were not visible before.


Copy everything and send it to AI. Use plan mode to let AI analyze all the new context.


**The Breakthrough:** While waiting for AI to respond, something catches the developer's eye: the public key in one of the error messages does not have "SB" in it. That means it is trying to use a **live mode** public key instead of a **test mode** key.


This is exactly what debugging is about: looking for weird variables and things that do not match expectations.


**Send Additional Context:**


**The Result:** AI confirms, problem identified! The wrong public key was being used. After AI updates it with the correct test environment key, signup works successfully. A new user appears in the Memberstack dashboard.


**The Debugging Loop, Round 4:** After successful signup, there are two new minor errors. Report them to AI: "It is working now, but we have two more errors: \[paste errors\]"


AI fixes them. Then one final small error appears. Paste it, send it, and AI resolves it.


**Final Result:** The application works perfectly. The user can sign up, log in, and the dashboard loads with no console errors.


**Step 5: Save a Backup** Push to main. The project has made significant progress, all thanks to systematic debugging.


## **Key Lessons from the Debugging Examples**


### **1. The Debugging Loop is Normal**


Notice how the authentication bug required multiple rounds of back-and-forth? That's not unusual. Complex bugs often require:


- Multiple attempts at fixes
- Adding logging/debugging tools
- Gathering more context
- Trying different approaches


Don't get discouraged when the first attempt doesn't work. This is the process.


### **2. Read and Verify AI Responses**


When AI suggested that Memberstack signups were disabled (which is not possible), catching that error prevented wasting time going down the wrong path. Always:


- Read what the AI is proposing
- Verify it makes sense in your context
- Push back if something seems incorrect
- Ask for different approaches if needed


### **3. Add More Context When Stuck**


When the initial errors didn't reveal the problem, adding console logging provided the breakthrough information needed. If you're stuck:


- Ask AI to add more logging
- Check different parts of your application
- Look for patterns in the errors
- Try to reproduce the bug in different ways


### **4. Look for the Unexpected**


The breakthrough came from noticing the public key looked wrong. When debugging:


- Pay attention to details that seem "off"
- Compare what you see vs. what you expect
- Look for inconsistencies
- Trust your instincts when something doesn't look right


## **When to Take a Break**


If you've been in the debugging loop for an hour and making no progress:


1. Save your current state
2. Take a break (seriously—walk away)
3. Come back with fresh eyes
4. Consider reverting to your last working backup and trying a completely different approach
5. Ask AI to explain the system to you rather than just fix it


Sometimes understanding *why* something isn't working helps you provide better context.


## **Building Confidence Through Practice**


The more you debug, the better you'll get at:


- Recognizing common error patterns
- Knowing what context to collect
- Understanding how to communicate with AI
- Staying calm when things go wrong


Remember: every successful vibe-coded project has gone through this exact process. The bugs you're encountering aren't signs of failure, they are normal parts of building software.


## **Your Debugging Checklist**


Save this checklist for the next time you encounter a bug:


1. **Don't panic** - Bugs are normal and expected
2. **Save a backup** - If your project is currently working (even partially)
3. **Open developer console** - Check for errors (right-click → Inspect → Console)
4. **Collect all context:**


- Console errors
- Server logs
- Screenshots
- Description of what's wrong


5. **Send to AI** - Give all context, don't prescribe solutions
6. **If not fixed:**


- Collect MORE context
- Add console logging if needed
- Look for unexpected details
- Send again


7. **If bug gets much worse** - Revert to backup and try different approach
8. **When fixed** - SAVE A BACKUP before continuing
9. **Move forward** - Continue building your project


## **Final Thoughts**


Vibe coding is new and unprecedented. It allows people without traditional development backgrounds to create functional, professional applications. But it is not magic. It requires patience, systematic thinking, and the willingness to methodically work through problems, sometimes at a snail’s pace.


The debugging workflow outlined in this guide isn't just about fixing bugs. It's about building confidence. When you know you have a reliable process for handling problems, you can tackle more ambitious projects. You can launch with confidence, knowing that when issues arise (and they will), you know exactly what to do.


The next time you encounter a bug, remember: you're not stuck. You're just at the start of the debugging loop. Follow the process, gather context, work with your AI assistant, and you'll get through it.


Happy vibe coding!


## **FAQs**


**Q: How long should I expect to spend debugging a typical bug?**


A: Simple bugs may resolve in 5-10 minutes, while complex issues can take 30 minutes to several hours. If you have been debugging for over an hour without progress, take a break or revert to your last backup and try a different approach.


**Q: What if the AI's suggested fix makes the bug worse?**


A: This is common and exactly why saving backups is critical. If you notice significantly more errors after a fix, revert to your last working backup immediately and try a different approach with fresh context.


**Q: I do not see any console errors, but something is clearly wrong. What should I do?**


A: The absence of console errors is itself useful information. Describe what you are seeing versus what you expected, include screenshots if relevant, and explain the specific incorrect behavior to your AI assistant.


**Q: Should I try to fix bugs myself or always use AI?**


A: Your role is to be the detective who gathers comprehensive context, not the doctor who prescribes treatment. Collect all relevant information and let the AI diagnose and suggest solutions unless you are completely certain of the cause.


**Q: What is the most important debugging habit to develop?**


A: Save backups frequently—whenever your project is working (even partially), before attempting any fix, and immediately after resolving an issue. This single habit gives you confidence to experiment without fear of breaking everything.


**Q: When should I start a fresh AI chat session versus continuing in the same conversation?**


A: Start a fresh chat when moving to a completely different task or feature to prevent confusion from unrelated context. Stay in the same session while debugging a single issue so the conversation history helps the AI understand your problem.
