# Offer templates

Reusable Spec Kitty offer templates for German and English client proposals.

## Files

- [offer-template.de.md](offer-template.de.md) - German Markdown template.
- [offer-template.en.md](offer-template.en.md) - English Markdown template.
- [offer-template.html](offer-template.html) - print-ready branded HTML template for browser export to PDF.
- [angebot-letterhead-template.de.md](angebot-letterhead-template.de.md) - formal German offer source with recipient, scope, price, VAT, validity, and acceptance fields.
- [angebot-letterhead-template.de.html](angebot-letterhead-template.de.html) - formal German company-letterhead HTML template for PDF generation.

Generated PDF preview:

- `output/pdf/angebot-letterhead-template.de.pdf`

## Known business details

Use these consistently unless the legal or finance details change:

| Field | Value |
|---|---|
| Brand | Spec Kitty |
| Legal entity | Spec-Kitty.ai (Delaware, USA) |
| Contact person | Robert Douglass |
| General email | contact@spec-kitty.ai |
| Training email | training@spec-kitty.ai |
| Website | https://spec-kitty.ai |
| Docs | https://docs.spec-kitty.ai |
| GitHub | https://github.com/Priivacy-ai/spec-kitty |

The current website and design refs do not include a postal address or bank details. Keep those placeholders in the template until finance/legal provides canonical values.

## Generation workflow

1. For formal German offers, start from `angebot-letterhead-template.de.md` or `angebot-letterhead-template.de.html`.
2. For lighter proposals, copy the German or English Markdown template.
3. Replace all `{{...}}` placeholders.
4. Delete optional blocks that do not apply.
5. For a branded PDF, render the filled HTML in a browser or with Playwright and export to PDF.

## Brand rules

- Keep the tone concrete and operational.
- Use Spec Kitty as the product voice where possible.
- Avoid hype, emoji, and exclamation marks.
- Use `EUR` for prices in plain text. Use `€` only when the surrounding document style already uses symbols.
