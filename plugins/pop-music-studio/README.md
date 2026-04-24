# pop-music-studio

`pop-music-studio` is a creative pipeline plugin for building pop music projects from the first lyrical brief through song composition, arrangement, vocal direction, release artwork, and MV planning.

## Overview

This plugin is built around a stage-based pipeline:

- one routing skill decides which stage or combination of stages is needed
- one shared rules file keeps the project brief, audience, and emotional arc consistent
- one specialized skill and one template define each pipeline stage

The goal of version 1 is not to generate final audio or rendered video files directly. It is to make the upstream creative package structured enough that each step can hand off cleanly to the next one.

## What It Includes

- 1 routing skill: `music-pipeline-router`
- 6 specialized skills:
  - `lyrics-writing`
  - `song-composition`
  - `arrangement-production`
  - `vocal-performance`
  - `cover-art-design`
  - `mv-generation`
- 1 shared rules file: `references/shared-pipeline-rules.md`
- 6 standalone templates in `templates/`
- Codex metadata: `.codex-plugin/plugin.json`
- Claude metadata: `.claude-plugin/plugin.json`

## Pipeline Stages

### 1. Lyrics Writing

Use `lyrics-writing` for requests mainly about:

- lyrical concept
- title and hook candidates
- verse, pre-chorus, chorus, bridge drafting
- rhyme, phrasing, and singability
- alternate chorus or hook options

### 2. Song Composition

Use `song-composition` for requests mainly about:

- song structure
- topline and melody direction
- chord movement
- tempo, key, and energy arc
- hook architecture and section mapping

### 3. Arrangement Production

Use `arrangement-production` for requests mainly about:

- instrumentation
- groove and rhythmic feel
- section builds and drops
- synth, guitar, drum, and texture planning
- producer handoff notes

### 4. Vocal Performance

Use `vocal-performance` for requests mainly about:

- singer persona and tone
- lead delivery per section
- harmony and ad-lib plans
- recording and processing notes
- retake or punch-in priorities

### 5. Cover Art Design

Use `cover-art-design` for requests mainly about:

- release artwork concepts
- visual moodboards
- typography and layout direction
- image-generation prompts
- asset checklists for single or EP covers

### 6. MV Generation

Use `mv-generation` for requests mainly about:

- music video concepting
- narrative beats
- scene structure and shot lists
- performance and styling direction
- editing rhythm and final delivery specs

## Routing Rules

Start with `music-pipeline-router`.

The router should:

- route stage-specific requests to the matching stage skill
- split broad requests into pipeline stages instead of collapsing everything into one answer
- preserve handoff artifacts from one stage to the next
- keep the same project brief, target audience, and emotional direction across all stages

If a request asks for the full creation flow, the expected stage order is:

1. `lyrics-writing`
2. `song-composition`
3. `arrangement-production`
4. `vocal-performance`
5. `cover-art-design`
6. `mv-generation`

## Shared Rules

All stages reuse `references/shared-pipeline-rules.md`.

The shared rules enforce:

- one source of truth for project brief and audience
- explicit stage inputs, outputs, and handoffs
- consistent emotional and visual motifs across music and visuals
- structured deliverables instead of loose brainstorming only
- explicit risks, assumptions, and open questions

Stage-specific templates still control the final section order and detail level.

## Usage Flow

1. Start with the routing skill `music-pipeline-router`.
2. Let the router determine whether the request belongs to a single stage or a multi-stage package.
3. Apply `references/shared-pipeline-rules.md`.
4. Read the matching stage skill and template.
5. Generate the output in a way that can be passed to the next stage.

## Example Prompts

- `Write a pop lyric brief and chorus draft for a late-night city romance single.`
- `Turn this lyric concept into a song structure, topline direction, and chord plan.`
- `Design a modern dance-pop arrangement around 122 BPM with a bright festival chorus.`
- `Create vocal direction, harmonies, and ad-libs for this hook-heavy female lead track.`
- `Plan cover art and image prompts for a nostalgic summer synth-pop single.`
- `Write an MV concept, scene beats, and shot list for this breakup anthem.`
- `Take this idea through all six stages and keep the concept consistent.`

## Version 1 Scope

This plugin currently standardizes:

- lyric development
- song structure and topline planning
- arrangement and production direction
- vocal direction packages
- cover art planning
- MV concept and shot planning

It does not yet cover:

- final DAW session generation
- direct stem export
- direct singing audio synthesis
- rendered cover images
- rendered MV files

## Extension Model

To add a new stage later:

1. Add a new template under `templates/`.
2. Add a specialized skill under `skills/`.
3. Extend the routing rules in `skills/music-pipeline-router/SKILL.md`.
4. Reuse `references/shared-pipeline-rules.md` instead of duplicating common rules.

The plugin should stay stage-oriented unless real usage shows the handoffs need a different workflow.
