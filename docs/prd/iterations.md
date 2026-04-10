# Iteration Plan

Generated from PRDs on 2026-04-10. Re-run the iteration planner when PRDs change.

## Iteration 1 — Product image input

### What we're building
A screen where the user provides their product photo, either by pasting a URL or uploading a file directly. The system validates the input (reachable URL, valid image format) and displays a preview. This is the first wizard step and the foundation everything else builds on.

### User stories
- BANNER-US-001: Provide a product image via URL or file upload for use in the banner

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003, BANNER-BR-004, BANNER-BR-005

### Acceptance criteria
- User can paste a product image URL and see a preview of the loaded image
- User can upload an image file from their device and see a preview
- If the URL is unreachable or the file is not a valid image, a clear error message is shown in Russian
- User can replace the image by pasting a new URL or uploading a different file

### Stakeholder summary
> Пользователь может загрузить фото товара — вставить ссылку или выбрать файл с устройства — и сразу увидеть превью загруженного изображения.

---

## Iteration 2 — Product text entry

### What we're building
A wizard step where the user enters the product name (required), key specs (optional), and price (optional). This is the second step of the creation flow. The entered text will later feed into banner generation.

### User stories
- BANNER-US-002: Enter product name and key specs so the banner text is accurate

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003

### Acceptance criteria
- User can enter a product name (required field — cannot proceed without it)
- User can optionally enter key specs and price
- User can navigate back to the image step and return without losing entered text
- Entered text is preserved when navigating between wizard steps

### Stakeholder summary
> Пользователь может ввести название товара, характеристики и цену, чтобы эта информация появилась на баннере.

---

## Iteration 3 — Marketplace and size selection

### What we're building
A wizard step where the user picks the target marketplace (Ozon, Wildberries, Yandex Market, Mega Market). The system automatically sets the banner dimensions based on the selected marketplace. No manual size entry needed.

### User stories
- BANNER-US-004: Select target marketplace and get correctly sized output automatically

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003, BANNER-BR-007

### Acceptance criteria
- User can select one of: Ozon, Wildberries, Yandex Market, Mega Market
- The selected marketplace determines the banner dimensions automatically
- User can change the marketplace selection and the dimensions update accordingly
- User can navigate back to previous steps and return without losing the selection

### Stakeholder summary
> Пользователь может выбрать маркетплейс, и система автоматически подберёт нужный размер баннера для этой площадки.

---

## Iteration 4 — Special instructions

### What we're building
A free-form text field as the final wizard step before generation. The user can type any additional requirements — "show waterproof rating", "add discount badge", etc. This step is optional; the user can leave it empty and proceed.

### User stories
- BANNER-US-005: Add free-form special instructions to influence the banner result

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003

### Acceptance criteria
- User can enter free-form text describing additional banner requirements
- The field is optional — user can proceed without entering anything
- User can navigate back to previous steps and return without losing the text
- Entered text is preserved across wizard navigation

### Stakeholder summary
> Пользователь может написать дополнительные пожелания к баннеру в свободной форме — например, «добавить значок скидки» или «указать водонепроницаемость».

---

## Iteration 5 — Banner generation and iteration

### What we're building
The generation step: the system assembles all wizard inputs into a generation prompt (invisible to the user), produces a banner, and displays it. The user can re-generate (same inputs, new result), go back and tweak any wizard step, then re-generate again. This is the core value loop.

### User stories
- BANNER-US-006: See the generated banner and be able to re-generate or tweak inputs

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003, BANNER-BR-008

### Acceptance criteria
- User can trigger banner generation after completing the wizard steps
- Generated banner is displayed as a preview
- User can re-generate with the same inputs and get a different result
- User can navigate back to any wizard step, change inputs, and re-generate
- The user never sees or edits the generation prompt directly

### Stakeholder summary
> Пользователь может сгенерировать баннер, посмотреть результат, и если что-то не нравится — перегенерировать или изменить данные и попробовать снова.

---

## Iteration 6 — Banner download

### What we're building
A download action on the generation result screen. The user downloads the finished banner in the exact format and dimensions required by the marketplace they selected earlier. The file is ready for direct upload to the marketplace — no post-processing needed.

### User stories
- BANNER-US-007: Download the finished banner ready for marketplace upload

### Business rules
- BANNER-BR-001, BANNER-BR-002, BANNER-BR-003, BANNER-BR-009

### Acceptance criteria
- User can download the generated banner with a single action
- Downloaded file is in the format required by the selected marketplace
- Downloaded file has the exact dimensions required by the selected marketplace
- The file is usable as-is for marketplace upload without any editing or conversion

### Stakeholder summary
> Пользователь может скачать готовый баннер, который сразу подходит для загрузки на выбранный маркетплейс — без дополнительной обработки.

---

## Blocked

Stories that can't be planned yet because of unresolved open questions.

| Story | Blocked by | Open question |
|-------|-----------|---------------|
| BANNER-US-003: Choose a visual style for the banner | BANNER-BR-006 | Exact list of style presets to offer is undefined. Dark/light is confirmed, but the full set of visual dimensions (minimal vs. bold, monochrome vs. colorful, etc.) needs to be decided. |

Nothing to do here — stakeholder is working on clarifying these.
