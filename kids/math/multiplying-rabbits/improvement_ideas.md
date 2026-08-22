Good instinct — right now it's basically a drill machine with a timer, not something that actually adapts to her. Here's
what would make it genuinely smarter, roughly in order of impact:

**1. Track mastery per individual fact, not just per level.**
Right now the only "memory" of her performance is which levels she's completed. That throws away the most useful data.
If you tracked correct/wrong (and maybe response time) for each of the ~96 facts individually, you'd know she's solid on
the 2s and 5s but keeps blowing 7×8 and 6×9 — which are famously the two hardest facts for basically every kid, for what
it's worth. That data is what everything else below depends on.

**2. Make problem selection adaptive instead of uniform random.**
Once you have per-fact history, stop picking randomly and start weighting toward facts she's gotten wrong before or
hasn't seen in a while — a simple spaced-repetition/Leitner-style bias. This is the single highest-leverage change.
Uniform random within a table means she spends equal time on 3×1 (trivial) and 3×9 (hard), which is a waste of the 10
minutes she's got.

**3. A "weak spots" view, for her and for you.**
A little heat-map grid of the multiplication table (rows 1-8, columns 1-12) colored by mastery — green/yellow/red —
would be genuinely motivating for a kid ("look, I turned my 7s green!") and useful for you as a parent to know what to
reinforce outside the app. This is more compelling pedagogically than the diploma, honestly — the diploma is a nice
reward but it doesn't teach anything, whereas watching a grid go from red to green gives her a visible sense of progress
on the actual skill.

**4. A real hint system, not just "keep guessing."**
Right now if she's stuck, she just keeps guessing with no scaffolding — that can go from "productive struggle" to "
frustrated flailing" pretty fast. After 2-3 wrong attempts, offer a hint tied to the number, not a canned "try again":
skip-counting ("6, 12, 18, 24... keep going"), a known-fact bridge ("you know 6×6=36, so 6×7 is just 6 more"), or a
simple dot array. And after enough failed attempts, just show the answer and have her retype it once — forcing her into
an unproductive guessing loop teaches nothing.

**7. Commutativity gap.**
Because problems are generated as `table × random(1-12)`, when she's doing the "3 table" she'll see 3×7 but never 7×3.
Recognizing that a×b = b×a is a real conceptual milestone, not just a fluency one — worth deliberately mixing both
orders in, especially in the mixed levels.

If I had to pick two to actually build first: per-fact mastery tracking + the heat-map view. Everything else (adaptive
selection, hints, spaced repetition) becomes much easier once that data model exists, and the heat-map alone is a strong
motivational payoff for relatively little work.