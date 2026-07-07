# Lab 4

## Lab 4-1

The scenario I picked: planning a 3-day Istanbul itinerary for a tourist with a **150 dollar total budget** for paid activities (not counting food or hotel), who likes history but hates crowded/touristy spots, and doesn't want to zigzag across the city every day.

The reason a single vague instruction would fail here is that this task actually has several hidden sub-steps buried inside it — picking sites that match the "likes history, hates crowds" taste, checking that the total cost doesn't go over $150, and then grouping the chosen sites by location so the person isn't crossing the city back and forth. If you just ask an AI "plan me a 3-day Istanbul trip," it usually nails the first part (picking nice places) but quietly skips or gets sloppy on the budget math and the geographic grouping, since those aren't things you'd naturally think to check unless you're told to.

So: in this scenario, the most important element to control is the order of operations, because if the AI picks the sites before checking the budget and location clustering, it ends up cementing a list of places first and then trying to force them into a route/budget afterward, which is how you get itineraries that go over budget or bounce all over the map. Because of that, my goal in one sentence is: to solve this task perfectly, the AI must first filter candidate sites by interest and crowd-level, then filter by budget, and finally group the survivors by location before assigning them to specific days.

## Lab 4-2

**Prompt A — general, single block:**
```
Plan a 3-day Istanbul itinerary for a tourist. Budget for activities is $150 total
(not including food or hotel). They like history but dislike crowded/touristy places,
and prefer not to travel back and forth across the city each day. Give me a day-by-day
plan.
```

It said (Prompt A output, summarized): Day 1 — Hagia Sophia, Blue Mosque, Basilica Cistern (all in Sultanahmet, fine so far); Day 2 — Topkapi Palace, then jumped to Dolmabahçe Palace across the Bosphorus, then back to the Grand Bazaar in Sultanahmet again; Day 3 — Chora Church, then Galata Tower, then back to Sultanahmet for the Spice Bazaar. It listed rough prices for each site but never added them up, and it included the Blue Mosque and Hagia Sophia which are usually two of the most crowded sites in the whole city — exactly what the person said they wanted to avoid.

So: Prompt A produced a result that felt scattered/inconsistent, because the AI missed the specific constraint of avoiding crowded sites (it put Hagia Sophia and the Blue Mosque front and center on Day 1 anyway) and also missed doing the budget math, so I couldn't actually tell if it stayed under $150 without doing it myself.

## Lab 4-3

**Prompt B — step-by-step:**
```
Plan a 3-day Istanbul itinerary for a tourist. Follow these steps in order:

Step 1: List 8-10 candidate historical sites in Istanbul that are known for being
relatively low-crowd or that can be visited early/late to avoid crowds. Skip the
most overcrowded landmarks unless there's no good low-crowd alternative.

Step 2: Look up an approximate ticket price for each candidate, then remove any
combination that would push the 3-day total activity cost over $150.

Step 3: Group the remaining sites by neighborhood/location so that everything
visited on the same day is within easy walking or short-transit distance of
each other.

Step 4: Only now, assign the grouped sites to Day 1, Day 2, and Day 3, and
give me the final plan with prices and a running budget total.
```

It said (Prompt B output, summarized): Step 1 gave a list that skipped Hagia Sophia/Blue Mosque and instead suggested things like the Chora Church, Rüstem Pasha Mosque, Süleymaniye Mosque, Pierre Loti Hill, and the Istanbul Archaeology Museums — all history-focused but noticeably less crowded. Step 2 dropped a couple of pricier options to stay under budget and showed a running total ending at $138. Step 3 grouped them by neighborhood (Fatih/Süleymaniye cluster, Eyüp/Pierre Loti cluster, Sultanahmet museums cluster). Step 4 turned that into a clean 3-day plan where each day only involves one neighborhood cluster, no backtracking across the city, and the total cost was shown clearly at the bottom.

So: the transition from Prompt B (step-by-step) was most effective in forcing a budget check and a location-grouping pass to actually happen instead of being skipped, because the AI finally understood the ordering I wanted — filter first, group second, schedule last — instead of jumping straight to "here's a nice sounding itinerary."

## Lab 4-4

Putting A and B side by side, the biggest difference was that Prompt A treated this like a single "list good places" task, while Prompt B treated it like a pipeline with checkpoints. While Prompt A felt like a highlight reel of famous spots with the constraints mentioned but not actually enforced, Prompt B provided a genuinely usable, budget-and-route-checked solution because it forced the AI to filter for crowd-level first, verify the budget second, and only build the day-by-day route after both of those were already satisfied — instead of picking pretty places first and hoping the constraints would somehow still hold.

The "control gap" here really comes down to this: a single instruction lets the AI satisfy the constraints it finds easiest (picking famous sites) while quietly deprioritizing the ones that take more effort to verify (adding up prices, checking a map). Splitting it into steps takes away that shortcut, because each step has to be finished before the next one starts.

## Lab 4-5

Reflecting on when each one is actually worth using:

I will use Prompt A (general) when the task is simple enough that there's basically one obvious way to satisfy all the constraints at once — nothing to calculate, nothing to cross-check — and Prompt B (step-by-step) when the task has hidden sub-steps that compete with each other, like budget vs. preference vs. geography here, where doing them in the wrong order quietly breaks one of the requirements, to ensure my AI outcomes are always something I can trust without having to double-check the math or the constraints myself afterward.
