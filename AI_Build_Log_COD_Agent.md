# AI Build Log: COD Agent for Magic Checkout

Razorpay AI x PM Build Challenge, Track 2 (Recover and Grow with AI) | Meghna, ISB

Prototype: https://magic-checkout-cod-agent-meghna.netlify.app

## 1. AI tools and models used

| Tool | Model / type | Used for |
| --- | --- | --- |
| Claude (claude.ai) | Anthropic Claude Opus, with web search | Market and competitor research, synthesis, persona and journey, prototype code, product note, one-pager |
| Playwright | Browser automation (not AI) | Scripted screen recording of the prototype for the demo video |
| Piper TTS | Offline text-to-speech model | Draft voiceover only. Replaced by my own voice in the final video |
| ffmpeg | Video tool (not AI) | Merging my voiceover with the recording |

## 2. Key prompts and workflow

The workflow ran in one sequence: research, segment, journey, reframe, build, communicate.

1. **Research:** "I am applying for Track 2. Do extensive preliminary market research. Understand the problem statement, then research what Razorpay is already doing." Claude read about 20 sources: Razorpay blogs and docs (Agent Studio, Agentic Dashboard, RAY, Vulcan, Magic Checkout), competitor launches (Cashfree Relay, PayU Agent HQ), and D2C return benchmarks.
2. **Segment and journey:** "Where are Agent Studio and Vulcan used? Who are the merchants we are talking about? Work on the user journey."
3. **Reframe (my pushback):** "We have removed Magic Checkout. Who does the COD risk analysis? This is a completely new product." See section 3.
4. **Build:** "Build the prototype. It has to follow the exact style of Magic Checkout," followed by screenshots of the Magic Checkout COD page as the visual reference.
5. **Communicate:** a one-page product note, checked line by line against the brief's five required points.

## 2a. Research depth: what I found, and what it changed

**What Razorpay already ships, and the gap.**

| Razorpay asset | What it does | Implication for my product |
| --- | --- | --- |
| Vulcan (Sep 2026) | Payments foundation model: routing, fraud, COD risk, checkout ordering. Trained on returned-parcel labels | Risk prediction is solved. Do not rebuild it |
| Magic Checkout COD settings | COD Intelligence, partial COD with risk slabs, COD fee, convert-to-prepaid discount over WhatsApp, manual review | Every lever exists, but each is static and global |
| Agent Studio (Mar 2026) | 8 pre-built agents, including RTO Shield and RTO Insights, plus a no-code builder. Built on the Claude Agent SDK | RTO Shield flags risk. Nothing chooses the cheapest intervention per order |
| RAY on WhatsApp (Sep 2026) | AI account manager in English and Hindi, with tap-to-verify actions | The right channel for a founder who runs the business from a phone |

**Competitive scan.**
- Cashfree Relay went live for all merchants in August 2026, free at launch, with approval checkpoints before money moves or a customer is contacted.
- PayU Agent HQ runs one agent per job.
- Pine Labs added a mandate-checking trust layer.

Approval before action is now table stakes. My design adds a comparison group and an automatic pause, so the agent proves its value rather than claiming it.

**Opportunity spaces I considered and rejected.**

| Space | Size of the problem | Why I did not pick it |
| --- | --- | --- |
| UPI AutoPay failures | About 20 million mandates revoked a month, mostly for insufficient balance | Razorpay cannot see customer balances, so prediction quality is weak. Subscription Recovery already covers retries |
| SME B2B receivables | About ₹8.1 lakh crore stuck; invoices take 73 days against a 45-day legal limit | Largest gap, but Razorpay's data signal is thin and it sits outside its core online merchants |
| Failed-payment win-back | Card success sits well below UPI | RAY already sends payment links to failed transactions |
| **COD returns (chosen)** | COD is 50 to 70% of D2C orders; COD returns 28 to 35% vs 4 to 8% prepaid; returns can eat 8 to 15% of revenue | Razorpay's strongest proprietary asset meets the largest P&L leak, for merchants with no one to manage it |

**Unit economics behind the persona.** 3,000 orders × 60% COD = 1,800 COD orders. × 28% returns = 504 returns. × ₹200 direct cost = ₹1.01 lakh a month. 504 × ₹1,200 = ₹6.05 lakh of GMV never realised.

**Decision model in the prototype.** For each order, with contribution margin M (45% of cart), return risk r and a ₹200 return cost, the agent compares expected profit:
- Allow COD: (1 − r)·M − 200·r
- WhatsApp check: risk falls by 25%, minus ₹1. Used only if the gain is at least ₹15, to save Meta's limited message quota
- Prepaid discount d (₹20 to ₹40): c·[(0.94)(M − d) − 12] + (1 − c)·(allow COD), where c is the buyer's take-rate, with diminishing returns
- Partial COD ₹99: 82% of buyers stay, and their risk halves
- Prepaid only: 45% of buyers switch, at a 6% prepaid return rate

Guardrails are applied before the choice. For example, prepaid only is never allowed for a buyer with 2 or more delivered orders. Every number on the order screen comes from this model, not a hard-coded label.

## 3. One AI output I rejected, and why

**The first concept was a standalone agent.** Claude's first user journey had the merchant connect Shopify and Shiprocket to a new agent, and it treated risk scoring as something we would build.

**I rejected it.** Razorpay already scores every COD order with Vulcan, and Magic Checkout already has the levers: block COD, partial COD, a prepaid discount over WhatsApp, a COD fee and manual review. A new product would duplicate both, and the brief asks for an improvement, not a recreation.

**What changed:** the product became an upgrade to Magic Checkout. It keeps Razorpay's risk model and existing levers, and adds only what is missing: a per-order decision and a learning loop. The lesson: the gap is not in prediction, but in the decision.

I also made three smaller corrections:
- Claude advised keeping the dashboard styling because of the deadline. I overrode it and supplied Magic Checkout screenshots, because the prototype had to feel native to the product.
- I replaced the AI voiceover with my own voice.
- I checked the one-pager against the five required points and had the missing ones added: the current workaround, what I built, and what I would test next.

## 4. What AI did versus what I decided

| AI did | I decided or did |
| --- | --- |
| Gathered and summarised sources | Chose Track 2 and the SME D2C segment |
| Proposed three opportunity spaces | Picked COD returns and rejected the standalone framing |
| Drafted the persona, journey and numbers | Set the guardrails, approval model, 10% comparison group and auto-pause |
| Wrote the prototype code and profit model | Set the scope cuts: no new risk model, no courier selection, no marketplaces |
| Drafted the note, one-pager and video script | Chose the visual direction, recorded the voiceover, reviewed every output |

## 5. Known limitations, risks and verification

**Limitations:**
- All merchant, order and outcome data is synthetic.
- Return-risk scores stand in for Vulcan's output.
- The profit model rests on assumptions: 45% contribution margin, ₹200 per return, 18% drop-off on partial COD, and diminishing returns on discounts.
- Customer-reply parsing and RAY messages are scripted examples, not live AI calls.
- The prototype is not production code.

**Risks:**
- Market figures come partly from vendor blogs, so they are directional.
- A learning agent can over-fit a small merchant's data. The network-level risk score and the comparison group reduce this.
- Discounts can cut conversion. The 2-point auto-pause guards against it.

**Verification:**
- Checked Magic Checkout's COD features against Razorpay's official docs.
- Re-computed every on-screen decision against the profit model.
- Reviewed screenshots of every screen.
- Tested the public link in an incognito window.

**Next validation step:** a 4-week pilot on 20 Magic Checkout merchants, using real Vulcan scores and Razorpay return data, against a 10% comparison group.

No API keys, credentials, private data or proprietary information are included.
