# FitFindr — planning.md

> Complete this document before writing any implementation code.
> Your spec and agent diagram are what you'll use to direct AI tools (Claude, Copilot, etc.) to generate your implementation — the more specific they are, the more useful the generated code will be.
> Your planning.md will be reviewed as part of your submission.
> Update it before starting any stretch features.

---

## Tools

List every tool your agent will use. For each tool, fill in all four fields.
You must have at least 3 tools. The three required tools are listed — add any additional tools below them.

### Tool 1: search_listings

**What it does:**
Search listings should take a user query describing what the user is looking for, search the database of listings for a match, and return a list of items that match the query. The search should be flexible enough to handle a variety of queries, including those that describe style preferences, price range, and specific item types.

**Input parameters:**
<!-- List each parameter, its type, and what it represents -->
- `description` (str): - represents the description of the item the user is looking for, including style preferences and specific item types.
- `size` (str): - represents the size of the item the user is looking for.
- `max_price` (float): - represents the maximum price the user is willing to pay for the item.

**What it returns:**
It should return a list of dicts, where each dict is a listing that matches the query.

**What happens if it fails or returns nothing:**
Tell the user that no items were found matching their query and suggest they try a different search or adjust their criteria.

---

### Tool 2: suggest_outfit

**What it does:**
Suggest outfit should take a new item (the one the user is interested in) and the user's existing wardrobe, and suggest an outfit that incorporates the new item with items from the wardrobe.

**Input parameters:**
<!-- List each parameter, its type, and what it represents -->
- `new_item` (dict): - represents the new item the user is interested in.
- `wardrobe` (dict): - represents the user's existing wardrobe.

**What it returns:**
Returns a dict with the suggested outfit, including the new item and the items from the wardrobe that go well with it.

**What happens if it fails or returns nothing:**
Tell the user that no outfit suggestions could be made with the new item and their existing wardrobe, and suggest they try a different item or add more items to their wardrobe for better suggestions.
---

### Tool 3: create_fit_card

**What it does:**
It should return a sharable desc of an outfit that the user can send/share with friends.
**Input parameters:**
- `outfit` (str): Outfit description to be shared.
- `new_item` (dict): - The new item that the user is interested in.

**What it returns:**
A string that describes the outfit in a sharable format.

**What happens if it fails or returns nothing:**
Tell the user that the fit card could not be created and suggest they try again or adjust the outfit description.

---

### Additional Tools (if any)



---

## Planning Loop

**How does your agent decide which tool to call next?**
The agent will first call search listings to find items that match query, then depending on output from search listings, it will call suggest outfit to suggest an outfit with the new item and existing wardrobe, and finally it will call create fit card to create a sharable description of the outfit.

---

## State Management

**How does information from one tool get passed to the next?**
Each tool will return its output to the planning loop, which will store the output in a state object. The state object will be passed to the next tool as input, allowing the agent to maintain context and continuity throughout the interaction.
---

## Error Handling

For each tool, describe the specific failure mode you're handling and what the agent does in response.

| Tool | Failure mode | Agent response |
|------|-------------|----------------|
| search_listings | No results match the query | If no results are found, the agent will inform the user and suggest adjusting their search criteria. |
| suggest_outfit | Wardrobe is empty | If the wardrobe is empty, the agent will tell the user and suggest adding more items to their wardrobe for better suggestions. |
| create_fit_card | Outfit input is missing or incomplete | If the outfit input is missing or incomplete, the agent will ask the user to provide more details. |

---

## Architecture

https://mermaid.live/edit#pako:eNqNUtuO2jAQ_RXL-7JIbJo4kDRR1Rd2t12paquFvjQgZMjkog125IsKBf69E3NpQTzUD_GM58yck6PZ0qXMgaa0VLytyORxqqaC4PmhQd1n3Ze8iNaaWY88PHwkX6Rss-8NF6IWpctmp44uIR860NhwA9m9u8g7Mgatayl6DnlCa7s4UkrZaJJ1F3law9IaxJ6HdmcSZBq4WlbzptYGefVlmWXaliVoM5fWFLW5rIbZUgEKmWNlvuQqP5dB5P8qcvpR_i7wyHMtcvJiYKV3SH9CTAJXfwVtG6NJIa3Id67vCvHM68Yq2JEnpSTa-JmLvIFDhoZ8laYuNs7iWe-mBOaRTyBAdQZ-cz-FOtiZhTnQoUDKI_BaCrsh5SZZ6JGRs4iMK47BAqWO0CikDM_TQgftnsnBz2u68H_pXsFYJQi62EqhEdr5cLEaZoMKDttT1E2T3hVJ0ddGyTdI78IwPMYPv-rcVClr15edjunQCEExLODc6wfDOFnQPm57ndPUKAt9ugK14l1Kt92UKTUVrGBKUwxzrt6mdCr22NNy8VPK1alNSVtWNC14ozGzbY5yH2uOS_0XghsGaoRrYmga-IGbQdMtXdM0TrxkEEfBcMD8OPaDqE83NI18Lx7EA8bwKQriJNr36W9H6nvv46GPhyURY9FwEO__AMIDMr0

<!-- Draw a diagram of your agent showing how the components connect:
     User input → Planning Loop → Tools (search_listings, suggest_outfit, create_fit_card)
                                                                          ↕
                                                                   State / Session
     Show what triggers each tool, how state flows between them, and where error paths branch off.
     Use ASCII art or a Mermaid diagram (https://mermaid.js.org/syntax/flowchart.html).
     Do NOT embed an image — graders need to read your diagram directly in the file;
     an embedded image or screenshot cannot be evaluated.
     You'll share this diagram with an AI tool when asking it to implement
     the planning loop and each individual tool. -->

---

## AI Tool Plan

<!-- For each part of the implementation below, describe:
     - Which AI tool you plan to use (Claude, Copilot, ChatGPT, etc.)
     - What you'll give it as input (which sections of this planning.md, your agent diagram)
     - What you expect it to produce
     - How you'll verify the output matches your spec before moving on

     "I'll use AI to help me code" is not a plan.
     "I'll give Claude my Tool 1 spec (inputs, return value, failure mode) and ask it to implement
     search_listings() using load_listings() from the data loader — then test it against 3 queries
     before trusting it" is a plan. -->

**Milestone 3 — Individual tool implementations:**
     
**Milestone 4 — Planning loop and state management:**

---

## A Complete Interaction (Step by Step)

Write out what a full user interaction looks like from start to finish — tool call by tool call. Use a specific example query.

**Example user query:** "I'm looking for a vintage graphic tee under $30. I mostly wear baggy jeans and chunky sneakers. What's out there and how would I style it?"

**Step 1:**
<!-- What does the agent do first? Which tool is called? With what input? -->

**Step 2:**
<!-- What happens next? What was returned from step 1? What tool is called now? -->

**Step 3:**
<!-- Continue until the full interaction is complete -->

**Final output to user:**
<!-- What does the user actually see at the end? -->
