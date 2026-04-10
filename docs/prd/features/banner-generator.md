---
title: Banner Generator
version: "1.0"
status: draft
last_updated: 2026-04-10
stakeholder: brother (store owner)
---

## Summary

Core feature of the product. A step-by-step wizard guides the store owner through banner creation. Each step collects one aspect of the banner — image, text, style, size, special instructions. The collected inputs are assembled into a generation prompt that produces a professional marketplace-ready banner.

## User Stories

- **BANNER-US-001:** As a store owner, I want to provide a product image (via URL or upload), so that the banner features my actual product photo.

- **BANNER-US-002:** As a store owner, I want to enter the product name and key specs in a dedicated step, so that the banner text is accurate and focused.

- **BANNER-US-003:** As a store owner, I want to choose a visual style (dark/light, minimal/photorealistic, etc.), so that the banner matches the mood I want without needing design knowledge.

- **BANNER-US-004:** As a store owner, I want to select the target marketplace and banner size, so that the output fits the platform's requirements without manual resizing.

- **BANNER-US-005:** As a store owner, I want a free-form text field to specify additional requirements ("must show waterproof rating", "add a discount badge"), so that I can influence the final result beyond the preset options.

- **BANNER-US-006:** As a store owner, I want to see the generated banner and be able to re-generate or tweak inputs, so that I can iterate until I'm happy with the result.

- **BANNER-US-007:** As a store owner, I want to download the finished banner ready for upload, so that I don't need any post-processing.

## Business Rules

### Wizard Flow

- **BANNER-BR-001** (serves all): The creation flow is a multi-step wizard with the following steps in order:
  1. **Product image** — URL paste or file upload
  2. **Product text** — name (required), key specs (optional), price (optional)
  3. **Style** — visual direction: dark/light theme, minimal/photorealistic aesthetic
  4. **Size & marketplace** — target platform (Ozon, Wildberries, Yandex Market, Mega Market); size is set automatically based on selection
  5. **Special instructions** — free-form text for anything not covered by the other steps

- **BANNER-BR-002** (serves all): The wizard must allow navigating back to any previous step to change inputs before generating.

- **BANNER-BR-003** (serves all): All wizard steps feed into a single generation prompt. The user never sees or edits the prompt directly.

### Image Input

- **BANNER-BR-004** (serves BANNER-US-001): The system must accept both a URL (fetched server-side) and a direct file upload. If a URL is unreachable or the image is invalid, show a clear error in Russian.

- **BANNER-BR-005** (serves BANNER-US-001): The product photo must be the visual focal point of the banner. Photos are sourced with clean/transparent backgrounds — no automatic background removal needed.

### Style & Size

- **BANNER-BR-006** (serves BANNER-US-003): Style options are presented as visual presets the user can tap, not as text dropdowns. `[OPEN]` Exact list of style presets to offer (dark/light is confirmed; need to define the full set).

- **BANNER-BR-007** (serves BANNER-US-004): Supported marketplaces: Ozon, Wildberries, Yandex Market, Mega Market. The system maps each marketplace to its required image dimensions automatically.

### Generation & Output

- **BANNER-BR-008** (serves BANNER-US-006): After generation, the user can re-generate (same inputs, new result), tweak any wizard step and re-generate, or download.

- **BANNER-BR-009** (serves BANNER-US-007): Downloaded banner must be in the exact format and dimensions required by the selected marketplace, ready for direct upload.

## Open Questions

- `[OPEN]` **Style preset list:** Dark/light is confirmed. What other style dimensions? Minimal vs. bold? Monochrome vs. colorful? Need to define the full set of visual presets for the style step. (BANNER-BR-006)

- `[OPEN]` **Multiple banners per product:** Marketplace listings often allow multiple images (e.g., Ozon supports up to 15). Does the stakeholder need to generate a set of banners per product (front, specs, features), or is one hero banner enough for MVP?

- `[OPEN]` **Generation technology:** What powers the banner generation — AI image generation (e.g., diffusion model), HTML/CSS template rendering, or a hybrid approach? This affects what "re-generate" means and how much variation the user gets.
