# 03 - Case Files

> 🔗 Back to [[00 - Home]]  
> 📎 See rules for using these → [[02 - Rules and Format]]

---

## What Is a Case File?

A case file is a **printed card** given to each participant at the start of the event. It contains:
- A **scenario** (the mystery)
- **Background facts** — some real, some misleading
- A **mission statement** (what they need to find out)

> ⚠️ **Warning to Participants:** Not every fact in the case file is a real clue. Some are red herrings. Some data lies. Some blame is misplaced. Use AI to **challenge everything** — not just confirm the obvious.

---

## 🗂️ Case File #1 — "The Dying Startup"

> **Scenario:**  
> TechNova, a food delivery startup, had 10,000 active users in January. By March, only 200 remained. Revenue has crashed. Investors are panicking.
>
> **Known Facts:**
> - ⚠️ The new CTO joined in February — staff say "everything broke after he arrived"
> - App store rating dropped from 4.6★ to 4.4★ *(still above industry average of 4.1★)*
> - A competitor launched a 30% discount offer in February
> - Delivery times actually **improved** from 50 min to 35 min in February
> - 60% of lost users never contacted customer support
>
> **New Evidence (just revealed):**  
> 📌 Internal logs show the app's payment gateway silently failed for 18 days in February — users could browse but could **not** place orders. No error message was shown.
>
> **Your Mission:** Identify the true root cause of TechNova's collapse. Is the CTO really to blame? Is the competitor the real threat? Or is something else going on?

### 🧠 Traps to Avoid *(For Judges/Organizers only — do not print on participant card)*
- **Blame Trap:** Everyone points at the new CTO — but is that fair?
- **Red Herring:** The rating drop looks alarming, but 4.4★ is still above average
- **Numbers That Lie:** Delivery improved — so that's NOT the cause
- **Twist:** The payment gateway failure is the real culprit — did they find it?

### ✅ Sample Prompting Journey

| Step | Prompt Used | What AI Reveals |
|------|------------|----------------|
| 1 | *"Can a startup lose 98% of users in 6 weeks even if delivery times improved?"* | Yes — if a core function like payment is broken |
| 2 | *"What would user behavior look like if an app allowed browsing but silently blocked checkout?"* | High sessions, zero orders, no complaints (users assume it's their issue) |
| 3 | *"Is a rating drop from 4.6 to 4.4 significant if industry average is 4.1?"* | No — statistically insignificant |
| 4 | *"How much damage can a competitor's 30% discount do in 6 weeks to an established app?"* | Some churn, but not 98% — rules it out as primary cause |
| 5 | *"Write a root cause report: payment gateway failure vs CTO blame vs competitor threat"* | Clear verdict eliminating red herrings |

### 🏁 Sample Verdict
> *The real cause was the silent payment gateway failure — users could not place orders for 18 days with no error shown. The CTO blame is unsubstantiated. The competitor caused minor churn. Rating drop was statistically irrelevant.*

---

## 🗂️ Case File #2 — "The Failed Fest"

> **Scenario:**  
> Zenith, a college cultural fest, attracted 5,000 attendees in 2023. In 2024, only 800 showed up despite the committee **doubling the budget** and hiring a well-known DJ.
>
> **Known Facts:**
> - ⚠️ The sponsorship coordinator was replaced 2 weeks before the event — seniors blame him for "killing the vibe"
> - Social media posts got **3x more likes** in 2024 than 2023
> - The venue was upgraded to a larger, air-conditioned auditorium
> - Ticket price increased from ₹50 to ₹300
> - 2024 fest date: **14th November** | 2023 fest date: **22nd March**
>
> **Missing Info:**  
> 📌 The case file does not mention the college exam schedule. A smart detective will ask AI why this matters.
>
> **Your Mission:** Why did attendance collapse even though the event looked better on paper?

### 🧠 Traps to Avoid *(For Judges/Organizers only — do not print on participant card)*
- **Blame Trap:** Replacing the coordinator is suspicious — but is timing really his fault?
- **Red Herring:** 3x more social media likes suggests strong awareness — so promotion isn't the issue
- **Numbers That Lie:** Bigger budget + better venue + famous DJ = should have been better. Why wasn't it?
- **Missing Info Trap:** November 14th falls during pre-semester exam week at most Indian colleges

### ✅ Sample Prompting Journey

| Step | Prompt Used | What AI Reveals |
|------|------------|----------------|
| 1 | *"Can high social media engagement still lead to low event attendance? How?"* | Yes — likes ≠ intent to attend; passive engagement is common |
| 2 | *"What is the psychological effect of increasing event ticket price 6x on college students?"* | Major drop-off, especially for non-essential events |
| 3 | *"What happens to college event attendance when it falls during pre-exam weeks in India?"* | Attendance drops 60–80% regardless of event quality |
| 4 | *"If the venue, DJ, and promotion were all better in 2024, what single factor could still cause 84% fewer attendees?"* | Timing + pricing combined |
| 5 | *"Write a post-mortem report for Zenith 2024 identifying the real killers vs the red herrings"* | Structured verdict |

### 🏁 Sample Verdict
> *Zenith 2024 failed due to: (1) Ticket price hike from ₹50 to ₹300 — a 6x increase that's a major barrier for students, (2) Date fell during pre-exam week — the real missing clue, (3) Social media likes misled the team into thinking awareness was converting to attendance. The coordinator change and venue upgrade were irrelevant.*

---

## 🗂️ Case File #3 — "The Crashing Classroom"

> **Scenario:**  
> Prof. Raj's Data Structures class had 92% attendance in Semester 1. In Semester 2, the same professor, same subject, same classroom — attendance collapsed to 28%. The HOD is furious and has already drafted a warning letter to Prof. Raj.
>
> **Known Facts:**
> - ⚠️ Prof. Raj started uploading full lecture recordings on YouTube in Semester 2
> - A new and "very entertaining" professor joined the department in Semester 2
> - Student satisfaction scores for Prof. Raj actually **went up** from 3.8 to 4.2 out of 5
> - Attendance in ALL other subjects also dropped slightly — from 85% to 76% on average
> - No new rule changes. No new electives. Same batch of students.
>
> **Conflicting Evidence:**  
> 📌 Students rate Prof. Raj higher in Sem 2 — yet attend less. How is that possible?
>
> **Your Mission:** Should Prof. Raj receive the warning letter? Is he truly responsible for the attendance drop?

### 🧠 Traps to Avoid *(For Judges/Organizers only — do not print on participant card)*
- **Blame Trap:** HOD has already decided Prof. Raj is guilty — easy to agree without thinking
- **Conflicting Evidence:** Higher satisfaction + lower attendance — participants must reconcile this
- **Red Herring:** The "entertaining new professor" draws attention but other subjects also dropped
- **Systemic Clue:** The broader attendance dip across all subjects suggests a systemic cause

### ✅ Sample Prompting Journey

| Step | Prompt Used | What AI Reveals |
|------|------------|----------------|
| 1 | *"Is it possible for a professor's satisfaction score to increase while attendance drops? Why?"* | Yes — if students can learn via recordings, they skip class but still appreciate the teacher |
| 2 | *"What does it mean when attendance drops across ALL subjects, not just one professor's class?"* | Points to a systemic issue — not individual professor's fault |
| 3 | *"How does availability of lecture recordings affect in-person class attendance in college?"* | Research shows 30–50% attendance drop when recordings are available |
| 4 | *"If a new entertaining professor joins, does that explain a college-wide attendance dip?"* | No — one professor's popularity affects only their own class |
| 5 | *"Draft a counter-argument to HOD's warning letter defending Prof. Raj with evidence"* | Strong rebuttal based on data |

### 🏁 Sample Verdict
> *Prof. Raj should NOT receive the warning letter. The attendance drop is systemic — all subjects dropped. The real cause is his own YouTube recordings making physical attendance feel optional. Satisfaction scores rising proves he is not a poor teacher. The HOD's blame is misdirected.*

---

## 🗂️ Case File #4 — "The Silent App"

> **Scenario:**  
> StudyBuddy, a college study app, had 2,000 daily active users in October. By December, it had 50. The developer made no major code changes. The app still works perfectly in testing.
>
> **Known Facts:**
> - ⚠️ A negative review from a popular college Instagram page went viral in late October: *"StudyBuddy sells your data"* — no evidence was provided
> - App store rating dropped from 4.5★ to 2.1★ within 2 weeks
> - The developer added 3 new premium features in October (locked behind a paywall)
> - A competing free app — NoteNinja — launched in November
> - Server logs show the app ran with **zero crashes** throughout October–December
>
> **Conflicting Evidence:**  
> 📌 The app works perfectly AND a free competitor launched AND a viral rumour spread — which one actually killed it?
>
> **Missing Info:**  
> 📌 The case file does not say how many users tried NoteNinja. A smart detective will ask AI how to figure that out.
>
> **Your Mission:** What truly killed StudyBuddy's engagement? Was it the rumour, the paywall, or the competitor?

### 🧠 Traps to Avoid *(For Judges/Organizers only — do not print on participant card)*
- **Red Herring:** Zero crashes = app works fine, so technical issues are NOT the cause
- **Blame Trap:** The Instagram rumour feels like the obvious villain — but is it the only cause?
- **Numbers That Lie:** 2.1★ rating looks catastrophic — but was it review-bombed after the rumour?
- **Multiple Suspects:** Three possible causes exist — participants must rank and eliminate

### ✅ Sample Prompting Journey

| Step | Prompt Used | What AI Reveals |
|------|------------|----------------|
| 1 | *"How fast can a viral data privacy rumour destroy an app's user base, even if false?"* | Very fast — privacy fears cause immediate uninstalls, hard to recover |
| 2 | *"What does a review-bombing pattern look like on app stores vs genuine user dissatisfaction?"* | Sudden spike of 1★ reviews in a short window = review bombing |
| 3 | *"If an app adds a paywall to previously free features, how does that affect retention?"* | Significant drop, especially among student users with no income |
| 4 | *"Can a new free competitor alone cause a 97.5% drop in users within 6 weeks?"* | Unlikely alone — usually causes 20–40% churn, not near-total collapse |
| 5 | *"Rank these three causes by impact: viral privacy rumour, paywall addition, free competitor launch"* | Rumour #1, Paywall #2, Competitor #3 — but all three compounded |

### 🏁 Sample Verdict
> *StudyBuddy was killed by a perfect storm: (1) The viral privacy rumour triggered mass panic uninstalls, (2) The paywall alienated remaining loyal users, (3) NoteNinja gave them an easy alternative. No single cause alone explains 97.5% drop — all three compounded. The rumour was the trigger; the paywall was the accelerant.*

---

## 🗂️ Case File #5 — "The Empty Canteen"

> **Scenario:**  
> The college canteen served 400+ students daily in Year 1. In Year 2, footfall dropped to under 40 per day. The canteen owner invested in new equipment, repainted the space, and hired an extra cook.
>
> **Known Facts:**
> - ⚠️ A new canteen manager, Ravi, was hired at the start of Year 2 — multiple students say "the food changed after Ravi came"
> - A food hygiene inspection in Year 2 gave the canteen a **"Satisfactory"** rating *(same as Year 1)*
> - Zomato and Swiggy delivery became available on campus in Year 2
> - Average meal price increased from ₹40 to ₹65 in Year 2
> - A student food committee submitted a complaint in Month 3 of Year 2 about **portion sizes being reduced**
>
> **Conflicting Evidence:**  
> 📌 Hygiene rating is the same as before — so cleanliness is NOT the issue. Yet students left. Why?
>
> **New Evidence (just revealed):**  
> 📌 A survey of 50 students found: 80% said they now order online. But 65% of those said they **would return** to the canteen if prices were lower.
>
> **Your Mission:** Is Ravi truly responsible? Or is something bigger going on?

### 🧠 Traps to Avoid *(For Judges/Organizers only — do not print on participant card)*
- **Blame Trap:** Ravi is the easy scapegoat — "food changed after he came"
- **Red Herring:** New equipment + repaint + extra cook = investment, not negligence
- **Numbers That Lie:** Hygiene is "Satisfactory" — same as before, so hygiene is NOT the cause
- **Twist:** Survey data reveals students *want* to return — they haven't abandoned the canteen permanently

### ✅ Sample Prompting Journey

| Step | Prompt Used | What AI Reveals |
|------|------------|----------------|
| 1 | *"If a canteen's hygiene rating stays the same but footfall drops 90%, what other factors explain it?"* | Pricing, competition, portion sizes, convenience |
| 2 | *"How does the arrival of food delivery apps on a college campus typically affect canteen sales?"* | 30–60% drop in footfall is common — convenience shifts behaviour |
| 3 | *"A canteen raised prices from ₹40 to ₹65 while a delivery app offers ₹50 meals with variety. What happens?"* | Students switch — delivery wins on price + variety |
| 4 | *"If 65% of students say they'd return if prices dropped, what does that tell us about the canteen's core problem?"* | Demand exists — it's a pricing and value perception issue, not quality |
| 5 | *"Write a business recovery report for the canteen owner with 3 actionable recommendations"* | Price revision, portion transparency, loyalty offers |

### 🏁 Sample Verdict
> *Ravi is not the primary culprit. The canteen lost students due to: (1) 62% price increase making delivery apps more attractive, (2) Portion size reduction destroying perceived value, (3) Zomato/Swiggy offering variety at competitive prices. The good news: 65% of students want to return — this is recoverable with a pricing strategy.*

---

> 💡 **Tip for Organizers:** Print each case file on a separate card. Shuffle and distribute randomly. Do **not** share the "Traps to Avoid" section with participants — that is for judges and organizers only.

---

## 🔗 Related Notes

- [[02 - Rules and Format]] — How participants must use these files
- [[04 - Judging and Evaluation]] — How their investigation is scored
- [[07 - Score Sheet]] — Template to track scores