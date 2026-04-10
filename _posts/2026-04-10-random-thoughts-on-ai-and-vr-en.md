---
layout: post
title:  Some Random Thoughts on AI & VR
date:   2026-04-10
categories: sec
---

*This article is directly translated from [AI & VR 杂谈](https://proteas.github.io/sec/2026/04/10/random-thoughts-on-ai-and-vr-zh.html) by AI.* <br/>

## The Backdrop
After reading the recent Anthropic article on "Mythos" and being inspired by Tielei’s piece, *On the Eve of a Great Shift in the VR Industry*, I wanted to share some of my own takes. I'd love to hear what everyone else thinks about where we’re headed.

## AI is the Trend (Bubble or Not)
Whether the AI industry is a bubble—and how big that bubble might be—is almost secondary to the fact that the trend is already here. It feels like the internet twenty years ago; even after the dot-com bubble burst, the underlying shift didn't stop. We need to look at this rationally. This momentum is the result of many forces, including massive capital backing, and capital has a very specific set of behaviors.

## The "Mythos" Impact
According to Anthropic, Mythos can identify vulnerabilities and write exploits. If it's truly as powerful as advertised, what does that mean for the VR industry?

* **The N-Day Landscape:** Currently, only vendors have access to the latest models. This means for the next six months to a year, we’ll likely see a flood of N-days coming from vendor-side research. These N-days are a resource in themselves. We should pay attention: How many are popping up? What types are they? Are we seeing any "black swan" bug classes? Most importantly, how many are actually exploitable? This will be the ultimate litmus test for Mythos’s true capabilities.
* **The 0-Day Landscape:** Once Mythos-level tools become widely available, the "arms race" will reach a new equilibrium. If everyone has the same high-powered tool, the deciding factor shifts back to the person behind the machine. In a post-Mythos world, 0-days will become scarcer and more valuable. Creative researchers who can think outside the box will be more in demand than ever.

As for what comes after Mythos? I’m not sure. Everything has a limit—our bodies do, and though it's harder to see, our thoughts do too. Models will eventually hit a wall, whether it's resource-related or a fundamental technical plateau. It reminds me of a question I once asked an AI: "Can a hydrogen bomb ignite the entire Earth’s atmosphere?" 

## Playing the Hand You’re Dealt
Most of us don't have access to Mythos yet. So, what’s the move? If you have a way to get your hands on it, do it. If not, don't waste time complaining. Instead, focus on how to play your current hand better.

The edge Mythos has over current models is its ability to handle "open-ended" problems with low false positives. Our current "hand" (existing models) struggles with wide-open questions. The solution? Use your brain to narrow the scope. Do the heavy lifting of defining the problem space, then hand the specific, constrained task to the model.

## Evaluating AI in the Trenches
Our work spans development, reversing, bug hunting, and exploitation across various domains: browsers, userspace, kernels, basebands, firmware, and hardware. While AI shows promise across the board, its actual utility depends on the specific scenario.

* **Example 1: XNU Auditing & LPE.** In my current work on XNU kernel exploitation, I’m not doing much reversing or dev work. The bottleneck is creative: figuring out a new direction to bypass modern mitigations. AI is almost useless here because these directions aren't just "copy-paste" from public research.
* **Example 2: Kext Drivers.** Here, AI is a game-changer. It can gut the workload of reverse engineering. Take the *DarkSword* sandbox escape, for instance—researchers achieved escape by modifying sandbox configurations directly in kernel memory. AI wouldn't necessarily "dream up" that specific strategy on its own, but it can clear the reversing fog so quickly that the human researcher can actually spot the opportunity and then use the AI to help write the exploit.

## Creativity Lives in the Details
Most "creative" breakthroughs in VR come from obsession with the details. AI's biggest contribution is handling those granular, soul-crushing details (like complex reversing tasks) that used to be massive roadblocks. By lowering the barrier to understanding the "how," AI frees us up to focus on the "what if."

## Offense vs. Defense
Most of this is from an attacker's perspective. For defenders, the math is different. For a defender, an AI finding a simple Null Pointer Dereference is a win. Defense requires a macro view; if you judge a tool solely by whether it makes a system "unhackable," then no tool has value. But if it raises the cost for the attacker, it’s working.

## Probability and the Era of Uncertainty
Large Language Models are probabilistic at their core—they are built on fundamental uncertainty. I can't think of a previous industrial revolution driven by "uncertainty." In information theory, the lower the probability, the higher the information content. But LLMs use that uncertainty to chase a "certain" output. It’s a bit of a paradox. We often say the only certainty is uncertainty; the mass adoption of AI might just mean we are entering an era where that’s more true than ever.

---

**References:**
1. [https://www.anthropic.com/glasswing](https://www.anthropic.com/glasswing)
2. *On the Eve of a Great Shift in the VR Industry* (Tielei)
3. [https://github.com/wh1te4ever/darksword-kexploit-fun/blob/main/darksword-kexploit-fun/research/sandbox_research.h](https://github.com/wh1te4ever/darksword-kexploit-fun/blob/main/darksword-kexploit-fun/research/sandbox_research.h)
