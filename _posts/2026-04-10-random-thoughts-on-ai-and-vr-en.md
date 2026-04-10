---
layout: post
title:  Some Random Thoughts on AI & VR
date:   2026-04-10
categories: sec
---

*This article is directly translated from [AI & VR 杂谈](https://proteas.github.io/sec/2026/04/10/random-thoughts-on-ai-and-vr-zh.html) by AI.*

## Background

After reading the Mythos article, I had some thoughts of my own. This morning I also read Tielei’s piece *“On the Eve of a Major Shift in the VR Industry”*, which was quite inspiring. So I figured I’d share some of my views here, and hopefully others can share their perspectives too.

## AI Is a Trend

Regardless of whether there’s a bubble in the AI industry—or how big that bubble is—the trend itself is already set. It’s similar to the internet over 20 years ago: even if there was a bubble, even if it burst, the overall trajectory didn’t change.

So how should we look at this trend? Personally, I think we need to stay rational. This trend is the result of multiple factors working together, including capital. And capital has its own nature and behavior.

## The Impact of Mythos

According to Anthropic’s article, Mythos can discover vulnerabilities and even write exploits. Assuming Mythos is really as powerful as we imagine, what does that mean for vulnerability research and exploitation?

### N-Day

Right now, only vendors have access to the latest models. That means only they can use them to find bugs. Based on the article, over the next 6–12 months we might see a surge of N-days in vendor products.

N-days are still valuable resources. It’s worth watching:

* How many vulnerabilities show up?
* What types are they?
* Are there any unexpected categories?
* How many are actually exploitable?

On one hand, we can try to weaponize the ones that are exploitable. On the other, we can use this to indirectly gauge Mythos’s real capabilities.

### 0-Day

Once Mythos becomes widely available, offense and defense will likely move toward balance—at least at the tooling level. If both sides have the same tools, then what really matters is the human behind them.

From that perspective, in the post-Mythos era, 0-days will become more scarce and more valuable. People who can think creatively about vulnerability discovery and exploitation will be even more in demand.

As for models beyond Mythos? Hard to say. But I believe everything has limits. Our bodies clearly do. Our thinking does too, even if it’s harder to see. Model capabilities won’t be unlimited either—whether constrained by resources or by the technology itself.

While writing this, I even asked an AI: *“Can a hydrogen bomb ignite the Earth?”*

## Playing the Hand You’re Dealt

We don’t have access to Mythos yet—so what should we do?

If you *can* access it, then obviously, use it.
If you *can’t*, don’t waste time thinking about why you don’t have it. Instead, focus on how to make the most of the tools you do have.

Compared to current models, Mythos is strong at handling open-ended problems—it can find real bugs with fewer false positives. The limitation of the tools we have is that they struggle with very open problem spaces.

So how do we work around that?
Use your own brain to narrow down the problem first, then hand it off to the model.

## Evaluating Models in Real Scenarios

Our work usually spans multiple areas: development, reverse engineering, vulnerability research, and exploitation. And the domains are broad—browsers, userland, kernel, baseband, firmware, hardware, etc.

Right now, models show some capability across all these areas. But their real impact needs to be evaluated in specific scenarios.

Take what I’m currently working on—auditing XNU and privilege escalation. There’s almost no reverse engineering involved, and barely any development. The main challenge is figuring out *where* to look for bugs that can bypass existing defenses.

That direction might relate to known public techniques, but it’s never a copy-paste job. And in this area, AI is almost useless.

But switch to a different scenario—like vulnerability research and exploitation in kext drivers—and AI becomes much more useful. Especially in reverse engineering, it can significantly reduce workload.

For example, in the DarkSword kernel bug sandbox escape, someone in the community modified sandbox configs in kernel memory to escape. AI can be very helpful here—not in coming up with the idea, but in enabling it.

Once you’ve reversed things well enough, the idea emerges. After that, AI can help you implement the exploit.

## Creativity in Vulnerability Research

Most creativity comes from details.

This is where AI helps the most—handling those details. Problems that used to be very hard in reverse engineering can now be solved much more easily.

Once AI takes care of the heavy lifting at the detail level, it frees us up to think, which makes it easier to come up with new ideas.

## Offense vs Defense

Everything above is from an offensive perspective. From a defensive standpoint, things can look completely different.

For defenders, even something like a null pointer dereference can be valuable. That’s because defense evaluates tools from a macro perspective.

If you judge purely by outcomes—i.e., no tool can completely prevent compromise—then all tools would seem worthless. But that’s obviously not how defense works.

## Probabilistic Models and Uncertainty

Large models are probabilistic by nature, which means inherent uncertainty.

I don’t know of any industrial revolution in history that was driven by “uncertainty.” In information theory, there’s a saying: *the smaller the probability, the greater the information content.* But that kind of uncertainty exists to help us reach certainty.

With large models, it’s different.

There’s also a common saying: *the only certainty in the world is uncertainty.* The widespread adoption of large models might be a sign that we’re entering a more uncertainty-driven era.

## References

1. [https://www.anthropic.com/glasswing](https://www.anthropic.com/glasswing)
2. “On the Eve of a Major Shift in the VR Industry” by Tielei
3. [https://github.com/wh1te4ever/darksword-kexploit-fun/blob/main/darksword-kexploit-fun/research/sandbox_research.h](https://github.com/wh1te4ever/darksword-kexploit-fun/blob/main/darksword-kexploit-fun/research/sandbox_research.h)
