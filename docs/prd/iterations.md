# Iteration Plan

Generated from PRDs on 2026-04-10. Re-run the iteration planner when PRDs change.

## Iteration 1 — Product image input

### What we're building
The first step of the banner wizard: a screen where the user pastes a product image URL or uploads a file directly. The system validates the URL/file and displays a preview of the uploaded image. If the URL is unreachable or the image is invalid, an error message is shown in Russian.

### User stories
- BANNER-US-001: Provide a product image via URL or file upload for use in the banner

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003, BANNER-BR-004, BANNER-BR-005

### Acceptance criteria
- User can paste a URL and see the fetched image displayed as a preview
- User can upload an image file from their device and see it displayed as a preview
- If a URL is unreachable or the file is not a valid image, a clear error message appears in Russian
- The uploaded/fetched image is stored and carried forward for later steps

### Stakeholder message
> Hey, we built the first step of the banner creator — product image input. You can test it like this: open the app, paste a URL to one of your product photos (or upload a file from your phone/computer), and check that the image shows up correctly. Try an invalid URL too and see if the error message makes sense. What do you think?

---

## Iteration 2 — Product text entry

### What we're building
The second wizard step: a form where the user enters the product name (required), key specs (optional), and price (optional). The step enforces that the product name is filled in before the user can proceed.

### User stories
- BANNER-US-002: Enter product name and key specs so banner text is accurate

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003

### Acceptance criteria
- User sees a form with fields for product name, key specs, and price
- Product name is required — the user cannot proceed without filling it in
- Key specs and price fields are optional
- User can navigate back to the image step and return without losing entered text

### Stakeholder message
> Hey, we built the product text step of the banner creator. You can test it like this: after uploading your product image, you'll see a new screen asking for the product name, specs, and price. Fill in a real product's details, try leaving the name empty to see the validation, and go back to the image step and forward again to make sure nothing is lost. What do you think?

---

## Iteration 3 — Marketplace and size selection

### What we're building
The fourth wizard step: a selection screen where the user picks the target marketplace (Ozon, Wildberries, Yandex Market, or Mega Market). The system automatically determines the correct banner dimensions based on the selected marketplace.

### User stories
- BANNER-US-004: Select target marketplace and banner size so the output fits platform requirements

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003, BANNER-BR-007

### Acceptance criteria
- User can select from exactly four marketplaces: Ozon, Wildberries, Yandex Market, Mega Market
- Selecting a marketplace automatically shows the corresponding banner dimensions
- User can navigate back to previous steps and return without losing their selection
- Only one marketplace can be selected at a time

### Stakeholder message
> Hey, we built the marketplace selection step. You can test it like this: after filling in your product details, pick the marketplace where you sell (Ozon, Wildberries, etc.) and see the banner size that gets set automatically. Try switching between marketplaces and check that the sizes look right for each one. What do you think?

---

## Iteration 4 — Special instructions input

### What we're building
The fifth wizard step: a free-form text field where the user can type additional requirements for the banner that aren't covered by the other steps — things like "show the waterproof rating" or "add a discount badge."

### User stories
- BANNER-US-005: Provide free-form additional requirements to influence the final banner

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003

### Acceptance criteria
- User sees a free-form text field for entering special instructions
- The field is optional — the user can proceed without entering anything
- User can navigate back to previous steps and return without losing their text
- Entered text is preserved and carried forward for generation

### Stakeholder message
> Hey, we built the special instructions step. You can test it like this: after picking your marketplace, you'll see a text box where you can type anything extra you want on the banner — like "add a discount badge" or "highlight waterproof rating." Try typing something, go back a step, and come forward again to make sure your text is still there. What do you think?

---

## Iteration 5 — Banner generation and iteration

### What we're building
The generation screen: after completing the wizard, the user triggers banner generation. The system assembles all inputs (image, text, marketplace/size, special instructions) into a generation prompt and produces a banner preview. The user can re-generate with the same inputs for a different result, or go back to tweak any wizard step and re-generate.

### User stories
- BANNER-US-006: See the generated banner and re-generate or tweak inputs to iterate on the result

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003, BANNER-BR-008

### Acceptance criteria
- User can trigger generation after completing all wizard steps
- A banner preview is displayed after generation
- User can re-generate with the same inputs and get a different result
- User can navigate back to any wizard step, change inputs, and re-generate
- The generation prompt is assembled automatically — the user never sees or edits it directly

### Stakeholder message
> Hey, we built the banner generation step. You can test it like this: go through all the wizard steps with a real product, then hit generate and see the banner it creates. Try hitting re-generate to get a different version with the same inputs. Then go back, change the product name or marketplace, and generate again. What do you think of the results?

---

## Iteration 6 — Banner download

### What we're building
A download action on the generation screen. After the user is happy with the generated banner, they can download it in the exact format and dimensions required by their selected marketplace — ready for direct upload, no post-processing needed.

### User stories
- BANNER-US-007: Download the finished banner ready for marketplace upload

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003, BANNER-BR-009

### Acceptance criteria
- User can download the generated banner with a single action
- The downloaded file is in the format required by the selected marketplace
- The downloaded file has the exact pixel dimensions required by the selected marketplace
- The file can be uploaded directly to the marketplace listing without resizing or conversion

### Stakeholder message
> Hey, we built the download feature. You can test it like this: generate a banner for one of your products, then hit the download button. Try uploading the downloaded file to your marketplace listing (Ozon, Wildberries, etc.) and check that it fits perfectly — no resizing or editing needed. What do you think?

---

## Blocked

Stories that can't be planned yet because of unresolved open questions.

| Story | Blocked by | Open question |
|-------|-----------|---------------|
| BANNER-US-003: Choose a visual style for the banner | BANNER-BR-006 | Style preset list is incomplete — dark/light is confirmed but the full set of visual presets (minimal vs. bold, monochrome vs. colorful, etc.) needs to be defined before this step can be built. |

Nothing to do here — stakeholder is working on clarifying these.
