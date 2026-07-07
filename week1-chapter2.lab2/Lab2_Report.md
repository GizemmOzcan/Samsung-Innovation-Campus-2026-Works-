# Lab 2

## Lab 2-1

For this one I used the Professional Health Coach example and just kept adding one element at a time to see where the response actually got better.

Starting point was just:
```
Give me advice on healthy living.
```
It said the usual stuff — eat balanced meals, drink water, exercise regularly, sleep enough, avoid processed food. Nothing wrong with it, but it's the kind of answer you could paste under literally anyone's name.

Then I added a role:
```
You are a certified health and fitness coach.
Give me advice on healthy living.
```
It talked a bit more like a coach this time, mentioned "three pillars" (nutrition, movement, rest) and gave a rough number like 150 minutes of activity a week — but it was still speaking to nobody in particular.

Next I added a goal:
```
...Suggest a weekly nutrition and exercise plan for someone who wants to lose weight.
```
This time it actually gave a small plan — calorie deficit, 4 days of cardio, 2 days of weights, portion control — but again, generic, could apply to anyone trying to lose weight.

Then context:
```
...Person: 28 years old, office worker, sits at a desk 8 hours/day,
only has 3 days a week with 30 minutes free, no gym access.
```
This is where it actually changed. It dropped the gym-based suggestions entirely and gave home exercises (squats, planks, lunges), plus a small tip about standing up every hour because of the desk job. It felt like it was finally responding to an actual person instead of "someone" in general.

Then constraints:
```
...No equipment, vegetarian meals only, don't make exaggerated health claims.
```
It gave an equipment-free warm-up/workout/cool-down routine and swapped meat suggestions for lentils, chickpeas, eggs, yogurt. It also added a line saying results vary and it can't promise a specific timeline — which is exactly what I asked for.

Then style:
```
...Tone: motivating but realistic, no jargon, talk like a supportive friend.
```
The wording changed noticeably here — it opened with something like "I know desk jobs are exhausting, but small changes add up," which felt a lot less clinical than before.

Finally output format:
```
...Format: General Approach / Weekly Exercise Plan (table) / Nutrition Tips (max 5 bullets)
```
It organized everything into a short intro, a 3-day table (day/exercise/duration), and 5 nutrition bullet points — genuinely something you could screenshot and follow.

If I had to pick one takeaway: I felt the biggest quality jump at the Context step, because that's the point where the advice stopped being generic and started fitting an actual person's schedule and limitations — everything before it could've applied to anyone, and everything after it was more about polishing than transforming. Also, the element I'll probably use most often going forward is Constraints, because that's what stops the AI from giving advice that sounds nice but isn't actually realistic or safe to follow (like the "don't overpromise" rule here).

## Lab 2-2

The scenario I used was a student who has a Data Structures final in 3 weeks, understands the material but can't stick to a study plan for more than 2 days, only has 2 hours a day free, and gets distracted by their phone constantly.

The thing is, if you just ask an AI "make me a study plan for this exam," it'll give you something that looks fine on paper — Week 1: this topic, Week 2: that topic — but it completely ignores the actual problem, which isn't *what* to study, it's *sticking to it*. So I wrote the prompt specifically to force the AI to think about that:

```
You are a Study Coach who cares about adherence, not just content. Build a 3-week plan for
someone who understands the material but has quit study plans by day 2 before, has only
2 hours/day, and gets distracted by their phone.

Rules: never exceed 2 hours/day, one topic per day max, make the first few days easy on
purpose, give one concrete rule for handling phone distraction, no generic pep-talk lines,
don't promise results you can't guarantee.
```

Out of all the constraints, I think the one that mattered most was making the first few days deliberately easy. That's the part that actually addresses *why* people quit — they usually go too hard in the first day or two, burn out, and then stop. A soft start is what gives the plan a chance to become a habit before the difficulty ramps up.

When I actually ran this, it came back with something reasonable: first two days just light review reading, then it slowly worked up to harder topics with practice problems, told me to keep the phone in another room while studying, and added a small rule saying if a day gets missed, don't try to "make up" for it the next day — just continue as planned, since trying to catch up is usually what makes people quit entirely.

## Lab 2-3

This step was about turning the Lab 2-2 prompt into something reusable — separating what never changes (the coaching rules) from what changes every time (subject, hours, student's situation). So instead of one big paragraph, I split it into a fixed "system" part and a small "fill in the blanks" part:

```
[SYSTEM — fixed, never changes]
You are a Study Coach who cares about adherence, not just content.
Always follow these rules:
- never exceed the daily time limit given
- one main topic per day, no overloading
- make the first 2-3 days deliberately easy, to build the habit before it gets harder
- include at least one concrete distraction-management rule (not just "be disciplined")
- no generic pep-talk lines
- no unrealistic promises (guaranteed grades, guaranteed results, etc.)
- tone: warm but direct, not preachy

[USER — fill in every time]
Subject: {subject}
Duration: {duration}
Daily time available: {daily_hours}
Student situation: {student_profile}

Build a study plan based on this.

[OUTPUT FORMAT — fixed]
- General Strategy (2-3 sentences)
- Weekly Plan (table: Day / Topic / Duration / Focus Method)
- Adherence Mechanism (2-3 concrete rules)
- Risk Warning (where drop-off is likely + what to do)
```

I intentionally designed the Adherence Mechanism section to be a required part of the output, not optional, because I wanted to force the AI to always propose a concrete way to stick to the plan, not just list what to study. The reason I chose to lock in the Output Format (rather than leave it as free text) was to prevent the AI from quietly dropping or shortening the adherence and risk sections whenever it tried to keep the response brief — which is exactly the failure I ran into later in Lab 2-4.

## Lab 2-4

Now I actually filled in the template and ran it:

```
Subject: Data Structures
Duration: 3 weeks
Daily time available: 2 hours
Student situation: understands the material but hasn't reinforced it, has quit study
plans by day 2 before, biggest distraction is phone/social media, tends to procrastinate.
```

It came back with something that was actually pretty solid for the first week — day by day, starting light (just reading/notes for the first two days: Arrays, then Linked Lists), then gradually adding practice problems as it moved into Stacks/Queues and Trees. For the adherence part, it said to keep the phone in another room while studying, and — the part I liked most — if a day gets missed, don't try to "catch up" the next day, just continue the plan as normal, because trying to catch up is usually what makes people quit for good.

Where it fell apart a bit was weeks 2 and 3 — instead of writing them out day by day like week 1, it kind of lumped them together, something like "Week 2: Graphs, Hashing, Sorting" all in one line instead of 7 separate days. So week 1 was genuinely copy-paste ready, but weeks 2-3 weren't at that same level yet.

Everything else held up against what I was checking for — it stayed under 2 hours a day, never put more than one topic on a single day, and didn't make any promises about grades or results. So really the one issue was that formatting drop-off after week 1.

## Lab 2-5

Since the only real problem was weeks 2-3 getting summarized instead of broken out day by day, I went back and made that explicit in the output format instead of leaving it implied:

```
[OUTPUT FORMAT — tuned]
- Weekly Plan: ONE table covering all 21 days individually, Day 1 through Day 21.
  No grouping, no summarizing multiple days into one row — every single day gets its own row.
```

Running it again with that one change fixed it — this time it actually wrote out all three weeks day by day at the same level of detail: week 1 stayed the same (arrays, linked lists, stacks/queues, trees), week 2 became Graphs → Hashing → Sorting broken across separate days instead of one lumped line, and week 3 turned into two full mock exams with dedicated error-review days in between, plus a light review on the last day before the actual final. The adherence and risk parts didn't really change since they weren't the problem to begin with.

So it only took one targeted change to the output format — I didn't have to touch any of the actual coaching rules or constraints, just how strictly the format was spelled out.

## Overall

Doing this step by step, the two things that stood out most were: context makes the biggest difference between a generic and a useful response, and if you actually care about something being in the output (like the adherence section here), you have to make the format explicit — otherwise it's the first thing that gets cut or summarized when the AI is trying to keep the response shorter.
