# SixthGear Docs style guide

## Voice

- Write directly to the reader using "you."
- Use active voice.
- Sound like an experienced colleague explaining the system.
- Stay professional, calm, practical, and specific.
- Explain a technical term when a non-developer may encounter it.
- Use the exact interface labels shown in Shopify, Sanity, Vercel, GitHub, and the SixthGear website.

## Avoid AI-style writing

Do not use: "seamlessly," "effortlessly," "robust," "powerful solution," "leverage," "unlock," "delve into," "navigate the complexities," "comprehensive suite," "it is important to note," "in order to," "simply," "as mentioned earlier," "in conclusion," "whether you are," or "this documentation aims to."

Do not praise ordinary functionality, repeat the introduction in a conclusion, add generic summaries, use vague marketing language, or add text only to make a page longer.

## Structure

- Use sentence case for headings.
- Do not add a manual H1 inside an MDX page. The frontmatter title supplies it.
- Keep most sentences below 25 words.
- Keep paragraphs between two and four sentences.
- Cover one main task or reference subject per page.
- Use numbered steps for procedures and tables for reference material.
- Use code blocks only for exact commands, syntax, or structures readers need.
- Put paths, commands, variables, file names, and identifiers in backticks.
- Bold exact interface labels such as **Products**, **Save**, and **Settings**.

## Page types

### How-to guide

Include purpose, prerequisites, steps, verification, common problems, and related tasks.

### Reference

Prioritize exact fields, ownership, dependencies, defaults, statuses, and edge cases in tables.

### Explanation

Explain how and why the system works. Do not turn architecture or caching pages into broad tutorials.

## Components

Use `<Steps>` for sequences, `<Warning>` for destructive or sensitive actions, `<Tip>` for practical shortcuts, and `<Info>` for definitions. Use cards only for real navigation and Mermaid only when relationships are clearer visually.

## Plain language for store staff

Pages under **At the Counter**, **Products and Stock**, and **Online Orders** are read by cashiers, baristas, and store staff. For these pages:

- Write so a new staff member can follow the page on their first day.
- Do not use em dashes. Use a period, a comma, or a colon instead.
- Do not use jargon. If a term is unavoidable, explain it in one short sentence the first time it appears.
- Use the same words the staff see on the screen, such as **Checkout**, **Cash**, and **QR PH**.
- Keep each step to one action.
- Say what the staff member should see after an important step, so they know it worked.

## Image and video placeholders

When a page needs a screenshot or a video that does not exist yet, add a placeholder in the exact spot where it belongs. Every placeholder names the file that should replace it, so the person uploading media knows where it goes.

Screenshot placeholder:

```mdx
<Frame caption="Screenshot needed: what the screen should show. Save as images/at-the-counter/page-name-1.png">
  <img src="/images/placeholders/screenshot-reserved.svg" alt="Placeholder for a screenshot of what the screen should show" />
</Frame>
```

Video placeholder:

```mdx
<Frame caption="Video needed: what the video should show. Save as videos/at-the-counter/page-name.mp4">
  <img src="/images/placeholders/screenshot-reserved.svg" alt="Placeholder for a video of what the video should show" />
</Frame>
```

File naming:

- Put files in a folder named after the page's section, such as `images/at-the-counter/` or `videos/at-the-counter/`.
- Name files after the page, then number screenshots in the order they appear: `take-payment-1.png`, `take-payment-2.png`.
- Record videos in landscape. Record tablet screens through screen mirroring so zoom effects can be added.

Replacing a placeholder:

- For a screenshot, change the `src` to the new file and update the `alt` text to describe the real image. Remove "Screenshot needed" from the caption.
- For a video, replace the whole `<Frame>` with `<Frame><video controls src="/videos/at-the-counter/page-name.mp4"></video></Frame>`.
