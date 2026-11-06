# Clock Race

**Live:** https://gwionrob.github.io/clock-race/

## Why this exists

This was made to settle an argument with my dad.

The question: you've got two clocks that start together at midnight. One keeps normal time, the other runs at **1/x** speed. How often do they show the same time in a day, a week, a month?

We couldn't agree on the answer. So instead of arguing about it over dinner, I built a thing that animates both clocks, counts every time they match, and shows the maths. Whoever was wrong can go and look at it.

## The maths

After `t` real hours the normal clock has moved `t` hours and the slow one has moved `t/x` hours. An analog clock face only shows time modulo 12 hours, so the two faces look the same whenever the gap between them is a whole number of 12-hour laps:

```
t − t/x = 12k
t = 12k / |1 − 1/x|
```

So they match every `12 / |1 − 1/x|` hours, which works out to **`2·|1 − 1/x|` times per day**.

| x | Slow clock speed | Match every | Per day | Per week | Per 30-day month |
|---|---|---|---|---|---|
| 2 | half speed | 24 h | 1 | 7 | 30 |
| 3 | third speed | 18 h | 1.33 avg | 9 | 40 |
| 4 | quarter speed | 16 h | 1.5 avg | 10 | 45 |
| 12 | 1/12 speed | ~13.1 h | 1.83 avg | 12 | 55 |
| ∞ | stopped | 12 h | 2 | 14 | 60 |

Two checks that the formula makes sense:

- **x → ∞:** the slow clock is basically stopped, and a stopped clock is right twice a day. The formula gives 2.
- **x = 1:** they're the same clock, so they always match.

These counts don't include the shared midnight start. Add one if you want to count it.

There's also a 24-hour dial toggle. With a 24-hour face every number above halves.

## Running it

It's one static HTML file with no build step:

```sh
open index.html
```
