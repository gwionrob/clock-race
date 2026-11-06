# Clock Race

**Live:** [Clock Race](https://gwionrob.github.io/clock-race/)

## Why this exists

This was made to settle an argument with my dad.

The question: you've got two clocks that start together at midnight.
One keeps normal time, the other runs at **1/x** speed.
How often do they show the same time in a day, a week, a month?

We couldn't agree on the answer. So instead of arguing about it over dinner,
I built a thing that animates both clocks, counts every time they match,
and shows the maths. Whoever was wrong can go and look at it.

## The maths

After `t` real hours the normal clock has moved `t` hours and the slow
one has moved `t/x` hours. An analog clock face only shows time modulo
12 hours, so the two faces look the same whenever the gap between them
is a whole number of 12-hour laps:

```
t − t/x = 12k
t = 12k / |1 − 1/x|
```

So they match every `12 / |1 − 1/x|` hours, which works out to **`2·|1 − 1/x|`
times per day**.

| x   | Slow clock speed | Match every | Per day  | Per week | Per 30-day month |
| --- | ---------------- | ----------- | -------- | -------- | ---------------- |
| 2   | half speed       | 24 h        | 1        | 7        | 30               |
| 3   | third speed      | 18 h        | 1.33 avg | 9        | 40               |
| 4   | quarter speed    | 16 h        | 1.5 avg  | 10       | 45               |
| 12  | 1/12 speed       | ~13.1 h     | 1.83 avg | 12       | 55               |
| ∞   | stopped          | 12 h        | 2        | 14       | 60               |

Two checks that the formula makes sense:

- **x → ∞:** the slow clock is basically stopped, and a stopped clock is
  right twice a day.
  The formula gives 2.
- **x = 1:** they're the same clock, so they always match.

These counts don't include the shared midnight start.
Add one if you want to count it.

There's also a 24-hour dial toggle. With a 24-hour face every number above halves.

### Different start times

You can set a start time for each clock. They still match every `12 / |1 − 1/x|` hours, but the first match comes sooner because the clocks are already part of the way there. If Clock A is `g` hours ahead at the start, the first match is at `(12 − g) / (1 − 1/x)` hours, wrapping `g` to the 0–12 range. That assumes the slow clock is the one being lapped.

### Drama mode

As a match gets close, the clocks start to throb like a heartbeat that speeds up, the panel glows, playback slows down, and each clock shows a dotted ghost of the other clock's hour hand so you can watch them close in. When they line up, everything freezes on the exact moment, shakes, and bursts into confetti with a big **MATCH!** Sound (heartbeat ticks and a chime) is on by default and kicks in after your first click on the page, since browsers block audio until then. The *Sound* button mutes it. Turn it all off with the *Drama* button. Shaking and confetti also switch off if your system is set to reduce motion.

### Plain-English mode

The page doesn't make you think in "1/x". You pick Clock B's speed from buttons like *Half speed* or *Quarter speed*, or type a percentage. The answer and explanation are written in plain sentences. The algebra is still there behind a "Show the algebra" toggle for anyone who wants it.

## Running it

It's one static HTML file with no build step:

```sh
open index.html
```
