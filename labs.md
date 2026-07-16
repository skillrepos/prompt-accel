# Prompt Engineering Accelerator — Hands-On Labs
## Mastering the Techniques, Patterns, and Strategies Behind High-Performance AI Prompting
 
## Session labs 
## Revision 6.3 - 07/16/26
 
## How to Use These Labs
 
Labs 1-4 can be done using any free AI chat interface (ChatGPT, Claude, Gemini, Copilot, or a local model via Ollama) including the one in the codespace. You'll paste prompts, observe results, iterate, and compare. No coding required. **Setup**: Open your preferred AI chat interface in a browser tab. Start a new conversation for each lab.
 
(If you want to use the model running in the codespace, run this command before prompting:
 
```
ollama run granite4:3b
```
**NOTE: Doing labs 1-4 in the codespace will likely be noticeably slower than running in another chat interface.**)
 
Labs 5-6 are designed to be done in the codespace environment. **Setup:** Follow the startup instructions in the README.md file IF NOT ALREADY DONE!
 
 
 
**NOTES**
- To copy and paste in the codespace, you may need to use keyboard commands - CTRL-C and CTRL-V. Chrome may work best for this.
---
 
<br>

## Lab 1: Creating Strong Prompts 
 
**Goal**: Apply the 6 building blocks (Task, Context, Role, Format, Examples, Constraints) to transform vague prompts into structured, measurable ones.
 
**What you'll learn**: How adding each element systematically improves output quality, consistency, and usefulness.
 
### Steps
 
<br>
**Step 1 — Establish a baseline.** Paste this vague prompt into your AI chat and note the response:
 
```
Explain cloud computing.
```
 
Observe: How long is the response? Is it useful for any specific audience? Could you predict what format you'd get?
 
<br><br>
 
**Step 2 — Add a Task definition.** Now try:
 
```
Task: Explain the three main service models of cloud computing (IaaS, PaaS, SaaS).
```
 
Compare: Is the output more focused? Does it cover what you expected?
 
<br><br>
 
**Step 3 — Add Context and Audience.** Build on the prompt:
 
```
Task: Explain the three main service models of cloud computing (IaaS, PaaS, SaaS).
Context: The audience is a group of small business owners evaluating whether to migrate from on-premises servers. They have no technical background.
```
 
Compare: Has the language changed? Are the explanations more relevant to the audience?
 
<br><br>
 
**Step 4 — Add a Role.** Add a persona:
 
```
Role: You are a cloud computing consultant who specializes in helping small businesses migrate to the cloud. You explain technical concepts using everyday business analogies.
 
Task: Explain the three main service models of cloud computing (IaaS, PaaS, SaaS).
Context: The audience is a group of small business owners evaluating whether to migrate from on-premises servers. They have no technical background.
```
 
Compare: Do you notice analogies or a different communication style?
 
<br><br>
 
**Step 5 — Add Format requirements.** Specify the output structure:
 
```
Role: You are a cloud computing consultant who specializes in helping small businesses migrate to the cloud. You explain technical concepts using everyday business analogies.
 
Task: Explain the three main service models of cloud computing (IaaS, PaaS, SaaS).
Context: The audience is a group of small business owners evaluating whether to migrate from on-premises servers. They have no technical background.
 
Format: For each service model, provide:
- Name and one-sentence definition
- A real-world business analogy
- One example use case for a small business
- Estimated monthly cost range
 
Present as a comparison table.
```
 
Compare: Is the output now consistently structured and easy to scan?
 
<br><br>
 
**Step 6 — Add Constraints.** Add boundaries:
 
```
Role: You are a cloud computing consultant who specializes in helping small businesses migrate to the cloud. You explain technical concepts using everyday business analogies.
 
Task: Explain the three main service models of cloud computing (IaaS, PaaS, SaaS).
Context: The audience is a group of small business owners evaluating whether to migrate from on-premises servers. They have no technical background.
 
Format: For each service model, provide:
- Name and one-sentence definition
- A real-world business analogy
- One example use case for a small business
- Estimated monthly cost range
 
Present as a comparison table.
 
Constraints:
- No jargon without immediate explanation
- Keep total response under 300 words
- End with a one-sentence recommendation for a typical 10-person office
```
 
<br><br>
 
**Step 7 — Run the final prompt 3 times.** Paste the complete prompt from Step 6 three separate times (in new conversations or after clearing context). Compare the three outputs:
- Do they all follow the table format?
- Are the analogies consistent in quality?
- Does the recommendation stay relevant?
This is measuring **consistency** — a key quality metric.
 
<br><br>
 
**Step 8 — Reflect.** Rate each version (Steps 1-6) on a 1-5 scale for: specificity, usefulness, and consistency. Note which building block made the biggest difference for this task.
 
<br><br>
 
<p align="center">— — —&nbsp;&nbsp;&nbsp;END OF LAB 1&nbsp;&nbsp;&nbsp;— — —</p>
<br><br>
 
---
 
<br>

## Lab 2: Few-Shot & Chain of Thought Comparison 
 
**Goal**: Compare zero-shot, few-shot, and Chain of Thought (CoT) prompting on a classification task where the "correct" answer depends on company-specific rules that the AI can't infer on its own.
 
**What you'll learn**: When accuracy problems are caused by missing domain knowledge (not sloppy reasoning), few-shot examples fix the gap while CoT only makes the wrong reasoning visible — a useful distinction for choosing the right technique.
 
**Scenario**: You work at a growth-stage SaaS company that just raised Series B. The company has unusual but defensible priority rules driven by its current business strategy — and several of these rules *contradict* standard industry practice.
 
### Steps
 
<br>
**Step 1 — Predict, then test.** Read this ticket carefully:
 
> "Hey, just wanted to let you know that Competitor X released a feature similar to your dashboard. I'm still happy with your product, but thought you'd want to know!"
 
**Before pasting anything**, write down: what priority would you assign (P1-Critical, P2-High, P3-Medium, P4-Low)? And what priority do you think the AI will assign?
 
Now test your prediction:
 
```
You are a support ticket classifier. Assign exactly one priority level to the following support ticket: P1-Critical, P2-High, P3-Medium, or P4-Low.
 
Ticket: "Hey, just wanted to let you know that Competitor X released a feature similar to your dashboard. I'm still happy with your product, but thought you'd want to know!"
```
 
Were you right about the AI's answer? This seems low-priority — the customer is happy and just sharing info. But at this company, **any mention of a competitor is an immediate P1 churn signal**, because reducing churn is the board's #1 metric post-Series B.
 
<br><br>
 
**Step 2 — Batch classify.** Now let's have the AI assess 5 tickets at once to build a zero-shot baseline:
 
```
You are a support ticket classifier. Assign exactly one priority level to each ticket: P1-Critical, P2-High, P3-Medium, or P4-Low.
 
Classify each ticket independently. Present results as a table with columns: Ticket #, Short Description, Priority, Reasoning (1 sentence).
 
Ticket 1: "Hey, just wanted to let you know that Competitor X released a feature similar to your dashboard. I'm still happy with your product, but thought you'd want to know!"
 
Ticket 2: "URGENT: I accidentally deleted my project files and I need them back IMMEDIATELY. This is a disaster!"
 
Ticket 3: "The 'Get Started' button on your pricing page has a weird color glitch on mobile. Just FYI."
 
Ticket 4: "None of our 25-person team can access the platform since 8 AM. We have client meetings today."
 
Ticket 5: "I was charged $45 twice this month. Can someone look into this?"
```
 
Look at the results. The AI will classify based on **general industry norms**. But this company's actual rules are very different.
 
<br><br>
 
**Step 3 — Spot the mismatches.** Here is this company's actual priority policy — shaped by their growth strategy:
 
- **Any mention of a competitor** = always P1, even if the customer sounds happy (churn prevention is the #1 board metric)
- **Data loss for single users** = P3, not P1 (automated backups recover files in 15 minutes — this is routine)
- **UI/cosmetic bugs on public-facing pages** (pricing, signup, landing) = P2 (these pages drive conversion — the growth team's top priority)
- **Service outages affecting teams** = P1 (standard)
- **Billing issues under $100** = P4 (self-service billing portal handles these automatically)
Compare the AI's zero-shot answers to this policy. How many of the 5 tickets did zero-shot get wrong?
 
<br><br>
 
**Step 4 — Try CoT without the company rules.** Start a new conversation. This time, give the AI a structured reasoning framework — but do NOT include the company-specific policy. We want to see if better *thinking* alone fixes the errors:
 
```
You are a support ticket classifier. Assign exactly one priority level to each ticket: P1-Critical, P2-High, P3-Medium, or P4-Low.
 
For each ticket, reason through these steps BEFORE assigning priority:
1. What is the actual technical or business issue? (Ignore emotional language.)
2. What is the blast radius? (One user, a team, all users, or a potential customer?)
3. Is there data loss, a security risk, or a revenue impact?
4. Does the customer have a workaround?
5. Based on business impact (not tone), assign priority.
 
Show your full reasoning for each ticket, then present a summary table.
 
Ticket 1: "Hey, just wanted to let you know that Competitor X released a feature similar to your dashboard. I'm still happy with your product, but thought you'd want to know!"
 
Ticket 2: "URGENT: I accidentally deleted my project files and I need them back IMMEDIATELY. This is a disaster!"
 
Ticket 3: "The 'Get Started' button on your pricing page has a weird color glitch on mobile. Just FYI."
 
Ticket 4: "None of our 25-person team can access the platform since 8 AM. We have client meetings today."
 
Ticket 5: "I was charged $45 twice this month. Can someone look into this?"
```
 
Compare to zero-shot. You'll likely find CoT produces **the same classifications** — and that's the key insight. The reasoning is now *visible* (you can read exactly how the AI thought through each ticket), but the answers don't improve. Why? Because the errors aren't caused by sloppy thinking — they're caused by **missing domain knowledge**. The AI reasons perfectly well about blast radius and business impact, but it has no way to know that this company treats competitor mentions as P1 or that single-user data loss is only P3 because of automated backups. CoT makes the reasoning auditable, which is great for debugging — but structured thinking can't substitute for information the model doesn't have.
 
<br><br>
 
**Step 5 — Teach the policy with examples.** Now paste this few-shot prompt — it teaches the company's policy through examples, not rules:
 
```
You are a support ticket classifier for a growth-stage SaaS company. Assign exactly one priority level: P1-Critical, P2-High, P3-Medium, or P4-Low.
 
Company priority rules (taught by example):
 
Ticket: "I noticed your competitor just launched a mobile app. I don't need one personally, but just a heads up."
Priority: P1-Critical
(Any mention of a competitor — even casual or positive — is P1. Churn prevention is our top metric.)
 
Ticket: "Help! I accidentally deleted my entire project folder. I need those files back!"
Priority: P3-Medium
(Single-user data loss is P3. Our automated backup system recovers files in 15 minutes.)
 
Ticket: "The testimonial carousel on the homepage is overlapping the signup button on tablets."
Priority: P2-High
(UI bugs on public-facing pages like homepage, pricing, or signup are P2 — they affect conversion rates.)
 
Ticket: "Our whole department of 40 people can't log in since this morning."
Priority: P1-Critical
(Service outages affecting teams are always P1.)
 
Ticket: "You double-charged me $29 on my last invoice. Please refund."
Priority: P4-Low
(Billing issues under $100 are P4 — our self-service billing portal handles these automatically.)
 
Ticket: "I just finished a demo of Competitor Y for my team. Still deciding between you two."
Priority: P1-Critical
(Active competitor evaluation = highest churn risk. Always P1 regardless of tone.)
 
Now classify these 5 tickets. Present as a table with: Ticket #, Short Description, Priority, Reasoning.
 
Ticket 1: "Hey, just wanted to let you know that Competitor X released a feature similar to your dashboard. I'm still happy with your product, but thought you'd want to know!"
 
Ticket 2: "URGENT: I accidentally deleted my project files and I need them back IMMEDIATELY. This is a disaster!"
 
Ticket 3: "The 'Get Started' button on your pricing page has a weird color glitch on mobile. Just FYI."
 
Ticket 4: "None of our 25-person team can access the platform since 8 AM. We have client meetings today."
 
Ticket 5: "I was charged $45 twice this month. Can someone look into this?"
```
 
Compare all three tables. Few-shot should nail all 5 — the examples directly taught the counterintuitive rules that neither zero-shot nor CoT could figure out. This is the payoff: examples don't just improve reasoning, they **transfer knowledge** the model couldn't access otherwise.
 
<br><br>
 
**Step 6 — Identify the pattern.** Look at your zero-shot, CoT, and few-shot results together. Notice that zero-shot and CoT likely produced the same classifications — the only difference is CoT shows its work. For each ticket, consider: what reasoning did CoT reveal, and why didn't that reasoning lead to the correct answer? What did the few-shot examples provide that structured thinking alone couldn't?
 
<br><br>
 
**Step 7 — Compare results.** (Virtually) fill in your scorecard:
 
| Ticket (short) | Zero-Shot | CoT (no rules) | Few-Shot | Company Policy |
|----------------|-----------|----------------|----------|----------------|
| Competitor mention (happy) | | | | P1 |
| Deleted files (urgent) | | | | P3 |
| Pricing page glitch (casual) | | | | P2 |
| 25-person outage | | | | P1 |
| $45 double charge | | | | P4 |
 
<br><br>
 
You should see: zero-shot (~2/5), CoT (~2/5 — same answers, but with visible reasoning), few-shot (5/5). The takeaway: CoT adds **auditability** (you can see *why* the AI chose each priority), but it doesn't add **accuracy** when the problem is missing domain knowledge. Only few-shot — which transfers the company's actual rules through examples — closes the gap.
 
<br><br>
 
**Step 8 (Optional) — Write your own tricky ticket.** Write a support ticket designed to **fool the zero-shot classifier** given this company's unusual rules. Hint: try a ticket where the obvious priority and the company policy priority are as far apart as possible.
 
<br><br>
 
**Step 9 (Optional) — Test your adversarial ticket.** Paste your tricky ticket and ask:
 
```
Classify this ticket using all three approaches (zero-shot, few-shot with the company examples, and CoT with the company-specific reasoning framework). Show each approach's classification side by side.
```
 
Which technique handled your adversarial ticket best?
 
<br><br>
 
Key takeaway: CoT improves *how* the AI reasons but can't teach it rules it doesn't know. Few-shot teaches domain knowledge directly through examples. In production, combine both: few-shot examples to teach the rules, plus CoT to make every decision auditable. The AI "knows" standard industry practices — but it can't know *your* company's unique priorities unless you teach it.
 
<br><br>
 
<p align="center">— — —&nbsp;&nbsp;&nbsp;END OF LAB 2&nbsp;&nbsp;&nbsp;— — —</p>
<br><br>
 
---
 
<br>

## Lab 3: Production Prompt Engineering 
 
**Goal**: Build a structured, constrained prompt that produces consistent, machine-parseable JSON output suitable for feeding into a real system.
 
**What you'll learn**: How to combine schema, role, and constraints to create production-grade prompts — and why each layer solves a different problem.
 
### Steps
 
<br>
**Step 1 — Generate, then break.** Paste this deliberately vague prompt:
 
```
Write changelog entries for these three product updates:
 
1. We added the ability for users to export their dashboard data as a CSV file.
2. We redesigned the notification preferences page so users can choose per-channel settings for email, SMS, and in-app alerts.
3. We fixed a bug where the search bar would return no results if the query contained special characters like & or #.
```
 
Look at what you got. It's probably readable — maybe bullet points, maybe a nicely formatted list. Now imagine you need to feed this into an automated system that sends targeted emails to affected users, updates a status page, and files Jira tickets. Could a script reliably extract the data it needs from this output?
 
<br><br>
 
**Step 2 — Try to parse it.** Imagine writing code to process this output automatically. For each entry, you'd need to extract: a title, a change type (feature/bugfix/improvement), a description, which product areas are affected, and whether users need to do anything. Try to answer these questions:
 
- Is there a consistent, predictable structure a script could rely on?
- Could you extract the change type programmatically, or is it buried in prose?
- Are affected product areas listed in a standard way, or described differently each time?
- Is "user action required" explicitly stated, or would a script have to guess from context?
- If you ran this prompt 10 times, would the format be identical every time?
The output is fine for a human reader — but it's not **machine-parseable**. That's the production gap. Think about the reasons a script could struggle with this output.
 
<br><br>
 
**Step 3 — Design a machine-readable format.** Based on the parsing problems already mentioned, think about a structured format (JSON, XML, or whatever you prefer) that a script could reliably consume. How would you define specific field names, data types, and allowed values? Spend 2 minutes designing your schema before looking at Step 4.
 
<br><br>
 
**Step 4 — Compare to a reference.** Start a new conversation and paste this fully constrained version:
 
```
Role: You are a technical writer producing entries for an automated notification system. Every entry must be identically formatted.
 
Task: Generate JSON changelog entries for these three product updates:
 
1. We added the ability for users to export their dashboard data as a CSV file.
2. We redesigned the notification preferences page so users can choose per-channel settings for email, SMS, and in-app alerts.
3. We fixed a bug where the search bar would return no results if the query contained special characters like & or #.
 
Output each entry as JSON with exactly this schema (no extra fields):
{
  "title": "string — max 10 words, sentence case, no trailing period",
  "type": "feature|improvement|bugfix|breaking_change",
  "description": "string — exactly 2-3 sentences. First: what changed. Second: why it matters. Third (optional): context.",
  "affected_areas": ["Use ONLY: dashboard, settings, search, auth, billing, notifications, api, reports"],
  "user_action_required": true|false,
  "details": "string — technical details or migration steps. Must be null if user_action_required is false."
}
 
Constraints:
- Output ONLY a valid JSON array, no markdown fences, no explanation before or after
- type must reflect: new capability = feature, enhancement = improvement, fix = bugfix, requires migration = breaking_change
```
 
Compare this output to your Step 1 results. Notice the difference: Step 1 produced human-readable prose; this produces machine-parseable JSON with controlled values. Run this constrained prompt 2-3 times in separate conversations — the output should be structurally identical every time. How does your schema from Step 3 compare to this reference?
 
<br><br>
 
**Step 5 — Stress-test with edge cases.** In the same conversation as Step 4, paste this:
 
```
Now generate changelog entries using the same constrained format for these two edge cases:
 
4. We migrated the authentication system from session-based cookies to JWT tokens. Existing sessions will be invalidated on March 15. Users will need to log in again, and any API integrations using session cookies must be updated to use bearer tokens. The new system supports refresh tokens with a 30-day expiry.
 
5. Fixed a typo in the footer — "Contant Us" now correctly reads "Contact Us".
```
 
These are the extremes: a complex breaking change with multiple impacts, and a trivial typo fix. Check:
- Did it correctly classify the JWT migration as "breaking_change" with user_action_required: true?
- Did it provide useful migration details (not just repeat the description)?
- Did it handle the typo gracefully — or did it inflate a one-line fix into a multi-sentence description to meet the "2-3 sentences" rule?
That last point is minor in this example, but could be more significant in others: constraints that work for normal entries can create awkward results at the extremes. Think about any rules that need adjusting.
 
<br><br>
 
**Step 6 — Make the AI self-validate.** Now ask the AI to check its own work:
 
```
Review all 5 changelog entries you generated. For each one, check against these rules and report any violations:
1. Valid JSON that parses without errors?
2. All required fields present with correct types?
3. Title under 10 words, sentence case, no period?
4. Description exactly 2-3 sentences?
5. affected_areas uses only standard names from the allowed list?
6. details is null when user_action_required is false?
```
 
Did the AI catch issues it introduced? This is a powerful production pattern: you can build validation directly into your prompts so the AI checks itself before returning results.
 
<br><br>
 
**Step 7 — Combine into one production prompt.** You've now built three layers: schema (Step 4), edge-case testing (Step 5), and self-validation (Step 6). In production, these all go into a single prompt. Start a new conversation and paste this combined version:
 
```
Role: You are a technical writer producing entries for an automated notification system.
 
Task: Generate JSON changelog entries for these product updates:
 
1. We added the ability for users to export their dashboard data as a CSV file.
2. We redesigned the notification preferences page so users can choose per-channel settings for email, SMS, and in-app alerts.
3. We fixed a bug where the search bar would return no results if the query contained special characters like & or #.
4. We migrated authentication from session-based cookies to JWT tokens. Existing sessions invalidated March 15. Users must log in again; API integrations using session cookies must switch to bearer tokens.
5. Fixed a typo in the footer — "Contant Us" now reads "Contact Us".
 
Schema (no extra fields):
{
  "title": "max 10 words, sentence case, no trailing period",
  "type": "feature|improvement|bugfix|breaking_change",
  "description": "2-3 sentences. First: what changed. Second: why it matters. Third (optional): context. For trivial fixes, 1 sentence is acceptable.",
  "affected_areas": ["ONLY: dashboard, settings, search, auth, billing, notifications, api, reports"],
  "user_action_required": true|false,
  "details": "migration steps or technical details. null if user_action_required is false."
}
 
Constraints:
- Output ONLY valid JSON array — no markdown, no commentary
- type: new capability = feature, enhancement = improvement, fix = bugfix, requires migration = breaking_change
 
After generating, self-validate: check every entry against the schema rules above. If any entry violates a rule, fix it and note what you corrected.
```
 
Notice the refinement: "For trivial fixes, 1 sentence is acceptable" — that's the edge-case fix from Step 5, built directly into the schema. And self-validation is baked into the prompt itself.
 
<br><br>
 
**Step 8 — Review the combined output.** Did the single production prompt produce clean results for all 5 entries — including the edge cases? Did self-validation catch anything? Compare this to the multi-step process from Steps 4-6. In production, one well-designed prompt replaces the iterative testing you did in this lab.
 
<br><br>
 
**Step 9 (Optional) — Apply to your own work.** Think of a structured output you need at work — a status report, an incident summary, a meeting recap, a product review. Design a production prompt for it with all three layers: schema, constraints, and self-validation. Test it with both typical and edge-case inputs.
 
<br><br>
 
**Step 10 — Reflect.** You built three layers of prompt quality: schema (structure), constraints (quality control), and self-validation (built-in error checking). The schema solves "can a script read this?" Constraints solve "will the data be reliable?" Self-validation solves "will it catch its own mistakes?" In production, all three go into one prompt — and you stress-test with edge cases before deploying.
 
<br><br>
 
<p align="center">— — —&nbsp;&nbsp;&nbsp;END OF LAB 3&nbsp;&nbsp;&nbsp;— — —</p>
<br><br>
 
---
 
<br>

## Lab 4: Multi-Expert & Reverse Prompting 
 
**Goal**: Use multi-expert prompting to get diverse perspectives on a complex decision, then use reverse prompting to discover hidden requirements.
 
**What you'll learn**: How simulating multiple expert viewpoints catches blind spots, and how letting the AI ask questions produces better specifications.
 
### Steps
 
<br>
**Step 1 — Generic baseline.** Paste this prompt:
 
```
Should our company adopt a 4-day work week? Give your recommendation.
```
 
Read the response. It's probably balanced and well-written. Now ask yourself: **could you actually make a decision based on this?** Look for specifics — does it give you a financial estimate of the cost? A concrete legal risk? A specific metric you could track? Note how many *actionable details* (numbers, named risks, specific recommendations) it includes.
 
<br><br>
 
**Step 2 — Design your own expert panel.** Before looking ahead, think about this question: *Which 3-4 expert roles would give you the most useful range of perspectives on the 4-day work week decision?*
 
Write down your panel (just the role names and what each should focus on), as in this kind of pattern:
 
```text
Expert # - Position
Focus on: responsibilities
Provide: types of assessment, risks, actions, or strategies
```
 
<br><br>
 
**Step 3 — Run the panel.** Paste a multi-expert prompt using either your own panel design or this reference:
 
```
Task: Analyze whether a 200-person software company should adopt a 4-day work week.
 
Simulate a panel of 4 experts. Each expert must provide their analysis independently:
 
Expert 1 - HR Director (15 years experience):
Focus on: employee retention, recruitment advantage, burnout, morale
Provide: Assessment + specific risk + specific benefit
 
Expert 2 - CFO:
Focus on: productivity impact, revenue implications, operational costs, client coverage
Provide: Assessment + financial estimate + key assumption
 
Expert 3 - Engineering Manager:
Focus on: sprint velocity, code quality, collaboration time, deep work
Provide: Assessment + metric prediction + mitigation strategy
 
Expert 4 - Employment Lawyer:
Focus on: labor law compliance, contract modifications, overtime implications, precedent cases
Provide: Assessment + top legal risk + required action
 
After all 4 experts present, provide:
- Points of agreement across experts
- Points of conflict between experts
- A synthesis recommendation with confidence level (low/medium/high)
- The single most important factor the decision hinges on
```
 
Now note the actionable details again — financial estimates, named legal risks, specific metrics, concrete recommendations. Were there significantly more or less that the panel surfaced compared to Step 1? The difference isn't just more perspectives — it's that each expert is forced to provide *specifics* rather than hedged generalities.
 
<br><br>
 
**Step 4 — Apply the panel to your own decision.** Think of a real decision you're facing at work (adopting a tool, changing a process, launching something new) or make one up. In the same conversation, ask:
 
```
Now apply the same multi-expert panel format to this decision: [describe your decision in 1-2 sentences]. Adapt the expert roles to be relevant to this specific domain.
```
 
Did the AI choose useful expert roles? Did any perspective surprise you?
 
<br><br>
 
**Step 5 — Flip the script with reverse prompting.** Now let the AI ask the questions. Paste:
 
```
I want to write a prompt that generates a comprehensive project proposal for a software project. But instead of me writing that prompt, I want you to help me discover what it needs.
 
Ask me a series of clarifying questions — one at a time — to understand:
- What kind of project this is for
- Who will read the proposal
- What sections are required
- What constraints exist
- What level of detail is needed
 
Ask your first question now. After I answer, ask the next one. After 4-5 questions, generate the complete prompt for me.
```
 
<br><br>
 
**Step 6 — Answer honestly.** Respond to each question the AI asks. Be as specific or vague as you'd naturally be — this tests whether the AI asks good follow-ups. Go through 4-5 rounds.
 
<br><br>
 
**Step 7 — Evaluate the generated prompt.** After the AI generates your prompt, score it:
- Did it include details you wouldn't have thought of on your own? (List 2-3 examples.)
- Does it incorporate all 6 building blocks from Lab 1?
- Is it more specific than what you would have written from scratch?
<br><br>
 
**Step 8 — Test the generated prompt.** Copy the prompt the AI built for you and run it. Does the output match what you described during the Q&A? Note any gaps — these reveal requirements the reverse prompting process missed.
 
<br><br>
 
**Step 9 — Add self-critique and framing.** Two more techniques can strengthen any analysis. Start a new conversation and paste this template — it combines multi-round self-critique with audience framing:
 
```
[INCENTIVE FRAME]
This analysis will inform a critical decision for [stakeholder].
Accuracy and completeness are essential. Take your time.
 
[CONTEXT]
Decision: Should our company adopt a 4-day work week?
Options: Full adoption, pilot program, or stay with 5-day
Current situation: 200-person software company, hybrid work
Timeline: Decision needed this quarter
 
[MULTI-ROUND ANALYSIS]
Round 1: Rate each option (1-10) with one-line justification.
Round 2: For the top option, list 3 strongest counter-arguments. Re-rate.
Round 3: Final recommendation with:
- Confidence level (0-100%)
- Biggest risk of recommended option
- What would change your recommendation
 
[FORMAT]
Present as structured sections with clear headers.
Keep total response under 500 words.
```
 
Notice the two additions: the **incentive frame** at the top ("This will inform a critical decision for...") signals high stakes and encourages thoroughness. The **multi-round structure** forces the AI to critique its own initial assessment before committing. Together, these produce more nuanced analysis than a single-pass prompt. Try editing the stakeholder (e.g., "the board of directors" vs. "a junior manager") — does the depth change?
 
<br><br>
 
**Step 10 — Reflect on when to use each.** You now have four techniques that solve different problems:
- **Multi-expert** is best when you know the question but need to stress-test it from multiple angles.
- **Reverse prompting** is best when you don't know what you don't know — when the problem space is unfamiliar.
- **Self-critique rounds** are best for high-stakes decisions where you need the AI to challenge its own assumptions before committing.
- **Incentive framing** ("This will be presented to...") is best when you want more depth and precision without changing the analysis structure.
- **Combined**: Use reverse prompting first (discover requirements), then feed into a multi-expert panel (stress-test), then add self-critique rounds for the final recommendation. Write down one real situation where this layered approach would be valuable.
<br><br>
 
<p align="center">— — —&nbsp;&nbsp;&nbsp;END OF LAB 4&nbsp;&nbsp;&nbsp;— — —</p>
<br><br>
 
---
 
<br>

## Lab 5: Prompting for an AI Agent 
 
**Goal**: See how the system prompt controls a real AI agent's behavior, then modify it to change that behavior — without touching any code.
 
**What you'll learn**: How agent prompts differ from chat prompts, and how editing a system prompt changes autonomous AI behavior in real time.
 
**Setup**: Open the GitHub Codespace for this lab. The weather agent is pre-built and working — you'll only edit the prompt text inside the code, never the code itself.
 
### Steps
 
<br>
**Step 1 — Run the working agent.** In the Codespace terminal, run:
 
```
python agent.py
```
 
When prompted, enter: **Paris, France**
 
Watch the output carefully. You'll see a loop: the agent **Thinks** (reasons about what to do), takes an **Action** (calls a weather tool), receives an **Observation** (the tool's result), then produces a **Final Answer**. This is the ReAct loop (Reason + Act) — Thought → Action → Observation, sometimes called TAO. Note the exact format of the tool call — the agent outputs `Action:` and `Args:` on specific lines.
 
![initial run](./images/prompt-accel5.png?raw=true "initial run")
 
<br><br>
 
**Step 2 — Read the system prompt.** Open `agent.py` in the Codespace editor. Find the `SYSTEM` prompt — it's a large text string starting at section "5" around line 66.  Read through it and identify these sections:
- **Tool definition**: Where does it describe the `get_weather` tool and its parameters?
- **Format rules**: Where does it specify the Thought/Action/Args/Observation format?
- **Critical rules**: Where does it tell the agent when to STOP and wait?
Don't modify anything yet — just map the sections.
 
```
code agent.py
```
 
![system prompt](./images/prompt-accel6.png?raw=true "system prompt")
 
<br><br>
 
**Step 3 — Map prompt to behavior.** Go back to the output from Step 1. For each behavior you observed, find the **specific line** in the SYSTEM prompt that caused it:
- The agent reasoned before acting → which line?
- The tool call used a specific format (Action/Args) → which line?
- The agent waited for the tool result before continuing → which line?
This is the key insight: every behavior you saw came from a line in the prompt. The Python code is just plumbing — the prompt is the control layer.
 
<br><br>
 
**Step 4 — Test a fictional city.** If not already running, start the agent again. Then enter: **Atlantis**
 
The agent has no geocoding tool — it converts city names to latitude/longitude coordinates by itself. Watch what happens: it will **make up coordinates** for Atlantis, call `get_weather` with those fake coordinates, and confidently report real weather data for some random spot on Earth. It didn't refuse, didn't say "I'm not sure Atlantis exists" — it hallucinated coordinates and presented the result as fact.
 
 
![Fictitious city](./images/prompt-accel9.png?raw=true "Fictitious city")
 
This is a prompt gap. The SYSTEM prompt has no rule telling the agent to verify a city is real before looking up its weather.
 
<br><br>
 
**Step 5 — Fix the prompt (two parts).** You might expect that adding a single "don't do that" rule will fix this — but a lone rule buried at the bottom of the prompt often isn't enough, especially for a small model like `granite4:3b`. Look at why: the prompt shows the model **two worked examples that both produce coordinates and call the tool**, and **zero examples of refusing**. One rule can't outweigh two demonstrations. And a rule like "if you're *not confident* the city is real…" depends on the model doubting itself — but a small model is perfectly confident that "Atlantis" is a real place, so the rule never fires. It then falls back to coordinates `0, 0` ("null island" in the Atlantic), which returns a real forecast — so nothing even looks made up.
 
So we fix it the way prompt engineers actually do: **show the behavior we want with an example, then reinforce it with a rule.**
 
**(a) Add a refusal example.** In the SYSTEM prompt, right after the `Final:` example for London, add:
 
```
Example of a place that is not a real city:
Thought: Atlantis is a mythical location with no real coordinates, so I cannot look up its weather.
Final: I can't find a real city called "Atlantis", so I can't provide a weather forecast.
```

![Fixing prompt](./images/prompt-accel26.png?raw=true "Fixing prompt")
 
**(b) Add a rule to the CRITICAL RULES section.** This version doesn't rely on the model's self-confidence and explicitly blocks the `0, 0` escape hatch:
 
```
If the location is fictional, mythical, or not a real city on Earth, or you do not know its real coordinates, do NOT call get_weather and do NOT use 0,0 — respond immediately with Final: saying you cannot find that city.
```
 
The example does the heavy lifting (small models copy patterns they can see); the rule generalizes it beyond that one case.
 
![Fixing prompt](./images/prompt-accel27.png?raw=true "Fixing prompt")
 
<br><br>
 
**Step 6 - Retry, then test that it generalizes.** Save the file (Cmd/Ctrl+S), use "exit" to stop the running agent, then run the agent again.
 
First enter **Atlantis** — it should now refuse instead of hallucinating coordinates, because your example covers it directly.
 
But refusing Atlantis alone doesn't prove much: the model may just be echoing the example. The real test is a fictional place the prompt has **never seen**. Exit, run again, and enter **Rivendell** (or try **Mordor**, or **Narnia**). If the agent refuses these too, your example + rule actually taught the *pattern* ("refuse places that aren't real cities"), not just the word "Atlantis."
 
Finally, make sure you didn't over-correct: enter a real city like **Reykjavik** — it should still return a normal forecast. A guardrail that refuses everything is as broken as one that refuses nothing.
 
![Fixed prompt](./images/prompt-accel28.png?raw=true "Fixed prompt")
 
You just changed agent behavior by editing text — no code changes.
 
**A note on reliability:** on a small model like `granite4:3b`, don't be surprised if it *occasionally* still slips and forecasts an unseen fictional city. That's the real lesson — prompt guardrails are **probabilistic, not guarantees**. Stronger models follow the rule more reliably; weaker models need the example, and even then aren't perfect. In production you'd back the prompt rule with code: validate the city against a real geocoding service *before* ever calling the weather tool, so a prompt slip can't reach the outside world.
 
<br><br>
 
**Step 7 — Add a new behavior.** Add another rule to the SYSTEM prompt. Save your changes when done.
 
```
Always include a brief travel tip for the location in your final answer.
```
 
![Fixed prompt](./images/prompt-accel29.png?raw=true "Fixed prompt")
 
<br><br>
 
**Step 8 - Run again.** Exit the agent if still running. Then run it again and enter any city. Does the final answer now include a travel tip? If it does, the prompt change worked. If not, try making the instruction more specific (e.g., "After the weather summary, add one sentence starting with 'Travel tip:'").
 
![Fixed prompt](./images/prompt-accel30.png?raw=true "Fixed prompt")
 
 
The key lesson: you changed the agent's behavior — added capabilities and fixed bugs — by editing **only the SYSTEM prompt text**. You never modified a line of Python. The prompt is the control layer for AI agents.
 
<br><br>
 
**Step 9 — Notice the pattern.** Agent prompts need four elements that chat prompts don't:
- **Tool definitions** — what tools exist and what they do
- **Format rules** — how the agent communicates (Thought/Action/Observation)
- **Execution rules** — when to act vs. wait, how to sequence steps
- **Guardrails** — what NOT to do, when to stop
These are the same prompt engineering concepts from Labs 1-4 (role, constraints, format), but now they control autonomous behavior instead of a single response.
 
<br><br>
 
<p align="center">— — —&nbsp;&nbsp;&nbsp;END OF LAB 5&nbsp;&nbsp;&nbsp;— — —</p>
<br><br>
 
---
 
<br>

## Lab 6: Working with Prompts in MCP
 
**Goal**: Learn how MCP (Model Context Protocol) separates prompt design from agent code — so you can update prompts on a server without touching the agents that use them.
 
**What you'll learn**: How to create, modify, and parameterize prompt resources on an MCP server, and why this separation matters for managing AI at scale.
 
**Setup**: This lab uses two terminals in the Codespace — one for the MCP server, one for the client agent. The code is pre-built; you'll only edit prompt text on the server side.
 
### Steps
 
<br>
**Step 1 — Start the MCP server.** In the first terminal, run:
 
```
python mcp_server.py
```
 
 
<br><br>
 
**Step 2 — Run the client agent.** Split the current terminal or open a second terminal and run:
 
```
python mcp_client_agent.py
```
 
Watch the startup — the client automatically **discovers** the available prompts and tools from the server. It doesn't have these built in; it asks the server what's available. This is MCP's discovery mechanism.
 
<br><br>
 
**Step 3 — Test the "summarize" prompt.** When the client prompts you, enter `summarize` as the tool. Then paste some text (a paragraph from any article, or make something up). Watch both terminals:
- The **client** sends your text to the server
- The **server** applies its summarize prompt template to your text
- The result flows back to the client
```
summarize
```
 
Here's some example text if you want to use it:
 
```
Artificial intelligence is becoming a common part of everyday life, from recommendation systems and virtual assistants to tools that help businesses analyze data more quickly. While these systems can save time and improve decision-making, they also raise important questions about privacy, accuracy, and fairness. Organizations that adopt AI successfully usually combine the technology with clear goals, reliable data, and human oversight rather than expecting it to solve every problem on its own. As AI continues to evolve, the most effective users will likely be those who understand both its strengths and its limitations.
```
 
The prompt that controlled the summarization lives on the **server**, not in the client code.
 
![MCP server and agent](./images/prompt-accel15.png?raw=true "MCP server and agent")
 
<br><br>
 
**Step 4 — Read the server's prompt code.** Open `mcp_server.py` in the editor. Find the function decorated with `@server.prompt("summarize")`. Inside, you'll see a prompt template with a `{text}` placeholder. This is the **prompt resource** — it's defined and managed on the server, and any agent that connects to this server can use it.
 
Notice the structure: the decorator names the prompt, the function returns the template, and `{text}` is where the user's input gets inserted.
 
![Prompt template](./images/prompt-accel16.png?raw=true "Prompt template")
 
<br><br>
 
**Step 5 — Modify a prompt.** Change the `prompt_summarize` function in `mcp_server.py` so its template is more specific. Replace the existing function body with the version below (this keeps the same `dict`-returning pattern the other prompts use — only the instruction text changes):
 
```python
@server.prompt("summarize")
def prompt_summarize(text: str) -> dict:
    """You are a technical writer. Summarize the following text in exactly 2 bullet points, focusing on actionable takeaways."""
    print(f"{BLUE}[PROMPT REQUEST] summarize called with text: {text!r}{RESET}")
    result = {
        "role": "user",
        "content": (
            "You are a technical writer. Summarize the following text in exactly "
            "2 bullet points, focusing on actionable takeaways.\n\n"
            f"{text}"
        )
    }
    print(f"{GREEN}[PROMPT RETURN] summarize response: {result}{RESET}")
    return result
```
 
![Updated prompt resource](./images/prompt-accel24.png?raw=true "Updated prompt resource")
 
 
<br><br>
 
**Step 6 - Rerun.** Save the file, restart the server (Ctrl+C in the first terminal, then `python mcp_server.py` again), and restart the client. Test with the same text as Step 3.
 
Did the output change? You changed the agent's behavior by editing the **server** — the client code didn't change at all.
 
![Revised output](./images/prompt-accel25.png?raw=true "Revised output")
 
 
<br><br>
 
This shows the MCP advantage: the prompt lives on the server, not in the agent. In Lab 5, the prompt was embedded in the agent's code. Here, it's a **resource** that can be updated independently. Imagine 10 different agents all using this server's "summarize" prompt — you update it in one place, and all 10 agents get the new behavior instantly.
 
<br><br>
 
**Step 7 — Create a new prompt resource.** Add a new prompt to the server. The existing prompts are `summarize`, `reword`, and `expand` — all general text transformations. You'll add `action_items`, which extracts to-do items from text.
 
First, add the prompt resource in `mcp_server.py`:
 
```python
@server.prompt("action_items")
def action_items_prompt(text: str) -> dict:
    """Extract action items from meeting notes or documents."""
    print(f"{BLUE}[PROMPT REQUEST] action_items called with text: {text!r}{RESET}")
    result = {
        "role": "user",
        "content": (
            "Extract all action items from the following text. Return each as a "
            "bullet point starting with a verb. If no action items exist, say "
            "'No action items found.'\n\n"
            f"{text}"
        )
    }
    print(f"{GREEN}[PROMPT RETURN] action_items response: {result}{RESET}")
    return result
```
 
![Additional prompt resource](./images/prompt-accel31.png?raw=true "Additional prompt resource")
 
<br><br>
 
**Step 8 - Add the tool stub.** Scroll down to around line 82. Then add the matching tool stub so the client can invoke it (follow the pattern of the existing tool functions):
 
```python
@server.tool("action_items")
def action_items_tool(text: str) -> str:
    print(f"{BLUE}[TOOL REQUEST] action_items_tool received input: {text!r}{RESET}")
    print(f"{GREEN}[TOOL RETURN] action_items_tool returning: {text!r}{RESET}")
    return text
```
 
![Additional tool stub](./images/prompt-accel32.png?raw=true "Additional tool stub")
 
<br><br>
 
**Step 9 - Run again.** Save the file. **Restart both the server and client.** Choose the function `action_items`. Test by pasting a paragraph of meeting notes as in the sample below or make some up.
 
```
action_items
```
 
Sample:
```
We agreed to launch the beta by Friday. Sarah will update the docs and Mike needs to fix the login bug before Thursday.
```
 
![New function](./images/prompt-accel21.png?raw=true "New function")
 
<br><br>
 
**Step 10 — Connect back to prompt engineering.** Look at the prompts you've written on the MCP server. They use the same techniques from earlier labs:
- **Role**: "You are a technical writer" (Lab 1 building block)
- **Constraints**: "exactly 2 bullet points", "starting with a verb" (Lab 3 production techniques)
- **Format**: "actionable takeaways", "bullet point" (Lab 3 structured outputs)
MCP doesn't replace prompt engineering — it gives your prompts a standard way to be discovered, shared, and updated across multiple agents.
 
<br><br>
 
**Step 11 — Reflect.** You've now seen two levels of prompt engineering for AI systems:
- **Lab 5**: System prompts that control a single agent's behavior. The prompt is embedded in the agent code. To change behavior, you edit the agent's file.
- **Lab 6**: MCP prompt resources that live on a server. Multiple agents can discover and use them. To change behavior, you edit the server — agents pick up the change automatically.
Both use the same prompt techniques you learned in Labs 1-4. The difference is **where the prompt lives and who controls it**. As AI systems grow from a single agent to many agents working together, MCP's separation of prompts from agents becomes essential.
 
Consider which of these two approaches (embedded prompt vs. MCP resource) would work better for your team's AI workflows, and why.
<br><br>
 
<p align="center">— — —&nbsp;&nbsp;&nbsp;END OF LAB 6&nbsp;&nbsp;&nbsp;— — —</p>
<br><br>
 
---
 
<br>
## Appendix: Quick Reference Card
 
### The 6 Building Blocks
1. **Task** — One clear, testable instruction
2. **Context** — Background, audience, environment
3. **Role** — Persona with relevant expertise
4. **Format** — Output structure (table, JSON, bullets)
5. **Examples** — Input/output pairs (few-shot)
6. **Constraints** — Boundaries (length, tone, exclusions)
### Technique Selection Guide
 
| Situation | Technique | Why |
|-----------|-----------|-----|
| Simple, common task | Zero-shot | Fast, low cost |
| Need consistent format | Few-shot | Examples anchor structure |
| Complex reasoning | Chain of Thought | Better, auditable multi-step reasoning |
| Multi-stakeholder decision | Multi-expert | Catches blind spots |
| Unclear requirements | Reverse prompting | AI discovers what you need |
| High-stakes output | Self-critique + incentive framing | Maximum thoroughness |
| Production integration | Structured + constrained | Consistent, parseable output |
 
### Quality Metrics Checklist
- Does the output follow the requested format?
- Is it consistent across multiple runs?
- Is the content accurate and grounded?
- Could downstream systems parse it reliably?
- Would a domain expert approve the output?
---
*© 2026 Tech Skills Transformations*
