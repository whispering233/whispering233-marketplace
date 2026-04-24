---
name: music-pipeline-router
description: Use when a user wants a pop music creation output and needs it routed to lyrics, song composition, arrangement, vocals, cover art, MV, or a full multi-stage pipeline.
---

# Music Pipeline Router

## Overview

Use this skill as the entry point for pop music creation requests that need a structured stage output instead of one blended response.

Always read `../../references/shared-pipeline-rules.md` first. Then route to the correct specialized skill.

## Routing Rules

Route to `lyrics-writing` when the request is mainly about:

- lyrical concept
- title exploration
- hook lines
- verse, chorus, bridge drafting
- phrasing and singability

Route to `song-composition` when the request is mainly about:

- melody or topline direction
- song structure
- chord movement
- BPM, key, and section energy
- hook architecture

Route to `arrangement-production` when the request is mainly about:

- instrumentation
- groove
- production palette
- section layering
- transitions and drops

Route to `vocal-performance` when the request is mainly about:

- singer persona
- lead delivery
- harmony design
- ad-libs
- recording or processing direction

Route to `cover-art-design` when the request is mainly about:

- cover concepts
- visual direction
- artwork prompts
- palette, styling, or typography

Route to `mv-generation` when the request is mainly about:

- MV concepts
- storylines
- scene structure
- shot lists
- edit rhythm and visual pacing

If the request clearly includes multiple stages:

- keep the output split by pipeline stage
- preserve handoff artifacts between stages
- keep the same brief, audience, and emotional target throughout
- use this default order unless the user requests a different one:
  1. `lyrics-writing`
  2. `song-composition`
  3. `arrangement-production`
  4. `vocal-performance`
  5. `cover-art-design`
  6. `mv-generation`

## Required Behavior

- enforce stage-based output
- preserve project-level consistency
- surface assumptions instead of hiding missing context
- prefer the specialized skill over writing everything here
- keep outputs usable as handoff artifacts for the next stage

## Output Contract

Before finalizing the result:

1. identify the requested stage scope
2. apply `../../references/shared-pipeline-rules.md`
3. apply the matching stage template
4. make sure each stage lists inputs, outputs, and next handoff notes

## Do Not

- duplicate all stage templates here
- collapse a full pipeline request into one loose block of prose
- invent final audio, image, or video files when the request only needs planning assets
- break consistency between lyrical, musical, and visual directions without explanation
