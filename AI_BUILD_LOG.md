# AI Build Log: Offer Copilot

**Razorpay AI x PM Build Challenge · Track 2: Recover and Grow with AI**
Akshay Pratap Singh · ISB PGP 2027
Live prototype: https://offer-copilot-blush.vercel.app/

Offer Copilot is a concept for the Razorpay Dashboard. It helps a small merchant pick a card offer that actually pays, ask the bank to share the cost, and prove what the offer earned using a holdout group. All data in the prototype is synthetic.

---

## 1. AI tools and models used

| Tool | Model / version | What I used it for |
| --- | --- | --- |
| ChatGPT | GPT-6 Luna (High reasoning), as selected in my ChatGPT chat | Understanding the Razorpay merchant journey, early brainstorming, overlap check against Razorpay's public docs, first HTML prototype (v2) |
| Claude (Cowork, Claude app) | Claude Opus 5.5 | Critique of v2, rebuilding the prototype twice, the offer profit model, bank co-funding flow, automated click-testing, deployment, demo video, desk research with sources, product note and this log |
| Claude in Chrome | Browser extension driven by Claude | Reading my ChatGPT thread, checking the live Vercel site loads and every flow runs without errors |
| Vercel MCP connector | Vercel API via Claude | Deploying the prototype as a static site |
| Kokoro TTS | Kokoro-82M v1.0 (open source, int8 ONNX), voice `af_heart` | Voiceover for the 90-second demo video |
| Playwright + headless Chromium | Not AI; automation scripts written by Claude | Screenshots of the prototype, frame-by-frame video rendering, scripted tests of each flow |
| ffmpeg | Not AI | Encoding the video and mixing voice with background audio |

External services: GitHub (source), Vercel (hosting), Google Fonts (Plus Jakarta Sans, JetBrains Mono). Public data sources are listed in the product note.

---

## 2. Key prompts, workflows and agent setup

### Workflow

1. **Understand the merchant (ChatGPT).** Walked through the Razorpay merchant journey and brainstormed problems.
2. **Pick and pressure-test an idea (ChatGPT).** Parked my first idea, then explored bank and card offers, and asked for an overlap check against Razorpay's existing products.
3. **Critique and rebuild (Claude).** Shared the challenge brief, my review analysis and the ChatGPT thread, and asked Claude to judge the v2 prototype against the brief.
4. **Iterate on the product, not the pixels.** Each round I named a specific gap: no alternatives, no bank consent, looks like a tutorial. Claude rebuilt and then ran a scripted click-through of every path before showing me.
5. **Ship.** Claude deployed to Vercel through the connector. I created the GitHub repo and uploaded the files.
6. **Explain it.** Claude wrote the video script, generated the voice with Kokoro, rendered animated scenes timed to the audio, and built the product note from sourced research.

### Agent setup

- **One main agent (Claude in Cowork)** with tools for file editing, a shell sandbox, Claude in Chrome, the Vercel connector, web search and fetch, and Claude Docs.
- **No autonomous publishing.** Every deploy and every public link was something I asked for in chat.
- **A test harness instead of trust.** For each build, Claude ran a Playwright script that clicked through every flow (plans, editor, approve, counter, decline, live, results) and checked phone width for horizontal scroll, and reported any JavaScript errors.
- **Video pipeline:** script (JSON) → Kokoro TTS per scene → a timeline built from audio lengths → an HTML stage rendered frame by frame in headless Chromium (2,729 frames) → ffmpeg mux with voice and background audio.

### Key prompts (lightly edited from what I typed)

> "Let's build on this merchant care rewards idea... make sure the merchant should not be fooling the Razorpay app, otherwise they can fool and chill on reward points."

> "Can we come back to the other idea... deciding the payment and merchant bank credit card? You said there was already some existing thing. Can we brainstorm on that?"

> "Ensure that it is not being already implemented in some dashboard or agentic AI."

> "This HTML file does not look up to the mark. It is giving a suggestion and just asking to review it. It's not giving other options or how can I change it. Improve the idea."

> "Before releasing the offer, should they take consent from the bank or not? Who bears this cost? There is no option of sending the request to the bank."

> "This does not look like an application, I can see instructions on the left. Build it like a real app, with a bank request flow."

> "Talk about the problem at the start with an animated merchant figure, show how he tries to reach the bank, then show Offer Copilot powered by Razorpay."

> "Do some research on why this needs to be addressed. Bring numbers with sources. Show what Razorpay solves today and what we add on top."

---

## 3. One AI output I rejected and how it improved

**Rejected: the v2 "Offer Planner" prototype from ChatGPT.**

It looked polished, with a Razorpay-style dashboard, bank payment mix and a seasonal chart. But the core of it was a single card saying "Test a Kotak card offer for Diwali" with a "Review this offer" button.

**Why I rejected it**

- It gave **one answer with no alternatives**, so a merchant couldn't compare options or see trade-offs.
- It **didn't show what a change would do**. Edit the discount and you got no sense of profit or risk.
- It treated the AI as a recommender, while the brief asks for AI that helps the merchant **make and execute a better decision**.

**What it became**

- **Three priced plans** (Careful, Balanced, Big push), with a recommended pick and the reason the other two lost.
- **A live editor** where every change re-prices profit, cost split and risk, plus a profit curve showing where the discount peaks.
- **A comparison against a fixed rule** on the same audience (10% off all Kotak orders: about ₹2.2k lost; Balanced plan: about ₹4.6k gained, sample data).

**A second correction:** my own question about bank consent exposed that no version had a way to ask the bank. That became the co-funding request flow with approve, counter and decline paths, plus a fallback the merchant picks upfront.

**A third correction:** Claude's first rebuild put step-by-step instructions in a left rail. I rejected it because it read as a tutorial, not a product. The final version is a normal dashboard app.

---

## 4. What AI did vs what I decided or built

| AI did | I decided or did |
| --- | --- |
| Generated ideas and the first prototypes | Chose Track 2 and the merchant problem |
| Checked Razorpay docs for overlap | Parked my first idea (Care Credits) because merchants could game verification for rewards |
| Wrote all prototype code, the profit model and test scripts | Insisted the idea must not duplicate existing Razorpay features |
| Proposed plan structures, alert types and guardrails | Rejected the single-suggestion design and asked for options and editability |
| Deployed to Vercel and verified the live site | Raised who pays and whether the bank must approve, which defined the bank flow |
| Wrote the video script, generated the voice, rendered the animation | Rejected the tutorial-style UI; directed the video story (merchant, bank dead end, reveal) |
| Found and cited market data (RBI, CARE Ratings, Paisabazaar, Nielsen) | Read and tagged 558 negative Razorpay reviews to understand merchant trust (my own analysis) |
| Drafted the product note and this log | Chose what to leave out (chatbot, auto-publishing, bank portal, mobile-first); created the GitHub repo and submitted |

---

## 5. Known limitations, risks and verification steps

### Limitations

- **Synthetic data.** The merchant (Nivara Home), payments, issuer shares and results are invented to show the experience.
- **The profit model is illustrative.** It models four things: buyers coming anyway, buyers the discount brings in, buyers who grow their basket to reach the minimum, and fatigue. Its parameters are hand-set, not fitted on real data.
- **The bank's reply is simulated.** The approve, counter and decline buttons are clearly labelled demo controls. There is no real bank integration.
- **Frontend only.** The prototype is a single static HTML file. State is saved in the viewer's browser and can be reset.
- **Voice.** The demo uses an American English AI voice; no Indian English voice was available in the toolchain.

### Risks if built for real

- **Wrong estimates** could push merchants into loss-making offers. Mitigated by ranges with confidence labels, hard budgets, and holdouts on by default.
- **Offer abuse** (card reuse, buy-discount-return). Mitigated by one discount per card per day and an auto-pause on refund spikes.
- **Over-automation** eroding trust. Mitigated by approve-before-launch and a merchant-chosen autonomy level.
- **Bank expectations.** A request is never shown as an approval, and a fallback is always set.

### Verification steps I took

- **Feature claims:** checked against Razorpay's own [Create Offers docs](https://razorpay.com/docs/payments/offers/create) and the [Agent Studio launch post](https://newsroom.razorpay.in/newsroom/razorpay-launches-the-worlds-first-ai-native-agent-studio-for-payments-at-ftx26-powered-by-anthropics-claude/), not taken from chat answers.
- **Market numbers:** every figure in the product note links to the page it came from. One caveat: the Nielsen promotion study covers consumer goods in the US and Europe, not India.
- **Prototype behaviour:** scripted tests clicked through every flow, checked for JavaScript errors and checked phone width for overflow. The live Vercel site was opened and the bank approve, counter and decline flows were run there too.
- **Model sanity:** the plan profits for the Kotak scenario were worked through step by step, and the recommendation logic was adjusted so it picks the plan with the best risk-adjusted profit rather than the most cautious one.

### Next steps to validate

1. Fit the model on real issuer-level payment history.
2. Back-test on last year's festive offers.
3. Run a 20-merchant pilot with holdouts for one festive season.

---

*No API keys, credentials, private data or proprietary information are included in this repository or log.*
