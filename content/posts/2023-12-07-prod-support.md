---
title: Production Support
description: A simple guide to handling production incidents
date: 2023-12-07T18:44:57+05:30
draft: true
toc: false
images:
tags:
  - production,support
---

Your phone rings at 3AM. [_"Something's broken, something's broken; It's your fault..."_](https://soundcloud.com/pagerduty/somethings-broken) begins to sound and the better(allegedly) half kicks you out of bed. Crawling out from the mists of sleep, it hits you - you're on-call this week. Whatever's broken, you're the one who has to make it right. You _are_ the defender of the night. You _are_ the hero your team deserves. You _are_ the production support engineer. You _ARE_ screwed.

The manager-types are sending off emails and asking for calls and status reports. You enter a room full of headless chickens and know you have to do something... _anything_ - only you have no idea what.

### The First Rule of Production Support
In the immortal words of a certain Mr. Adams, **"Don't Panic."**

The first thing you need to do is to take a deep breath and calm down. You're not going to be able to fix anything if you're not calm and collected. You need to be able to think clearly. You need to be able to _think_.

### The Second Rule of Production Support
**"Primum non nocere."** - First, do no harm.

Like a doctor, you need to make sure the medicine doesn't kill the patient. Do not guess or make assumptions. Make changes only after you have a clear understanding of the problem. If you don't know what's wrong, don't fiddle with the knobs hoping for a miracle.

### The (Not So) Secret Sauce
The first thing to do is to systematically capture information so everyone involved in the _'incident'_ has the same information - no matter how long they've been in the room (or slack channel - trello board or whatever you use to track incidents).

#### 1. Capture the Symptoms
  What is happening? Where? How often? What is the impact? List out everything you believe is relevant. This is great for people joining the incident - they come up to speed quickly. You can continue to focus on the problem.

#### 2. Generate Hypotheses
  List out what could cause the symptoms listed. Don't rule out anything, no matter how crazy it sounds.

#### 3. Run Tests
  Come up with tests to rule out each hypothesis. Perform actions that provide evidence to rule something out. It's essential to structure the test to help you strike out a hypothesis, not prove it.

#### 4. Record Results
  Record the results of the tests. By this time, you should have ruled out all but a few hypotheses. You are now closer to understanding the root cause and can now make an informed decision on how to fix it. If you haven't, go back to step 2 (try harder! Feed your brain more sugar/saturated fats). 
  
  This process also acts as a record that helps the new people who've joined by outlining what you've already tried. It also serves as a great launchpad to write the Production Incident Report (yeah - you're not getting out of that one) and gain a deeper understanding of the system.

Summing up, the steps are:
1. Stay calm and don't act on assumptions.
2. Use a rational approach to diagnose the problem.
3. Fix the issue.
4. Write it up and save future **YOU** a few headaches.


### References
- [Troubleshooting Without Losing Common Ground - Dan Slimmon](https://www.youtube.com/watch?v=tTBffC6zF2g)
- [Troubleshooting: First, Do No Harm - Paul Smith](https://medium.com/everestengineering/troubleshooting-first-do-no-harm-c7d6d4fd6977)