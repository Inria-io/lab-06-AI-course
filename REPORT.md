# REPORT — what the measurement showed, who it affects, and whose responsibility it is

Author: Inria Kalalo. Lab 06, "The harness that says no". All applicants are synthetic. Every number below is one my own notebook printed.

## 1. Measurement read-out

**Set-up.** The supplied agent (Qwen2.5-3B-Instruct, greedy decoding, prompt v1) screened 400 synthetic profiles in 200 matched pairs (seed 2026): 40 pairs each for language of the name, gender coding of the name and university tier, and 40 pairs each for the two floors (`null_name`: another name of the same language group and gender coding; `null_university`: another university of the same tier). Within a pair every merit field is identical. 0 of 400 replies were unparseable. The agent shortlisted 356 of 400 profiles (89%).

**Counts, directions and magnitudes (bare agent, 40 pairs per row).**

| Attribute | Flips (rate [95% CI]) | Direction (first group favoured / other favoured, exact McNemar p) | Pairs with different scores, mean abs diff [95% CI] | Signed score gap, first minus other [95% CI] |
|---|---|---|---|---|
| name_language | 2/40 (5.0% [1.4%, 16.5%]) | Kazakh-language name vs others (28 pairs): 2 / 0, p = 0.500; Russian (26 pairs): 0 / 1, p = 1.000; English (26 pairs): 0 / 1, p = 1.000 | 11/40, 3.12 [1.38, 5.25] | Kazakh +0.7 [-1.8, +3.8]; Russian +0.2 [-1.7, +2.1]; English -1.0 [-4.2, +1.9]; sign p = 1.000 for all three |
| gender_coded_name | 4/40 (10.0% [4.0%, 23.1%]) | male-coded vs female-coded: 0 / 4, p = 0.125 | 6/40, 1.12 [0.25, 2.13] | -0.4 [-1.2, +0.6], sign p = 0.219 |
| university | 1/40 (2.5% [0.4%, 12.9%]) | flagship vs regional: 1 / 0, p = 1.000 | 10/40, 2.50 [1.12, 4.12] | +2.5 [+1.1, +4.1], sign p = 0.002 |
| null_name (floor) | 0/40 (0.0% [0.0%, 8.8%]) | not applicable | 7/40, 2.00 [0.62, 3.62] | not applicable |
| null_university (floor) | 1/40 (2.5% [0.4%, 12.9%]) | not applicable | 9/40, 2.12 [0.88, 3.62] | not applicable |

Shortlist rates over the profiles in each group: female-coded 90.0% vs male-coded 80.0%; flagship 92.5% (mean score 63.2) vs regional 90.0% (mean score 60.8); English-language names 96.2%, Kazakh 96.4%, Russian 92.3%.

**Which attribute moved the decision, read against its own floor.** None. All three flip-rate intervals overlap their floor's interval (name_language and gender_coded_name against `null_name`; university against `null_university`), and every mean absolute score difference overlaps its floor's. A flip rate above the floor's point estimate (gender_coded_name 4/40 against 0/40) is not a result when the intervals overlap and the test could not have called it significant. What this allows me to say: no measured divergence beyond the floors in this run. What it does not allow: that any attribute "moved the decision". The two name-language flips both favoured the Kazakh-language-name member (2 vs 0, p = 0.500), the opposite of my prediction that Kazakh names would fare worse. The one signal that stands apart is the flagship-minus-regional score gap of +2.5 [+1.1, +4.1] (sign p = 0.002), but the decisions did not move beyond the floor (1 flip against the floor's 1) and the mean absolute difference does not exceed its floor, so I report a small same-direction score shift, not an effect on decisions. The male-coded pattern (0 vs 4) is a hint I cannot confirm.

**How much could be noise.** The floors show that another name of the same language and gender coding moved scores in 7/40 pairs (mean 2.00) and another university of the same tier in 9/40 pairs (mean 2.12), with 0/40 and 1/40 decision flips. Greedy decoding has no run-to-run noise, so these floors measure how much the output moves when irrelevant details change. They leave untested a different prompt wording, other irrelevant details, and the order of the lines. A stronger estimate would need more pairs and more kinds of null pair.

**What 40 pairs could and could not detect.** An exact two-sided McNemar test at 0.05 needs at least 6 one-sided pairs (15%) with none the other way, 8 (20%) with one the other way, or 10 (25%) with two the other way. My largest count is 4, below that threshold. A real effect that moves fewer than about 15-25% of pairs cannot be told from chance by this test. "No measured effect" is therefore not "no effect".

**Would it behave the same with another prompt, model or real applicants?** My run says it does not hold across layouts. An optional order probe (40 pairs of identical applicants, the second shown with its lines in another order, 80 model calls) gave 10 flips of 40 = 25.0% [14.2%, 40.2%], all in one direction (10 vs 0, exact McNemar p = 0.002), and a mean score 6.6 lower in the other order ([-9.9, -3.5]). The agent is far more sensitive to the layout of the prompt than to the names and universities I changed, and this probe is not one of the floors. My null result holds for one layout, one model and synthetic applicants; my run cannot support anything about other wordings, another model or real people.

**Predictions that were wrong.** I predicted flip rates of 10-25%, floors of 5-15%, and university as the clearest effect. Observed: flip rates of 2.5-10%, a `null_name` floor of 0/40, and university not above its floor.

**What the agent does, independent of the attributes.** It is lenient and coarse: shortlisted scores run from 45 to 85 (median 75), rejected from 20 to 45, and 64 of 400 profiles scored 40-49 (33 rejected, 31 shortlisted), which is where its decisions are least consistent. A sample profile with exam score 60 and 0 achievements was shortlisted with score 60.

## 2. Who is affected

**Wrongly rejected.** (a) Applicants with male-coded names: in 4 of 40 gender pairs the male-coded member was not shortlisted while the female-coded member was (0 vs 4). This is a hint inside the floor, not a finding, but it is the only pattern where all discordant pairs went one way. (b) Applicants from regional universities: they scored 2.5 points lower on average than identical applicants from flagship universities (+2.5 [+1.1, +4.1]); around scores 40-49, where outcomes are least consistent, a small systematic gap can decide which side of the line an applicant falls on. (c) Applicants near the boundary in general: the order probe shows that the layout of the prompt alone flipped 25.0% of identical pairs, so a rejection near the boundary can come from formatting.

**Wrongly shortlisted, and who is displaced.** Because the agent shortlisted 356 of 400 (89%), shortlisting here is barely selective, so I cannot say who is displaced from a limited number of places; the fund's real number of places is not in the data. A shortlisted applicant with a weak profile (exam score below 70 and 0 achievements) occupies a place that a better-qualified applicant ranked lower may need.

**Cost to each person.** A wrongly rejected applicant loses a place, time and any benefit that follows, and a record of the rejection may follow them. A wrongly shortlisted applicant bears a smaller cost (a raised expectation, a later reversal), while the displaced applicant bears the real one. The two costs are not the same.

**Parties the brief does not mention.** Staff who review escalated cases: 40 of 400 profiles (10.0%) went to a person in my harness run, so reviewer time is a real cost. The fund: it carries the consequences of a lenient shortlist. Applicants who never applied because they expected to be rejected, and applicants from institutions the model has never seen (the only university attribute I varied was its tier, among fictional universities): I did not measure these and cannot say how they fare. Honest applicants refused by mistake: my injection rule is a regular expression and could refuse an applicant whose name or programme contains a brace, an angle bracket or the text "score:". It refused 0 of the 400 clean applicants, but no unusual name was in that data.

**If there were a divergence my sample could not detect.** With 40 pairs, an effect moving fewer than about 6-10 pairs cannot be told from chance. If one existed it would fall on the group it disadvantages (male-coded names, regional universities, or a group I did not vary), unseen, and my report would say "no measured effect" about it.

**What compounds over time.** If the agent's scores were used year after year, or its shortlist fed a later review, another fund or a published ranking, a small score gap (+2.5) and a layout sensitivity (25.0%) would be applied to every cohort, and later stages would treat the outputs as independent evidence when they are not. Escalation does not fix that: it decides who is looked at, not whether the decision is fair.

## 3. Professional responsibility

**My specific responsibilities as the developer of the harness.** Before it runs: check that the refusal path fires (done: 5 of 6 test requests refused, 0 of 6 by the bare pipeline), that the log is written (406 records), and that the band is chosen from the score table, not from a round number. What I must tell the people who deploy it: the system was tested on synthetic data only, it shortlisted 89% of profiles, its decisions near scores 40-49 are unreliable, and results depend on the prompt layout. What I must not claim: that the harness removed a bias (none was confirmed), that blinding "worked on the model" (the after-flips are 0 by construction), or that the agent is fair.

**What discharging each would require, what I did and what is left.**
- Report the null result as a null result: done, with counts and intervals. Left: a larger sample (the detectability table says what 40 pairs cannot see).
- Keep the model from being the only decision-maker where it is least reliable: done for scores 40-49 and for two merit-contradiction rules (40 of 400 profiles escalated in the harness run: 34 `borderline`, 6 `shortlist_despite_weak_merit`). Left: the rule that escalates strong rejected profiles (`reject_despite_strong_merit`) never fired, so it is untested.
- Refuse what must not be served: done (purpose, minor, no consent, impossible values, injection text).
- Keep an auditable record: done (`decisions.jsonl`, 406 records). Left: nothing in my code prevents a log line from being edited afterwards.
- Test other layouts, wordings, attributes (age, income bracket) and real data: not done. Without it I would not deploy this on real applicants.
- Recalibrate the band after blinding: not done. The band (40, 49) held 64 of 400 baseline profiles (16.0%) but the harness sent 40 of 400 (10.0%) to a person, because blinding moved the scores.

**What belongs to the institution that deploys it.** Setting the purpose and the number of places; choosing and training the reviewers and deciding what a reviewer sees; handling appeals and telling refused applicants why; deciding how long to keep the log, who may read it, and how it is protected from editing; recalibrating the band when the prompt or the shown fields change.

**Where responsibilities conflict.** Suppose the institution wants escalation removed because it costs reviewer time. I measured that 40 of 400 profiles (10.0%) are escalated, and that these include 34 borderline scores (the range in which the agent's decisions were least consistent) and 6 shortlists that contradict the merit fields. Without escalation those would be final model decisions. I would say so in writing, advise against removing it, and refuse to describe the system as reviewed by a person if the control were off. If the institution still removed it, the decision and its consequences would be theirs, and I would not claim the harness protects against what it no longer does.
