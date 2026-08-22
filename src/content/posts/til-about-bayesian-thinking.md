---
title: TIL about Bayesian thinking
author: Federico D'Eredità
pubDatetime: 2026-08-02
slug: til-about-bayesian-thinking
featured: false
draft: false
tags:
  - til
  - bayesian-thinking
  - bayes
description:
  Plain-English notes on Bayesian thinking, priors, posteriors, base rates, and why the same test accuracy gives wildly different real-world odds depending on how rare the thing is, with two everyday software engineering failure modes it explains.
---

## Context

Watched two videos on Bayesian thinking: Veritasium's ["The Bayesian Trap"](https://www.youtube.com/watch?v=R13BD8qKeTg) and BBC Ideas' ["The power of Bayesian reasoning"](https://www.youtube.com/watch?v=1LzdESG6-2E). No formulas here, just the plain-English version.

## Fact

ELI5 version: you have a guess about something. New information shows up. You nudge your guess based on that information, you don't throw the old guess away and start over. Do that enough times and your guess gets close to the truth. That's it, that's Bayesian thinking.

Three words worth knowing, in plain terms:

- **Prior**: your guess before the new information.
- **Posterior**: your guess after you've folded the new information in.
- **Base rate**: how common the thing actually is in general, before any test or evidence.

The base rate is the part people mess up, and it changes everything. Two examples, same test logic, wildly different results because the thing being tested for is common in one case and rare in the other.

**Mammogram example (common condition).** 100 women get screened for breast cancer. About 1 of them actually has it, and the test almost always catches her. Of the 99 who don't have cancer, the test still falsely flags about 3 of them (it's not perfect). So 4 women get a positive result, but only 1 of them actually has cancer. If you're one of the 4 people flagged, your real odds of having cancer are 1 in 4, 25%, not the 90%+ headline accuracy of the test.

**Rare disease example (rare condition).** Same idea, but the disease only affects 1 in 1000 people instead of 1 in 100. Even with a 99% accurate test, almost everyone who tests positive still doesn't have the disease, because there are so few real cases to begin with, the false positives from the huge healthy population swamp them. Real odds drop to single digits.

Same math, same test quality, completely different real-world answer, because the base rate (how common the thing is) is different. That's the whole trick: never trust a test's accuracy number alone, always ask how rare the thing being tested actually is.

Other places this shows up, from both videos:

- **Spam filters** update the probability an email is spam as each suspicious word or pattern is found, one word doesn't decide it, each one nudges the guess.
- **Breaking Enigma in WWII**, Turing's team at Bletchley Park updated their guesses about the machine's settings as new patterns turned up, instead of restarting from zero each time.
- **Your own brain** does this constantly: it predicts what you're about to see or hear, then corrects that prediction against what your senses actually report. Perception itself runs on this loop.

## Consequence

Two failure modes worth watching for, both everyday software engineering situations.

**Ignoring the base rate: alert fatigue.** Say a monitoring system flags "possible incident" with 99% accuracy, but real incidents only happen in 1 out of 1000 requests. Same math as the rare disease test: most flags are false positives. Engineers stop trusting the alerts, real incidents get lost in the noise. The fix isn't a "more accurate" model, it's picking alert thresholds against the actual base rate of incidents, not against the model's headline accuracy number.

**The "Bayesian trap": root cause fixation.** A recurring prod issue gets traced to the DB connection pool three times in a row. The team's prior hardens to "it's always the pool." Next incident, same symptoms, on-call skips investigating and just bumps the pool config again, no real check. Real cause this time: a downstream API timeout that only looks like pool exhaustion. Bug ships again. This is the trap exactly as the video describes it: repeated same-result evidence pushes belief toward certainty, so the team stops testing alternate hypotheses and keeps "fixing" a non-cause.

## Takeaway

For a software engineer, this boils down to two habits.

Never trust a classifier, alert, or test's accuracy number in isolation, always ask what the base rate of the thing it detects actually is. This is the same reasoning that should drive alert thresholds, anomaly detection tuning, and how much weight to put on any single automated flag.

Never let a root cause or a "prior" about a system harden into certainty. A postmortem or debugging session that fits one data point is a hypothesis, not a conclusion, treat it as a prior and keep it open to a genuinely independent piece of evidence before calling it done. The moment "it's always X" stops being questioned is the moment the team stops looking for the real bug.

Sources:

- [Veritasium: The Bayesian Trap](https://www.youtube.com/watch?v=R13BD8qKeTg)
- [BBC Ideas: The power of Bayesian reasoning](https://www.youtube.com/watch?v=1LzdESG6-2E)
