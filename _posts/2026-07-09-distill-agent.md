---
layout: post
title: "Distill Agent: AI-assisted data cleaning"
date: 2026-07-09
description: How and why I built Distill Agent, an AI data-cleaning agent that won an Honorable Mention at the STAI-X Challenge 2026.
tags: statistics ai
categories: distill-agent
---

I love most of the process of learning statistics, but I hate most of the process of working as a statistical programmer on collaborative projects. That's mostly because data processing and cleaning can take up almost 70% of the effort in an analysis. We go back and forth between data filtering and the statistical models, wading through messy code and poorly documented decisions &mdash; labor that requires almost none of what we actually learned in statistics.

At the STAI-X Challenge 2026, I finally saw a great opportunity to design an AI agent to help human statisticians with data cleaning. So I built [Distill Agent](https://github.com/dewei-lin/distill-agent) with Claude Code &mdash; a reusable data-cleaning agent that won an Honorable Mention in the competition.

## What problems are the agent designed to solve

I started with the general problem: how can we use AI to help human statisticians with data cleaning? Along the way, I made a few observations from my own experience:

1. AI writes neater, better-organized code and documentation than most human programmers.
2. Human statisticians prompt AI to do the job for them, but often fail to explain the rationale behind each decision the AI makes. People joke that accountants are the least likely to lose their jobs in the AI era, because AI can't be held accountable, only humans can. The same goes for statisticians: we're responsible for everything that goes in the report, not the AI.
3. There's a trade-off between the quality of the answer and how specific the prompt is. A single prompt like "do this data cleaning" often leads to unwanted results, because the AI doesn't actually know what to do given the limited **context** it's given and the **skills** it's equipped with, which is why I don't think a single AI agent should perform tasks that require different analytical skills. Instead, a high-quality answer usually comes from an informative, well-designed prompt: we need to give the AI the least freedom possible, and details to the fullest extent.

## The design of Distill agent

Here's where the design actually landed:

As human statisticians, we design a structured loop for data cleaning (duplicate deletion &rarr; missingness &rarr; ...), then assign sub-agents with specific *skills* to specific sub-tasks. During each session, the human statistician makes the decision calls, and the AI writes the code and documents the decisions made. At the end of the session, four outputs are generated:

1. A clean dataset, if the data doesn't contain any unstructured parts (images, notes, etc.)
2. A data-cleaning script that allows for one-line execution from raw data to clean data
3. A presentation-ready report documenting the characteristics of the data and the decisions made
4. A machine-readable log file that records the session, so users can reuse the assets from the session with another AI, and potentially with similar data

On top of that, an in-session AI agent is available to answer questions at any time during the session. This agent is equipped with *skills* that give it general knowledge of the data (and its documentation, if any) and the ability to run code and inspect the data. That means it can answer questions like "what does `temp` stand for?" or "do both variables have the same missingness rows?". These are the questions the sub-agents can't answer, since they're given less freedom to perform open-ended analysis.

## The product and the name

With the help of Claude Code, Distill Agent runs as a no-code, chat-based web interface with no scripting required. It can be deployed both online and locally, and is able to process small-to-moderate size data (less than 1 GB). The final layout has four major panels working together:

{% include figure.liquid loading="eager" path="assets/img/blog/distill-agent/product-panels.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Distill Agent interface: pipeline sidebar, cleaning session chat panel, variable inspector, and outputs panel" %}

The agent follows an Orchestrator&ndash;Worker pattern with human-in-the-loop checkpoints:

{% include figure.liquid loading="eager" path="assets/img/blog/distill-agent/pipeline-diagram-v2.png" class="img-fluid rounded z-depth-1" zoomable=true alt="Distill Agent pipeline: Upload, Intake, Orchestrator, cleaning stages, human-in-the-loop checkpoints, output generation" %}

And why the name Distill? Data is to data scientists what water is to life, and just as you can't survive on contaminated water, you can't build reliable models on dirty data. Distill Agent is built to be the distillation device for data. AI handles the mechanical, pattern-detection work autonomously, while the human analyst keeps full control over the judgment calls that require domain knowledge. And like a well-run water utility, nothing happens in the dark: every decision, automated or human, is logged, justified, and compiled into a reproducible set of outputs, including a standalone Python script that regenerates the cleaned data with no agent in the loop. Clean data you can drink, and a record of exactly how it was treated.

## Final thoughts

This was actually my first project working with AI, and it turned out to be a lot of fun. I'm looking forward to doing more research at the intersection of Statistics and AI.

Honestly, the most interesting part of this project was the engineering side of it. I might be an okay designer, but I barely knew anything about how to write software, let alone how to manage memory during a session. At the beginning, sessions kept freezing halfway through because they ran out of memory. Later, I found that designing independent sub-agents reduced memory pressure, and that clearing memory at certain checkpoints was essential.

For more about Distill Agent, check out my [GitHub page](https://github.com/dewei-lin/distill-agent) and the [online demo](https://distill-agent.up.railway.app/).
