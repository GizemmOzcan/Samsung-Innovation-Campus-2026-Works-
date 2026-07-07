# [Lab 2] Designing Specific Tasks with Structured Prompts
## Lab 2-1: Step-by-Step Prompt Element Practice

**Mission:** Build a Professional Health Coach to explore how a vague request transforms into a high-quality, actionable AI response.

For this one I used the Professional Health Coach example and just kept adding one element at a time to see where the response actually got better.

**Step 0 — no elements:**
```
Give me advice on healthy living.
```
It said the usual stuff — eat balanced meals, drink water, exercise regularly, sleep enough, avoid processed food. Nothing wrong with it, but it's the kind of answer you could paste under literally anyone's name.

**Step 1 — + Role:**
```
You are a certified health and fitness coach.
Give me advice on healthy living.
```
It talked a bit more like a coach this time, mentioned "three pillars" (nutrition, movement, rest) and gave a rough number like 150 minutes of activity a week — but it was still speaking to nobody in particular.

**Step 2 — + Goal:**
```
...Suggest a weekly nutrition and exercise plan for someone who wants to lose weight.
```
This time it actually gave a small plan — calorie deficit, 4 days of cardio, 2 days of weights, portion control — but again, generic, could apply to anyone trying to lose weight.

**Step 3 — + Context:**
```
...Person: 28 years old, office worker, sits at a desk 8 hours/day,
only has 3 days a week with 30 minutes free, no gym access.
```
This is where it actually changed. It dropped the gym-based suggestions entirely and gave home exercises (squats, planks, lunges), plus a small tip about standing up every hour because of the desk job. It felt like it was finally responding to an actual person instead of "someone" in general.

**Step 4 — + Constraints:**
```
...No equipment, vegetarian meals only, don't make exaggerated health claims.
```
It gave an equipment-free warm-up/workout/cool-down routine and swapped meat suggestions for lentils, chickpeas, eggs, yogurt. It also added a line saying results vary and it can't promise a specific timeline — which is exactly what I asked for.

**Step 5 — + Style:**
```
...Tone: motivating but realistic, no jargon, talk like a supportive friend.
```
The wording changed noticeably here — it opened with something like "I know desk jobs are exhausting, but small changes add up," which felt a lot less clinical than before.

**Step 6 — + Output Format:**
```
...Format: General Approach / Weekly Exercise Plan (table) / Nutrition Tips (max 5 bullets)
```
It organized everything into a short intro, a 3-day table (day/exercise/duration), and 5 nutrition bullet points — genuinely something you could screenshot and follow.

### Discussion Point Cevapları

1. **Kalitenin en çok değiştiği step:** Context eklendiği step, çünkü ondan önceki her cevap teknik olarak doğru ama kişisiz — herkese uyardı. Context girince tavsiye gerçek bir insanın programına ve kısıtlarına göre şekillenmeye başladı.
2. **En "must-have" iki element:** Context ve Constraints — Context olmadan cevap kişiselleşmiyor, Constraints olmadan da güvenli/gerçekçi olmuyor (mesela abartılı sağlık iddiaları gibi).
3. **İlk tune edilecek element:** Eğer cevap yine "herkese uyar" gibi hissettirirse ilk Context'i genişletirim, çünkü buradaki en büyük kalite sıçraması oradan geldi.

> **Activity:** *"I felt the biggest 'quality' jump at Step [Context], because that's the point where the advice stopped being generic and started fitting an actual person's schedule and limitations — everything before it could've applied to anyone, and everything after it was more about polishing than transforming. Also, the element I will most often use is [Constraints], because that's what stops the AI from giving advice that sounds nice but isn't actually realistic or safe to follow."*

---

## Lab 2-2: Creating My Own AI Study Coach with Prompt Engineering

**Mission:** Design an AI Study Coach that generates a realistic, adherence-focused study plan based on a scenario.

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

When I actually ran this, it came back with something reasonable: first two days just light review reading, then it slowly worked up to harder topics with practice problems, told me to keep the phone in another room while studying, and added a small rule saying if a day gets missed, don't try to "make up" for it the next day — just continue as planned, since trying to catch up is usually what makes people quit entirely.

### Discussion Point Cevapları

1. **En kritik constraint:** İlk birkaç günü kasıtlı olarak kolay tutmak. Çünkü insanların planı bırakma sebebi genelde ilk günden fazla yüklenip tükenmek — yumuşak bir başlangıç, zorluk artmadan önce planın alışkanlığa dönüşme şansını veriyor.
2. **Neden "sticking to it" sorunu content sorunu değil:** Öğrenci konuyu zaten anlıyor, sorun bilgi eksikliği değil, sürdürülebilirlik. O yüzden prompt'ta "adherence, not just content" ifadesini bilinçli olarak öne çıkardım.

> **Activity:** *"I think adding a deliberately easy first few days is key to making this plan realistic because it directly addresses the 'hard to stick to plans' problem — most people quit not from lack of knowledge, but from going too hard too soon and burning out."*

---

## Lab 2-3: Structured Prompt Design

**Mission:** Crafting the Master Template by organizing study coach instructions into a clear, hierarchical structure that the AI can parse with high accuracy.

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

### Discussion Point Cevapları

1. **SYSTEM/USER ayrımının mantığı:** SYSTEM kısmı hangi öğrenci/konu olursa olsun asla değişmeyen koçluk kurallarını taşıyor; USER kısmı ise her seferinde doldurulan değişken bilgi. Bu ayrım sayesinde template'i başka bir ders/öğrenci için tekrar yazmadan yeniden kullanabiliyorum.
2. **Adherence Mechanism'ın "required" olması:** Bunu opsiyonel bırakmadım, çünkü AI kısa tutmaya çalıştığında ilk atacağı kısım genelde "ekstra" gibi gördüğü bölümler oluyor — bunu format'a sabitleyerek her seferinde çıkmasını garantiledim.
3. **Output Format'ı neden kilitledim:** Serbest metin bırakırsam AI, cevabı kısaltmak isterse adherence/risk kısımlarını sessizce atlayabilir ya da özetleyebilir — nitekim Lab 2-4'te tam olarak bu problemi yaşadım.

> **Activity:** *"I intentionally designed the [Adherence Mechanism] section to be a required part of the output, not optional, because I wanted to force the AI to always propose a concrete way to stick to the plan, not just list what to study. The reason I chose to lock in the [Output Format] was to prevent the AI from quietly dropping or shortening the adherence and risk sections whenever it tried to keep the response brief."*

---

## Lab 2-4: Model Execution and 1st Result Check

**Mission:** Test your structured prompt and evaluate the AI's response based on the "Reality Rules" you defined in the template.

Filled in the template and ran it:

```
Subject: Data Structures
Duration: 3 weeks
Daily time available: 2 hours
Student situation: understands the material but hasn't reinforced it, has quit study
plans by day 2 before, biggest distraction is phone/social media, tends to procrastinate.
```

<img width="465" height="571" alt="Ekran görüntüsü 2026-07-08 002232" src="https://github.com/user-attachments/assets/f9102032-478b-4239-a549-551dd19d47cf" />


It came back with something that was actually pretty solid for the first week — day by day, starting light (just reading/notes for the first two days: Arrays, then Linked Lists), then gradually adding practice problems as it moved into Strings, Linked Lists, and Stacks. For the adherence part, it said to keep the phone in another room while studying and to anchor the study session to the same daily trigger (e.g. right after dinner).

Where it fell apart was weeks 2 and 3 — instead of writing them out day by day like week 1, it collapsed them into single lumped lines, literally "Days 8-14 cover Queues, Recursion, Hash Tables, and Trees, building from theory into applied problem-solving..." instead of 7 separate rows. Same thing for week 3 — Graphs, Heaps, Sorting, Searching all folded into one paragraph instead of individual days. So week 1 was genuinely copy-paste ready, but weeks 2-3 weren't at that same level of granularity.

Everything else held up against what I was checking for — it stayed under 2 hours a day, never put more than one topic on a single day, and didn't make any promises about grades or results. So really the one issue was the formatting drop-off after week 1, exactly the kind of thing that happens when the format instruction is implied ("table") rather than explicit ("every day, no grouping").

### Discussion Point Cevapları

1. **Reality Rules'e göre nerede tuttu, nerede tutmadı:** Zaman limiti, tek konu/gün ve "söz vermeme" kuralları tamamen tutuldu. Tutmayan tek şey format derinliğiydi — hafta 1 günlük detayda, hafta 2-3 özet halinde geldi.
2. **En "must-have" iki kural:** Daily time limit ve "no catch-up after a missed day" — ikisi de doğrudan "sürdürülemez plan" riskini engelliyor.
3. **İlk tune edilecek yer:** Output Format'ı daha da katılaştırırım — "her gün ayrı satır" gibi belirsizlik bırakmayan bir kural eklerim.

> **Activity Reflection:** *"Initially, the AI gave a detailed day-by-day breakdown for Week 1 but summarized Weeks 2-3 into single lines instead of individual days, so I plan to refine the [Output Format] by explicitly requiring every single day to have its own row, to improve consistency across all three weeks."*

---

## Lab 2-5: Prompt 1st Tuning and Re-execution

**Mission:** Refine your template based on the evaluation from Lab 2-4 to achieve the highest level of adherence and realism in the study plan.

Since the only real problem was weeks 2-3 getting summarized instead of broken out day by day, I went back and made that explicit in the output format instead of leaving it implied:

```
[OUTPUT FORMAT — tuned]
- Weekly Plan: ONE table covering all 21 days individually, Day 1 through Day 21.
  No grouping, no summarizing multiple days into one row — every single day gets its own row.
```

<img width="473" height="692" alt="Ekran görüntüsü 2026-07-08 002259" src="https://github.com/user-attachments/assets/04c2bea8-821f-4f3a-aa43-9974ab5ad7c2" />
<img width="490" height="367" alt="image" src="https://github.com/user-attachments/assets/7aade867-b81f-4358-b272-bb9760f22d67" />



Running it again with that one change fixed it — this time it actually wrote out all 21 days individually at the same level of detail: week 1 stayed essentially the same (arrays, strings, linked lists, stacks), week 2 broke Queues, Recursion, Hash Tables, and Trees into their own separate days instead of one lumped paragraph, and week 3 did the same for Tree traversals, Heaps, Graphs, Sorting, and Searching, ending with a full mock test on day 21. The adherence and risk parts didn't really change content-wise — phone-in-another-room, a fixed daily trigger, and a "no-zero rule" (10-minute minimum on bad days) — since formatting granularity, not the coaching rules, was the actual problem.

So it only took one targeted change to the output format — I didn't have to touch any of the actual coaching rules or constraints, just how strictly the format was spelled out.

### Discussion Point Cevapları

1. **Design intent vs. actual performance gap'i:** Ben zaten format'ta "haftalık tablo" istemiştim ama "her gün ayrı satır" demedim — AI bunu "haftalık özet de olur" diye yorumlamış. Gap, benim zımni varsaydığım şeyi açıkça yazmamamdan kaynaklandı.
2. **Neden sadece format değişti, kurallar değişmedi:** Çünkü sorun kuralların ihlali değildi (hiçbiri çiğnenmedi), sorun sadece çıktının granülaritesiydi — yani cerrahi bir düzeltme yeterliydi.

> **Activity Reflection:** *"By adjusting the [Output Format] to require one row per individual day with no grouping allowed, I was able to solve the problem of Weeks 2-3 being summarized instead of broken out, resulting in a plan that is fully copy-paste ready across all 21 days, not just the first week."*

---

## Overall Takeaways

Doing this step by step, the two things that stood out most were: **context makes the biggest difference** between a generic and a useful response, and **if you actually care about something being in the output** (like the adherence section, or day-by-day granularity), **you have to make the format explicit** — otherwise it's the first thing that gets cut or summarized when the AI is trying to keep the response shorter.
