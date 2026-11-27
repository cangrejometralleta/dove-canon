# DeLaCase Measurement

## First pass

One fictional library procedure provides three samples. Plain and DeLaCase use
the same words, punctuation and paragraph breaks. Mini uses Cadencia Dove Mini
with DeLaCase enabled and keeps the sentence order for comparison. One author
prepared the samples and scored their highlights against a manual key.

| Sample | Words | Tokens | Most marks in a passage | Key entries marked | Marks outside the key |
| --- | ---: | ---: | ---: | ---: | ---: |
| Plain | 255 | 277 | 0 | 0 of 53 | 0 |
| DeLaCase | 255 | 284 | 3 | 44 of 53 | 6 |
| Mini | 207 | 235 | 3 | 41 of 53 | 2 |

Counts use `tiktoken` 0.14.0 and `o200k_base` on each sample body, including
internal paragraph breaks. For this fixture, periods and commas delimit the
21 passages. The count exempts each sentence initial and the proper name
Saturday. A mark counts toward coverage when its word matches an entry in the
same sentence. Marks outside the key are counted by distinct word per sentence.

Changing capitalization alone added seven tokens (2.5%). Mini saved 42 tokens
(15.2%) compared with plain and marked 41 key entries. The key measures the
placement of highlights.

## Plain

```text
The library opens a repair desk on the first Saturday of every month to help people keep small appliances working. At the entrance, a volunteer records each visitor's name and asks what the appliance does when it fails. The volunteer assigns a ticket so the technician can call the visitor without reading their name aloud.

The technician checks the appliance for damaged wiring before plugging it in. If the wiring looks unsafe, the technician stops the inspection and gives the visitor a written explanation. The visitor can take the appliance home or leave it with the desk for recycling.

For appliances that pass inspection, the technician tries to reproduce the fault while the visitor watches. The technician explains the proposed repair and asks the visitor to approve any part that costs money. The desk keeps common parts in stock and adds their price to the ticket only after the visitor agrees.

The technician tests the appliance again after the repair and shows the visitor the result. If the test fails, the technician records the remaining fault and refunds the price of any part removed from the appliance. If the test passes, the visitor receives a receipt that lists the work and the parts used.

At closing, the coordinator counts completed repairs and records appliances still awaiting parts. The coordinator uses that list to place an order before the next session and calls visitors whose repairs can resume. The library publishes a monthly total of successful repairs and keeps names and ticket details at the desk.
```

## DeLaCase

```text
The Library opens a Repair Desk on the first Saturday of every month to help people keep small appliances working. At the entrance, a Volunteer Records each visitor's Name and asks what the appliance does when it fails. The volunteer Assigns a Ticket so the technician can call the visitor without reading their Name aloud.

The Technician Checks the appliance for damaged Wiring before plugging it in. If the Wiring looks Unsafe, the technician Stops the Inspection and gives the visitor a written Explanation. The visitor can take the appliance Home or Leave it with the desk for Recycling.

For appliances that pass Inspection, the Technician tries to Reproduce the Fault while the visitor watches. The technician explains the proposed Repair and asks the visitor to Approve any part that Costs money. The desk keeps common Parts in stock and adds their Price to the ticket only after the visitor Agrees.

The Technician Tests the appliance again after the Repair and shows the visitor the result. If the Test fails, the technician Records the remaining Fault and Refunds the price of any part removed from the appliance. If the test Passes, the Visitor receives a Receipt that lists the work and the Parts used.

At closing, the Coordinator Counts completed Repairs and records appliances still awaiting parts. The coordinator uses that list to place an Order before the next Session and Calls visitors whose repairs can resume. The library Publishes a monthly Total of successful repairs and keeps Names and ticket details at the desk.
```

## Mini

```text
The library opens its Repair Desk for small appliances on the first Saturday of each month. At the entrance, a volunteer Records your Name and asks how the appliance Fails. The volunteer Assigns a Ticket so the technician can call you by number.

The Technician Checks for damaged Wiring before plugging in the appliance. If the Wiring looks Unsafe, the technician Stops the inspection and gives you a written Explanation. Take it Home or leave it for Recycling.

If the appliance passes Inspection, the technician tries to Reproduce the Fault while you watch. The technician explains the Repair and asks you to Approve any paid Parts. The desk stocks common Parts and adds their Price to your ticket once you Agree.

The technician Tests the appliance after the Repair and shows you the Result. If the Test fails, the technician Records the fault and Refunds the price of any part they remove. If it Passes, you get a Receipt for the work and Parts.

At closing, the Coordinator Counts completed Repairs and records appliances awaiting parts. The coordinator Orders parts before the next session and Calls visitors whose Repairs can resume. The library Publishes the monthly repair Total and keeps Names and ticket details at the desk.
```

## Scoring key

Each line lists the words that carry the main entities or actions in the
corresponding sentence. Exact words count once per sentence, ignoring case.
The key gives alternate words where mini changes the phrasing.

```text
library repair desk month
volunteer records name fails
assigns ticket name/number
technician checks wiring
wiring unsafe stops explanation
home leave recycling
inspection reproduce fault
repair approve costs/paid parts
parts price agrees/agree
tests repair result
test records fault refunds
passes receipt work parts
counts repairs records awaiting
order/orders calls resume
publishes total names details
```


## Model reader trial

Five fresh sessions per version used Claude Sonnet 5.5
(`claude-sonnet-5-5`), low effort, through Claude Code 2.1.331. Each
session had tools disabled and received one version of the procedure,
with its five paragraphs numbered. The questions, rubric and clarification
prompt stayed fixed. Run order was shuffled with seed 271. The OpenAI API
attempt returned an exhausted credit balance before collecting any answers.

Each question had two criteria worth one point each. The grader reviewed
the answers by run ID and recorded omissions before opening the version
mapping. Wording in the answers could still reveal the source version.

| Version | Runs | Total score | Median and range per run | Paragraphs requested for clarification |
| --- | ---: | ---: | ---: | ---: |
| Plain | 5 | 120 of 120 | 24 (24–24) | 0 |
| DeLaCase | 5 | 120 of 120 | 24 (24–24) | 0 |
| Mini | 5 | 110 of 120 | 22 (22–22) | 0 |

Plain and DeLaCase both reached the ceiling, so this trial leaves the
effect of capitalization on comprehension unresolved. Every Mini answer
omitted that the post-repair test was a repeat and that the published total
counted successful repairs. Mini drops both qualifiers from the source.
Even if testing after repair earns the first point by context, Mini scores
23 of 24 in every run. The remaining deduction is tied to a wording change.

### Questions and rubric

Readers were asked to answer using only the passage, give one brief answer
per question in order, and say when the passage did not provide an answer.
They also listed any paragraph they would want re-explained before applying
the procedure. Each rubric cell represents one point and accepts equivalent
wording.

| # | Question | Criterion A | Criterion B |
| --- | --- | --- | --- |

| 1 | When does the desk open, and what kind of item does it handle? | First Saturday of each month | Small appliances |
| 2 | How does the desk summon a visitor, and what does the volunteer ask about the appliance? | By ticket or ticket number | How it fails or what it does when it fails |
| 3 | What does the technician check before powering the appliance, and what happens to the inspection if that check fails? | Damaged or unsafe wiring | Stops the inspection |
| 4 | After an unsafe wiring finding, what written item does the visitor receive and what can they do with the appliance? | Written explanation | Take it home or leave it for recycling |
| 5 | What does the technician do first after inspection passes, and what must happen before the desk adds a part price to the ticket? | Tries to reproduce the fault | Visitor approves or agrees to the charge |
| 6 | What happens after the repair, before either result is acted on? | Appliance is tested again | Visitor is shown the result |
| 7 | If the test fails and a part is removed, what gets recorded and what gets refunded? | Remaining fault | Price of the removed part |
| 8 | If the test passes, what document does the visitor get, and what two things does it list? | Receipt | Work and parts used |
| 9 | At closing, which two categories does the coordinator count or record? | Completed repairs | Appliances still awaiting parts |
| 10 | How does the coordinator use the closing record before the next session, and whom do they call? | Orders parts before the next session | Calls visitors whose repairs can resume |
| 11 | What figure does the library make public, and what information stays at the desk? | Monthly total of successful repairs | Names and ticket details |
| 12 | A visitor approves a paid part, then the repair fails and the part is removed. What happens to the charge at approval and after failure? | Price is added to the ticket after approval | Price of the part is refunded after failure |

<details>
<summary>Reader responses and scores</summary>

```json
[
  {
    "run": "R01",
    "version": "delacase",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances.",
      "It assigns a ticket so the technician can call the visitor without reading their name aloud; the volunteer asks what the appliance does when it fails.",
      "The technician checks for damaged wiring before plugging it in; if the wiring looks unsafe, the technician stops the inspection.",
      "The visitor receives a written explanation and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must agree/approve the part that costs money before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The technician records the remaining fault and refunds the price of any part removed from the appliance.",
      "The visitor gets a receipt listing the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "The coordinator uses the list to place an order before the next session and calls visitors whose repairs can resume.",
      "The library publishes a monthly total of successful repairs; names and ticket details stay at the desk.",
      "At approval, the part's price is added to the ticket (only after the visitor agrees); after the failure, the price of the removed part is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R02",
    "version": "plain",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances (repairs).",
      "The technician calls the visitor by ticket number, so the name isn't read aloud; the volunteer asks what the appliance does when it fails.",
      "The technician checks for damaged wiring; if it looks unsafe, the inspection stops.",
      "The visitor receives a written explanation and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must agree to (approve) the part's cost before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The technician records the remaining fault and the price of any part removed from the appliance is refunded.",
      "The visitor gets a receipt listing the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "The coordinator uses the list to place an order for parts before the next session and calls visitors whose repairs can resume.",
      "The library publishes a monthly total of successful repairs; names and ticket details stay at the desk.",
      "At approval, the part's price is added to the ticket (only after the visitor agrees); after the failed test and part removal, that price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R03",
    "version": "plain",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances (repairs).",
      "The technician calls the visitor by ticket number, without reading their name aloud; the volunteer asks what the appliance does when it fails.",
      "The technician checks for damaged wiring; if it looks unsafe, the inspection stops.",
      "The visitor receives a written explanation and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must approve the part that costs money before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The technician records the remaining fault and refunds the price of any part removed from the appliance.",
      "The visitor gets a receipt listing the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "They use the list to place an order before the next session and call visitors whose repairs can resume.",
      "The library publishes a monthly total of successful repairs; names and ticket details stay at the desk.",
      "The part's price is added to the ticket only after approval; if the repair fails and the part is removed, its price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R04",
    "version": "plain",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances (repairs).",
      "The technician calls the visitor by the assigned ticket (not by name); the volunteer asks what the appliance does when it fails.",
      "The technician checks for damaged wiring; if it looks unsafe, the inspection stops.",
      "The visitor receives a written explanation and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must approve the paid part before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The remaining fault is recorded and the price of any part removed from the appliance is refunded.",
      "The visitor gets a receipt listing the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "The coordinator uses the list to place an order for parts before the next session and calls visitors whose repairs can resume.",
      "The monthly total of successful repairs is published; names and ticket details stay at the desk.",
      "The price is added to the ticket only after approval; if the repair fails and the part is removed, that part's price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R05",
    "version": "delacase",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances.",
      "The volunteer assigns a ticket so the technician can call the visitor without reading their name aloud; the volunteer asks what the appliance does when it fails.",
      "The technician checks for damaged wiring before plugging it in; if the wiring looks unsafe, the technician stops the inspection.",
      "The visitor receives a written explanation and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must agree to (approve) the part that costs money before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The technician records the remaining fault and refunds the price of any part removed from the appliance.",
      "The visitor gets a receipt that lists the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "The coordinator uses the list to place an order before the next session and calls visitors whose repairs can resume.",
      "The library publishes a monthly total of successful repairs; names and ticket details stay at the desk.",
      "At approval, the part's price is added to the ticket; after the failed test and part removal, that price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R06",
    "version": "plain",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances.",
      "The volunteer assigns a ticket so the technician can call the visitor without reading their name aloud; the volunteer asks what the appliance does when it fails.",
      "The technician checks for damaged wiring before plugging it in; if the wiring looks unsafe, the inspection stops.",
      "The visitor receives a written explanation and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must approve the paid part before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The technician records the remaining fault and refunds the price of any part removed from the appliance.",
      "The visitor gets a receipt that lists the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "The coordinator uses the list to place an order before the next session and calls visitors whose repairs can resume.",
      "The library publishes a monthly total of successful repairs; names and ticket details stay at the desk.",
      "The passage says the part's price is added to the ticket only after the visitor agrees; after a failed test and part removal, that price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R07",
    "version": "delacase",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances.",
      "The volunteer assigns a ticket so the technician can call the visitor without reading their name aloud; the volunteer asks what the appliance does when it fails.",
      "The technician checks for damaged wiring before plugging it in; if the wiring looks unsafe, the technician stops the inspection.",
      "The visitor receives a written explanation and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must agree to/approve the part that costs money before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The technician records the remaining fault and refunds the price of any part removed from the appliance.",
      "The visitor receives a receipt that lists the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "The coordinator uses the list to place an order before the next session and calls visitors whose repairs can resume.",
      "The library publishes a monthly total of successful repairs; names and ticket details stay at the desk.",
      "At approval, the part's price is added to the ticket (only after the visitor agrees); after the failed test and part removal, that price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R08",
    "version": "mini",
    "score": 22,
    "omissions": [
      "6a",
      "11a"
    ],
    "answers": [
      "It opens on the first Saturday of each month and handles small appliances.",
      "It calls the visitor by ticket number; the volunteer asks how the appliance fails.",
      "The technician checks for damaged wiring; if it looks unsafe, the inspection is stopped.",
      "The visitor receives a written explanation and can take the appliance home or leave it for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must approve and agree to the paid part before its price is added to the ticket.",
      "The technician tests the appliance and shows the visitor the result.",
      "The technician records the fault and refunds the price of any part removed.",
      "The visitor gets a receipt, listing the work and the parts.",
      "The coordinator counts completed repairs and records appliances awaiting parts.",
      "The coordinator orders parts before the next session and calls visitors whose repairs can resume.",
      "The library publishes the monthly repair total; names and ticket details stay at the desk.",
      "At approval, the part's price is added to the ticket once the visitor agrees; after the failure and removal, the price of the removed part is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R09",
    "version": "mini",
    "score": 22,
    "omissions": [
      "6a",
      "11a"
    ],
    "answers": [
      "It opens on the first Saturday of each month and handles small appliances.",
      "It calls visitors by ticket number; the volunteer asks how the appliance fails.",
      "The technician checks for damaged wiring before plugging in; if the wiring looks unsafe, the technician stops the inspection.",
      "The visitor receives a written explanation and can take the appliance home or leave it for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must approve (agree to) the paid part before its price is added to the ticket.",
      "The technician tests the appliance and shows the visitor the result.",
      "The technician records the fault and refunds the price of any part they remove.",
      "The visitor gets a receipt listing the work and the parts.",
      "The coordinator counts completed repairs and records appliances awaiting parts.",
      "The coordinator orders parts before the next session and calls visitors whose repairs can resume.",
      "The library publishes the monthly repair total; names and ticket details stay at the desk.",
      "The part's price is added to the ticket once the visitor agrees; after the failure and part removal, the price of that part is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R10",
    "version": "plain",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances.",
      "The volunteer assigns a ticket so the technician can call the visitor without reading their name aloud; the volunteer asks what the appliance does when it fails.",
      "The technician checks for damaged wiring before plugging it in; if the wiring looks unsafe, the inspection stops.",
      "The visitor receives a written explanation and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must approve the part that costs money before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The technician records the remaining fault and refunds the price of any part removed from the appliance.",
      "The visitor gets a receipt that lists the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "The coordinator uses the list to place an order before the next session and calls visitors whose repairs can resume.",
      "The library publishes a monthly total of successful repairs; names and ticket details stay at the desk.",
      "At approval, the part's price is added to the ticket; after the failure, the part is removed and its price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R11",
    "version": "mini",
    "score": 22,
    "omissions": [
      "6a",
      "11a"
    ],
    "answers": [
      "It opens on the first Saturday of each month and handles small appliances.",
      "It calls the visitor by ticket number; the volunteer asks how the appliance fails.",
      "The technician checks for damaged wiring; if it looks unsafe, the inspection stops.",
      "The visitor gets a written explanation and can take the appliance home or leave it for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must approve/agree to the paid part before its price is added.",
      "The technician tests the appliance and shows the visitor the result.",
      "The technician records the fault and refunds the price of any part removed.",
      "The visitor gets a receipt listing the work and parts.",
      "The coordinator counts completed repairs and records appliances awaiting parts.",
      "The coordinator orders parts before the next session and calls visitors whose repairs can resume.",
      "The library publishes the monthly repair total; names and ticket details stay at the desk.",
      "The part's price is added to the ticket upon approval; after the failure and removal of the part, that price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R12",
    "version": "mini",
    "score": 22,
    "omissions": [
      "6a",
      "11a"
    ],
    "answers": [
      "It opens on the first Saturday of each month and handles small appliances.",
      "It calls the visitor by ticket number; the volunteer asks how the appliance fails.",
      "The technician checks for damaged wiring before plugging in; if the wiring looks unsafe, the inspection stops.",
      "The visitor receives a written explanation and can take the appliance home or leave it for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must approve and agree to any paid parts before the price is added to the ticket.",
      "The technician tests the appliance and shows the visitor the result.",
      "The technician records the fault and refunds the price of any part removed.",
      "The visitor gets a receipt listing the work and the parts.",
      "The coordinator counts completed repairs and records appliances awaiting parts.",
      "The coordinator orders parts before the next session and calls visitors whose repairs can resume.",
      "The library publishes the monthly repair total; names and ticket details stay at the desk.",
      "The part's price is added to the ticket once the visitor agrees; after the failed test and part removal, that price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R13",
    "version": "delacase",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances (repairs to keep them working).",
      "The volunteer assigns a ticket so the technician can call the visitor without reading their name aloud; the volunteer asks what the appliance does when it fails.",
      "The technician checks the appliance for damaged wiring before plugging it in; if the wiring looks unsafe, the technician stops the inspection.",
      "The visitor receives a written explanation, and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must agree to (approve) the part that costs money before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The technician records the remaining fault and the price of any part removed from the appliance is refunded.",
      "The visitor receives a receipt that lists the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "The coordinator uses the list to place an order before the next session and calls visitors whose repairs can resume.",
      "The library publishes a monthly total of successful repairs; names and ticket details stay at the desk.",
      "At approval, the part's price is added to the ticket (only after the visitor agrees); after the failure and removal, the price of that part is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R14",
    "version": "mini",
    "score": 22,
    "omissions": [
      "6a",
      "11a"
    ],
    "answers": [
      "It opens on the first Saturday of each month and handles small appliances.",
      "It calls the visitor by ticket number; the volunteer asks how the appliance fails.",
      "The technician checks for damaged wiring before plugging in; if the wiring looks unsafe, the inspection is stopped.",
      "The visitor gets a written explanation and can take the appliance home or leave it for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must agree to/approve the paid part before its price is added to the ticket.",
      "The technician tests the appliance and shows the visitor the result.",
      "The technician records the fault and the price of any removed part is refunded.",
      "The visitor gets a receipt listing the work and the parts.",
      "The coordinator counts completed repairs and records appliances awaiting parts.",
      "The coordinator orders parts before the next session and calls visitors whose repairs can resume.",
      "The library publishes the monthly repair total; names and ticket details stay at the desk.",
      "The part's price is added to the ticket when the visitor approves it; if the repair fails and the part is removed, that price is refunded."
    ],
    "clarify": []
  },
  {
    "run": "R15",
    "version": "delacase",
    "score": 24,
    "omissions": [],
    "answers": [
      "It opens on the first Saturday of every month and handles small appliances.",
      "It assigns a ticket so the technician can call the visitor without reading their name aloud; the volunteer asks what the appliance does when it fails.",
      "The technician checks for damaged wiring before plugging it in; if the wiring looks unsafe, the technician stops the inspection.",
      "The visitor receives a written explanation and can take the appliance home or leave it with the desk for recycling.",
      "The technician tries to reproduce the fault while the visitor watches; the visitor must agree/approve the part that costs money before its price is added to the ticket.",
      "The technician tests the appliance again and shows the visitor the result.",
      "The technician records the remaining fault and refunds the price of any part removed from the appliance.",
      "The visitor gets a receipt listing the work and the parts used.",
      "The coordinator counts completed repairs and records appliances still awaiting parts.",
      "The coordinator uses the list to place an order before the next session and calls visitors whose repairs can resume.",
      "The library publishes a monthly total of successful repairs; names and ticket details stay at the desk.",
      "The passage says the part's price is added to the ticket only after the visitor agrees; after failure and removal, the price of the part is refunded. It does not say more about how the charge is handled at approval."
    ],
    "clarify": []
  }
]
```

</details>

## Human lookup pilot

A recall exercise with two passages was replaced with one lookup at a time.
The reader kept each passage visible and answered a question about where
an item went under an exception in the procedure.

One reader tried the marked passage first and the plain passage second.
The marked passage felt easier to scan, although the answer preserved the
color and substituted a different noun for the container. The plain
passage required more attention by self-report and the answer matched
both details. The reader reported that capitals helped them jump between
details on rereading.

The passages used different wording and the order was fixed. Search time
was unmeasured. A follow-up can use both capitalization variants of each
passage, reverse the order across readers, and record lookup time and
answer accuracy separately.

A four-round chat run used the order DeLaCase, plain, plain, DeLaCase
with a fresh passage and one multiple-choice lookup per round. All four
answers were correct. The intervals between chat timestamps were 27 s
and 14 s for DeLaCase and 23 s and 22 s for plain. These intervals include
turn overhead, so they do not isolate lookup time. The timed comparison
with reversed assignments across readers remains open.
