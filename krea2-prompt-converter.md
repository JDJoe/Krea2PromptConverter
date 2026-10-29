---
name: krea2-prompt-converter
description: "Convert legacy Stable Diffusion, Pony, or Illustrious tag prompts into clear natural-language prompts for Krea 2. Use when the user pastes a quality-tag prompt, score tags, or weighted tags and wants a clean Krea 2 version."
type: workflow
lifecycle: active
metadata:
  type: workflow
  version: "1.0"
---

# Krea 2 Prompt Converter

Convert legacy Stable Diffusion, Pony, and Illustrious tag prompts into clear natural-language prompts for Krea 2.

The goal is **faithful conversion, not creative rewriting**. Preserve the original prompt's meaningful visual intent, remove redundant or model-specific noise, and express the remaining details in clear prose. Do not redesign the scene unless the user asks you to.

## Core conversion rules

### 1. Preserve the visual intent

Keep every meaningful detail that affects what should appear in the image, when it can be expressed coherently:

- Subject and visible identity features
- Body type and anatomy
- Hair, expression, pose, gesture, and gaze
- Clothing, accessories, and distinct materials
- Setting, objects, and background
- Composition, framing, viewpoint, and camera angle
- Lighting, atmosphere, and intended photographic or artistic treatment
- Action and relationships between people and objects

Do not flatten distinctive details into generic descriptions. For example, "large, heavy, naturally sagging breasts" contains more visual information than simply "large breasts." Preserve the intended meaning without adding new anatomy or changing the description.

### 2. Remove legacy prompt noise

Remove or consolidate generic quality claims and model-specific tags that do not describe a distinct visible feature. Examples include:

- `masterpiece`, `best quality`, `amazing quality`
- `4K`, `8K`, `ultra detailed`, `extremely detailed`
- `score_9` and similar score tags
- Repeated synonyms such as `beautiful`, `gorgeous`, `perfect beauty`, `breathtaking`
- Weighting syntax such as `(tag:1.2)`, `((tag))`, `[[tag]]`, and similar notation

Do not remove a phrase merely because it is emphatic. If it communicates a concrete visual quality—such as a very large garment, a strongly wrinkled nose, or a heavily weathered surface—preserve that information in plain language.

Treat words such as `photorealistic`, `anime`, `oil painting`, or `watercolor` according to context. They may be redundant quality claims in one prompt, but may express an important visual medium or style in another. Preserve the intended medium or style when it is meaningful.

### 3. Do not invent details

Do not add a setting, prop, pose, expression, gaze, background, clothing item, camera effect, or artistic style that the source does not specify.

Avoid embellishments such as "captivating expression," "refined facial features," "cinematic atmosphere," or "softly rendered background" unless the original prompt supports them.

Make only the small inferences needed to turn tags into grammatical prose. If a missing detail is not essential, leave it unspecified. If the source contains a major contradiction that cannot be resolved confidently, briefly flag it instead of silently choosing a new interpretation.

### 4. Resolve repetition and contradictions carefully

Combine duplicate or overlapping tags without losing meaningful distinctions. For example, `short blond hair, pixie haircut, undercut hair, shaved sides` can become "short blond pixie hair with closely shaved sides."

When tags conflict, do not blindly include both and do not invent a compromise. Preserve the clearest apparent intent if the resolution is obvious. If the conflict could materially change the image and the intent is unclear, ask a concise clarification or mention the conflict.

### 5. Write clear natural language

- Follow the section order in "Prompt structure" below: STYLE first, then CHARACTERS, LOCATION, CAMERA, MOOD.
- Inside each section, write short coherent sentences, not tag lists.
- Turn tag lists into descriptions of visible objects, features, materials, and relationships.
- Prefer concrete wording over generic praise: "oxblood leather" is more informative than "beautiful red material."
- Describe the actual lighting when specified: for example, "soft light from the left."
- Keep the original photographic, illustrative, or stylized intent.
- Do not add ornate prose just to make the prompt sound polished.

### 6. Name characters with easy tags, never pronouns

Give each character a short capitalised tag that is easy to tell apart and easy to debug:

- Role plus a number when the roles differ: `WOMAN1`, `MAN1`, `DOG1`.
- The most visible feature when two characters share a role: `REDHEAD` and `BLONDY`, not `WOMAN1` and `WOMAN2`.

Describe each character fully once with the tag ("WOMAN1 is a woman with long silver hair…"), then use only the tag ("WOMAN1 looks…", "WOMAN1's hand…"). Never use she, her, he, his, it or they for a character. A tag on its own renders nothing; it works only after the full description.

Objects are never tagged like characters. Describe them with plain nouns: "a red rug", "the window", not "The Rug".

### 7. Translate camera and composition tags into what the camera sees

Describe the framing and viewpoint in concrete terms when the source provides them:

- `close-up` → close portrait framing
- `full body`, `full shot`, `uncropped` → the entire figure visible from head to toe
- `low angle` → camera below the subject, looking upward
- `high angle` → camera above the subject, looking downward
- `profile` → the subject seen from the side
- `looking at viewer` → the subject looks directly into the camera

Use only the details supported by the source. Do not turn a generic full-body tag into a new pose or location.

Remember: the camera is a viewpoint, not a person. State where the lens is and what it sees. Do not invent an observer or another character to explain a camera angle.

### 8. Describe materials as part of visible objects

A list such as `silk, sheer, lace, leather, fur, feathers, stockings, transparent robe` should be reorganized into a coherent description of the garment and accessories when the source makes their relationships clear.

Do not assume every material belongs to the same item. If the source does not say whether feathers are trim, a separate accessory, or part of a garment, choose neutral wording rather than inventing a specific construction.

### 9. Keep the prompt focused

There is no universal word-count limit that guarantees a good result. As a practical starting point, remove repetition and aim for a concise prompt in which each sentence contributes useful visual information. Short prompts can work well for exploration; more controlled scenes may need more detail.

Do not cut important details merely to hit a word target. Stop adding text when it repeats existing information or introduces unsupported ideas.

### 10. Treat negative prompts cautiously

Translate useful positive intent into a description of what should be visible. Avoid long lists of things that should not appear. Do not promise that negative wording will reliably suppress unwanted elements.

If a source prompt relies on a specific exclusion, preserve it only when it is important to the user's intent, and phrase it as clearly and briefly as possible.

## Conversion example

**Input (legacy tag prompt)**

`4K, best quality, masterpiece, photorealistic, realistic picture, extremely detailed, dynamic, great lighting, 1girl, long silver hair, looking at viewer, ornate dress, score_9, (masterpiece:1.2)`

**Output (Krea 2)**

```text
STYLE: Photorealistic photograph.
CHARACTERS: WOMAN1 is a woman with long silver hair wearing an ornate dress. WOMAN1 looks directly into the camera.
```

The output removes the quality and score tags and keeps the medium, hair, dress and gaze. The source gives no setting, lighting or framing, so LOCATION, CAMERA and MOOD are left out rather than invented.

## Second example: preserve the details

**Input (legacy tag prompt)**

`best quality, 1girl, short blond pixie haircut, undercut, skinny body, fit body, long transparent robe, silk, sheer, lace, leather, fur, feathers, stockings, side soft light, full shot, uncropped`

**Output (Krea 2)**

```text
CHARACTERS: WOMAN1 is a slim, athletic woman with short blond pixie hair and closely shaved sides, wearing a long, transparent robe of sheer silk and lace, with leather accents, fur and feather details, and stockings.
LOCATION: Soft light from the side.
CAMERA: Full-length framing, WOMAN1's entire figure visible from head to toe.
```

This version combines overlapping tags but does not invent a pose, expression, location, style or additional clothing. "best quality" is noise, so there is no STYLE line.

## Prompt structure

Write the prompt with these labels, in this order, one per line. Leave out a label when the source says nothing about it; never fill it with "unspecified" or a guess.

```text
STYLE:      [one medium + technique; LoRA trigger if any]
CHARACTERS: [tag + full visual once, then tag only; clothing; pose; hands; gaze]
LOCATION:   [place, key objects as plain nouns, light source and direction]
CAMERA:     [shot size, height, position, what is near the lens, framing]
MOOD:       [one short visible mood line]
```

## Multi-image sequences and references

Use this section only when the user is building a sequence or supplying reference images.

- Prompts for separate images may be treated independently. Repeat the outfit, setting, props, and identity details needed in each image.
- Keep permanent character identity separate from temporary clothing, pose, and scene details when the workflow supports that distinction.
- When using a reference image, state exactly which identity features should remain consistent and which aspects should change.
- Avoid broad instructions such as "keep everything exactly" when the pose, clothing, or setting needs to change.
- If a new pose or camera angle is required, state that explicitly.

## Output behavior

By default, return only the converted prompt, ready to copy and paste into Krea 2.

Do not include:
- A preamble or explanation
- A list of removed tags
- Multiple alternatives
- Ratings or a score
- Unrequested creative suggestions

If a significant contradiction prevents a faithful conversion, ask one concise question or add a short note only when needed.

## Final checklist

- [ ] Meaningful visual details from the source are preserved.
- [ ] Redundant quality tags, score tags, and weighting syntax are removed.
- [ ] The intended medium or style is preserved when meaningful.
- [ ] No unsupported pose, expression, prop, setting, or effect has been added.
- [ ] Repeated details are consolidated without losing distinctions.
- [ ] Contradictions are handled conservatively.
- [ ] Camera, framing, materials, and lighting are clear where specified.
- [ ] Sections follow STYLE, CHARACTERS, LOCATION, CAMERA, MOOD, with empty ones left out.
- [ ] Every character has an easy tag, is described once, and is never referred to by a pronoun.
- [ ] The result reads naturally and is easy to paste into Krea 2.
- [ ] The prompt is concise without sacrificing important information.

Use this skill to convert legacy prompts faithfully. Improve clarity, not the user's idea.
