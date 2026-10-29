# Krea 2 Prompt Converter

A short skill document that turns legacy Stable Diffusion / Pony / Illustrious tag prompts into clean natural-language prompts that Krea 2 actually follows.

# Why use it

 Krea 2 ignores quality tags, score tags, and weight syntax. Those prompts waste the useful budget and often produce weaker results. This converter strips the noise while preserving the original visual intent.

# Which file to use

- **krea2-prompt-converter.md** (long version): a skill or system prompt for big chat models such as Grok, Claude or ChatGPT. It has the full rules, edge cases and a final checklist.
- **krea2-prompt-converter-compact.md** (compact version): paste the whole file as-is into the RULES (system prompt) node of a ComfyUI workflow, so a small local model such as Qwen3-VL-4B can fix prompts before they reach Krea 2.

Both write prompts in the same section order: STYLE, CHARACTERS, LOCATION, CAMERA, MOOD.

# How to use it

 Paste the document as a system prompt (or skill) for an LLM, or the compact file into the ComfyUI RULES node, then give it a legacy or messy prompt. It returns a single ready-to-paste Krea 2 prompt. No explanations, no alternatives.
