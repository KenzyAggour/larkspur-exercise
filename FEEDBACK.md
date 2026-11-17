# Client review: Larkspur disruption-care agent

**To:** Room 9  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-17 11:15

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py get_flight_status: your edit says YYYY-MM-DD while the input_schema date field still says "MM/DD/YYYY".**

Your diff appended "Make sure date is in the format YYYY-MM-DD." to the description of get_flight_status. The date property inside the same schema keeps its description "MM/DD/YYYY". Claude is handed both formats on every turn for that one tool. Nothing in the repository shows which one the OpsFeed backend accepts.

Run python3 run.py K7PQ2M --trace and paste the date string that appears in the get_flight_status call and what came back.

**2. agent.py search_alternatives: your new description says it is "Called after check policy", and the trace you committed never calls it.**

readout-trace.json shows 4 API turns and 3 tool calls in order: lookup_booking, get_flight_status, check_policy. search_alternatives, hold_seat and care_entitlements do not appear. PITCH.md says the agent "finds alternative flights" and "places temporary seat holds", and the committed run contains neither.

Run python3 run.py --all --trace and paste the totals footer, so the tool list per PNR is on the page.

**3. PITCH.md Number: 343 tokens per turn from 2,754 to 3,097 has no bench or tool-tax output behind it in this repository.**

The only run evidence committed is readout-trace.json: 12924 tokens in over 4 turns, with 0 cache read and 0 written. agent.py has no cache_control anywhere. The schema tax you quote is paid on every one of those turns, and care_entitlements (374 characters) is now one of ten tools sent each time.

Run python3 run.py --tool-tax and paste its output next to the 2,754 and 3,097 figures.

**4. agent.py care_entitlements() wraps check_policy and returns a subset, and no run shows the model choosing it over check_policy.**

The tool's description reads "retrieves the booking and applies Larkspur's policy", which is what the shipped check_policy description also covers. PITCH.md says care_entitlements "was not selected during the initial Build 2.1 probe" and that routing "required further description and probe improvements". The committed trace calls check_policy and never care_entitlements. A larger model would face the same two overlapping descriptions, so the model name does not settle which tool gets picked.

Run python3 run.py --show-tools and paste the care_entitlements and check_policy entries, then run python3 verify.py 2.1 and paste the result.

**5. No evals/cases.json exists, and verify.py, eval_harness.py, bench.py and readout.py differ from the shipped pack.**

The review shows banked gates 1.2, 1.3, 1.4, 2.1 and 2.2 and no eval cases. A change to a verifier or harness changes what any number from it means. PITCH.md leaves Lever as "<cost | speed | intelligence>" and the four answers under Priya asked blank, so no before/after exists yet. Until bench files exist, Priya cannot get a measured answer on cost, speed or accuracy.

Run python3 bench.py --label before --stage 1 --runs 3 and python3 bench.py --label after --stage 1 --runs 3, then python3 bench.py --compare before after, and paste the compare output.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (296 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
