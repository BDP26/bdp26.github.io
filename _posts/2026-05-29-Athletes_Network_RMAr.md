---
layout: post
title: "Life After Sports: The AI Job Matchmaker Reality Check"
author: Vivek Viruthiyel & Colin Keller
---

# Life After Sports: The AI Job Matchmaker Reality Check

Years of elite training teach unparalleled discipline, resilience, and teamwork. But what happens when the final whistle blows on a professional sports career? Athletes possess incredible skills, yet their non-traditional backgrounds often fail to fit the rigid checkboxes of a standard corporate recruitment process.

For our Big Data project, we partnered with the [Athletes Network](https://www.athletes-network.com/), a Swiss startup that connects former professional athletes with corporate partners. Our goal: Can we automate this with AI?

### The Problem

The Athletes Network advisors manually review thousands of potential athlete-job pairings. Only about 1 in 20 is a genuine match, meaning that the athlete fits the job description. The existing system flags everything that looks remotely plausible, forcing advisors to spend most of their time rejecting bad suggestions.

![The AI Matchmaker: Finding the 5% Fit](../../../assets/img/AthletesNetwork_img.png)

We wanted to reduce that noise. Fewer false alarms mean advisors spend their time on candidates who actually fit.

### What We Did

We tested everything from simple spreadsheet-style models to advanced neural networks trained on athlete and job profiles—nine configurations across six different model families in total.

The twist: The data we had to work with consisted entirely of categorical checkboxes. Region. Industry. Education level. Contract type. There was no CV text, no cover letter, no work history narrative. Just lists of ticked boxes.

### What We Found

**How you describe a profile matters almost as much as which AI you use.**
When we converted the checkbox data into readable sentences like "The athlete is looking for a position in finance, holds a Master's degree, and is based in Zurich," the AI performed dramatically better. In fact, picking the right language model to read those sentences made a bigger difference than choosing between a simple or a complex network. Even within the same model, writing each field as a proper sentence instead of a raw data dump provided a highly meaningful boost.

**The AI hallucinated when asked to help.**
We tried using a language model (similar to a mini ChatGPT) to generate ideal candidate profiles for each job, hoping to give the AI richer context. Instead, it produced generic fluff like "strong teamwork and leadership skills" that matched almost every athlete and made things worse.

**Neural networks forgot what they learned.**
Our most sophisticated model looked brilliant in testing but fell apart on new data it had never seen. With only 426 athletes in the dataset, the AI had essentially memorized individual athletes rather than learning what makes a good match in general.

### The Bottom Line

Our best model cut false positives by roughly 40% compared to the existing system—a real improvement that saves advisor time. However, it required significantly more computing power to get there, and it is still nowhere near replacing human judgment.

The honest conclusion: **the data is the ceiling, not the algorithm.** Checkbox profiles simply do not contain enough information for AI to make fine-grained hiring decisions. The biggest leap forward would come from giving the AI full CVs and job descriptions to read, rather than building a smarter model on top of ticked boxes.

AI is a useful filter. It is not a matchmaker.

**Over to you:** Are there skills from your own background that a standard job application form simply cannot capture? That is exactly the problem the Athletes Network is trying to solve.
