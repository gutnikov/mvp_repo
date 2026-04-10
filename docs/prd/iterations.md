# Iteration Plan

Generated from PRDs on 2026-04-10. Re-run the iteration planner when PRDs change.

## Iteration 1 — Banner creation wizard with generation and preview

### What we're building
A step-by-step wizard that collects a product image (URL or upload), product name and specs, target marketplace and size, and optional special instructions — then generates a banner and shows it to the user. The style step is omitted for now (blocked by an open question on style presets), so generation uses a sensible default style. The user can navigate back through wizard steps, tweak inputs, and re-generate until satisfied.

### User stories
- BANNER-US-001: Provide a product image via URL or file upload
- BANNER-US-002: Enter product name and key specs
- BANNER-US-004: Select target marketplace and banner size
- BANNER-US-005: Add free-form special instructions
- BANNER-US-006: See the generated banner and re-generate or tweak inputs

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003, BANNER-BR-004, BANNER-BR-005, BANNER-BR-007, BANNER-BR-008

### Acceptance criteria
- User can paste a product image URL and see it loaded in the wizard; an error in Russian appears if the URL is unreachable or the image is invalid
- User can upload a product image file directly instead of a URL
- User can enter a product name (required) and optionally key specs and price
- User can select a marketplace (Ozon, Wildberries, Yandex Market, Mega Market) and the banner size is set automatically
- User can type free-form special instructions in a text field
- User can navigate back to any previous wizard step and change inputs before generating
- User can trigger banner generation and see the result displayed on screen
- User can re-generate with the same inputs and get a new result
- User can tweak any wizard step and re-generate
- The product photo is the visual focal point of the generated banner

### Stakeholder message
> Hey, we built the banner creation wizard. You can paste a product image URL (or upload a photo), type in the product name and specs, pick your marketplace, add any special notes, and hit generate to see your banner. If you don't like it, change anything and regenerate. Style selection isn't in yet — we're waiting on your input for the full list of style presets. Give it a try and let us know what you think!

---

## Iteration 2 — Banner download in marketplace-ready format

### What we're building
A download action on the generation result screen. Once the user is happy with their generated banner, they can download it as a file that is ready for direct upload to the selected marketplace — correct format, correct dimensions, no post-processing needed.

### User stories
- BANNER-US-007: Download the finished banner ready for marketplace upload

### Business rules
- BANNER-BR-009

### Acceptance criteria
- User can click a download button after generation to save the banner as a file
- The downloaded file matches the exact format required by the selected marketplace
- The downloaded file matches the exact pixel dimensions required by the selected marketplace
- The downloaded file can be uploaded directly to the marketplace listing without any editing or resizing

### Stakeholder message
> Hey, we added banner download. After you generate a banner you're happy with, hit the download button and you'll get a file that's ready to upload straight to your marketplace listing — correct size, correct format, no extra steps. Try generating a banner for Ozon and downloading it, then check if it uploads cleanly.

---

## Blocked

Stories that can't be planned yet because of unresolved open questions.

| Story | Blocked by | Open question |
|-------|-----------|---------------|
| BANNER-US-003: Choose a visual style for the banner | BANNER-BR-006 | The full list of style presets beyond dark/light needs to be defined — minimal vs. bold? Monochrome vs. colorful? |

Nothing to do here — stakeholder is working on clarifying these.
