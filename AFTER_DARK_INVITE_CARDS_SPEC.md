# After Dark invite cards: field spec for Hermes

Design source: `DateMeUp After Dark Invite Cards.dc.html` (templates 1a–1l). Dashed outlines on the board mark app-filled fields. Turn them off with the `showFields` tweak to see the clean card.

## Rule zero
**Nothing on the board is sample text except the fields below.** Sam, Priya, idea names, descriptions, days, times and plan items are all sample data. If a string is not in a template's "Fixed" list, it comes from data. Never bake a name, date, time, idea or venue into an image or template file.

## When a card is sent
- An After Dark idea is added to a couple plan. The card is sent with the main invite (Share Card) as a second page or image.
- Share sheet only. Never email (After Dark rule 11).
- One card per After Dark idea in the plan.

## Fields
| Field | Source | Format | Empty |
|---|---|---|---|
| `sender.firstName` | Proposer's display first name | As entered, max 20 chars, truncate with … | Never empty |
| `sender.initial` | First letter of `sender.firstName` | Uppercase, 1 char | Never empty |
| `partner.firstName` | The other member of the 2-person group | As entered, max 20 chars | Never empty |
| `idea.name` | Suggestion.name (seed or user idea) | As entered, ≤40 chars | Never empty |
| `idea.description` | Suggestion.description | As entered, ≤140 chars | Hide the line |
| `idea.where` | Suggestion.where | HOME → "At home" / "HOME"; OUT → "Out" / "OUT" | Hide the segment |
| `idea.duration` | Suggestion.duration | "2h", "90m", "1 night" | Hide the segment |
| `slot.day` | Day of the After Dark item, else the plan day | Short "Fri" or long "Friday" (per template), recipient's locale | Use plan day |
| `slot.time` | Time of the After Dark item | Recipient's locale/clock, "10:00 PM" | Hide the segment |
| `plan.items[]` | Main plan items (1j only), After Dark item excluded | Name + time, max 3 | Hide the row |

- Segments joined with " · " are built at render time. Drop empty segments with their separator, never leave "Fri · · 2h".
- "(uppercase)" in a template's list means CSS `text-transform` or `.toUpperCase()` at render, not stored data.
- User-written ideas render through the same fields. Run the existing report/block check before rendering.

## Per template
Each card on the board has a "Fields / Fixed" caption underneath. Summary:

| ID | Fields | Fixed copy | Notes |
|---|---|---|---|
| 1a Velvet | sender, slot.day (long), idea.name, idea.description, day·time, where·duration | "🌙 AFTER DARK", "added one more idea for", "Yes, Maybe or Pass in DateMeUp" | |
| 1b Moon phases | day·time, idea.name, idea.description, sender, partner | "From …, for …", "🌙 AFTER DARK · DATEMEUP" | |
| 1c Love letter | idea.name, partner (salutation), idea.description, sender (sign-off), day·time·where | ",", " x", seal | Description is the letter body |
| 1d Admit two | idea.name, idea.description, day, time, where, sender, partner | "ADMIT TWO", DAY/TIME/WHERE, "Yes, Maybe or Pass" | No time → "ANY" |
| 1e Neon | idea.name, idea.description, day·time·duration, sender | "OPEN LATE", "🌙 After Dark · from" | |
| 1f Do not disturb | idea.name, idea.description, day·time·duration, sender, partner | "DO NOT DISTURB", "Yes, Maybe or Pass" | Only offer for HOME ideas |
| 1g Sealed envelope | partner (possessive), sender.initial, idea.name, idea.description, day·time | "For …'s eyes only" | Names ending in s: "James'" |
| 1h Queen of hearts | idea.name, idea.description, day·time·where, sender | Q ♥, "🌙 After Dark · from" | |
| 1i Room key | day·time, idea.name, idea.description, sender, partner | "CHECK-IN ·", "ROOM FOR 2", "DO NOT DISTURB" | |
| 1j Tonight's menu | slot.day (long), plan.items[], idea.name, idea.description, sender | "TONIGHT'S MENU", I/II/III, "🌙 AND FOR DESSERT" | 0 plan items → hide the list |
| 1k Moonrise | day·time·duration, idea.name, idea.description, sender | "🌙 AFTER DARK", "From … · Yes, Maybe or Pass" | |
| 1l Question mark | sender, partner, idea.name, idea.description, day·time·duration | "has a question for", Yes/Maybe/Pass boxes, "Answer in DateMeUp" | Boxes are decorative, never pre-ticked |

Wordmark "datemeup" is fixed on every card.

## Text fitting
Titles are sized for the sample idea names. Real names can be up to 40 chars, so:
- `idea.name`: auto-fit. Start at the board size, step down 4px until it fits in 3 lines. Minimum 56px (1c, 1e Caveat: 72px). Never ellipsis a title.
- `idea.description`: fixed size, max 4 lines; above that, step down to 22px minimum.
- Names in running text: no fit, max 20 chars.
- Wait for `document.fonts.ready` (Sora, DM Sans, Caveat) before measuring.

## Rendering
- Page 816 × 1056 CSS px (8.5 × 11 in at 96 dpi). Viewport 816 × 1056, `deviceScaleFactor: 3` → 2448 × 3168 PNG (288 dpi). Same method as `SHARE_CARD_RENDER_FIX.md`.
- Capture the card element only. No board chrome: no option badges, captions, dashed field outlines or drop shadow.
- Full bleed: corner pixels must be the card background.

## Removed in rev 2 (were hard-coded, wrong for real users)
- 1c: letter body mentioned "the noodles and the jazz" and signed "Sam x". Now `idea.description` + `sender.firstName`.
- 1g: "Open it at 7" → now `idea.description`; seal letter now `sender.initial`.
- 1i: "ROOM 214" → fixed "ROOM FOR 2".
- 1j: fixed five mystery courses → `plan.items[]` + the idea.
- 1k: fixed "Lights low." headline → `idea.name`.
- 1l: 20-dot progress row (only made sense for one idea) removed.
- 1h, 1k: "Sam dealt this one", "suggested this for after Friday" → generic "from {sender}".
