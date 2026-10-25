# DateMeUp Share Cards: final 36-template set (for Hermes)

Oct 25 2026. This file is the single source for the invite card (Share Card) templates. It replaces the 36-template list in earlier handoffs.

- Design sources: `DateNight Share Cards.dc.html` (original templates) and `DateMeUp Invite Cards Round 2.dc.html` (reworked 2C, 2D, 2K, 3C, 3L and new 4A–4E). Where both show a template, the round 2 board wins.
- Two sizes per template, chosen automatically by delivery method (section 2). Letter layouts: `DateMeUp Invite Cards Letter.dc.html`.
- Render: phone = 360 × 640 CSS px at deviceScaleFactor 3 → 1080 × 1920 PNG. Letter = 816 × 1056 CSS px at deviceScaleFactor 3 → 2448 × 3168 PNG. Full bleed, no rounded corners or drop shadow, capture the card element only. Method in `SHARE_CARD_RENDER_FIX.md`.
- Two modes per template: **vote** (initial invite) and **official** (it's on). 36 templates × 2 modes × 2 sizes = 144 renders.
- After Dark add-on cards are separate: `AFTER_DARK_INVITE_CARDS_SPEC.md`.

## 1. The final 36
Removed: 1F Minimal calendar, 2A Boarding pass, 2I Forecast, 3E Passport page, 3F Bento tiles. Delete their code, picker entries and renders.

| # | ID | Template | Mood | Change | Photo | Design source |
|---|---|---|---|---|---|---|
| 1 | 1A | Bold type poster | playful | Global rules only | No | DateNight Share Cards.dc.html |
| 2 | 1B | Ticket stub | classic | Global rules only | No | DateNight Share Cards.dc.html |
| 3 | 1C | Polaroid | playful | Global rules only | Yes | DateNight Share Cards.dc.html |
| 4 | 1D | Retro postcard | classic | Global rules only | Yes | DateNight Share Cards.dc.html |
| 5 | 1E | Movie marquee | dark | Global rules only | No | DateNight Share Cards.dc.html |
| 6 | 1G | Handwritten note | classic | Global rules only | No | DateNight Share Cards.dc.html |
| 7 | 1H | Collage / sticker | playful | Global rules only | Yes | DateNight Share Cards.dc.html |
| 8 | 1I | Map pin | playful | Global rules only | No | DateNight Share Cards.dc.html |
| 9 | 1J | Neon sign | dark | Global rules only | No | DateNight Share Cards.dc.html |
| 10 | 1K | Receipt | playful | Global rules only | No | DateNight Share Cards.dc.html |
| 11 | 1L | Playbill / gig poster | classic | Global rules only | No | DateNight Share Cards.dc.html |
| 12 | 2B | Swiss grid | classic | Global rules only | No | DateNight Share Cards.dc.html |
| 13 | 2C | Vinyl sleeve | classic | **Reworked** (round 2) | No | Round 2 board |
| 14 | 2D | Comic panel | playful | **Reworked** (round 2) | Yes | Round 2 board |
| 15 | 2E | Chalkboard | dark | Global rules only | No | DateNight Share Cards.dc.html |
| 16 | 2F | Scoreboard | dark | Global rules only | No | DateNight Share Cards.dc.html |
| 17 | 2G | Newspaper | classic | Global rules only | No | DateNight Share Cards.dc.html |
| 18 | 2H | Confetti invite | playful | Global rules only | No | DateNight Share Cards.dc.html |
| 19 | 2J | Terminal | dark | Global rules only | No | DateNight Share Cards.dc.html |
| 20 | 2K | Badge / seal | classic | **Reworked** (round 2) | No | Round 2 board |
| 21 | 2L | Magazine cover | dark | Global rules only | Yes | DateNight Share Cards.dc.html |
| 22 | 3A | Luggage tag | classic | Global rules only | No | DateNight Share Cards.dc.html |
| 23 | 3B | Mixtape | playful | Global rules only | No | DateNight Share Cards.dc.html |
| 24 | 3C | Trading card | playful | **Reworked** (round 2) | Yes | Round 2 board |
| 25 | 3D | Formal invitation | classic | Global rules only | No | DateNight Share Cards.dc.html |
| 26 | 3G | Sticky notes | playful | Global rules only | No | DateNight Share Cards.dc.html |
| 27 | 3H | Bingo card | playful | Global rules only | No | DateNight Share Cards.dc.html |
| 28 | 3I | 8-bit arcade | dark | Global rules only | No | DateNight Share Cards.dc.html |
| 29 | 3J | Tarot card | dark | Global rules only | No | DateNight Share Cards.dc.html |
| 30 | 3K | Departures board | dark | Global rules only | No | DateNight Share Cards.dc.html |
| 31 | 3L | Wanted poster | classic | **Reworked** (round 2) | Yes | Round 2 board |
| 32 | 4A | Fortune cookie | playful | **New** (round 2) | No | Round 2 board |
| 33 | 4B | Matchbook | dark | **New** (round 2) | No | Round 2 board |
| 34 | 4C | Crossword | classic | **New** (round 2) | No | Round 2 board |
| 35 | 4D | Recipe card | classic | **New** (round 2) | No | Round 2 board |
| 36 | 4E | Library card | classic | **New** (round 2) | No | Round 2 board |

Mood filter counts: playful 12, classic 14, dark 10. 2C Vinyl sleeve moves from dark to classic (light sleeve now).

## 2. Size is automatic (never ask the user)
The user picks a template and mode only. They are never asked which size. The server renders the size the delivery method needs, on demand, and caches it per plan + template + mode + size.

| Delivery method | Size | How it's sent |
|---|---|---|
| In-app (thread message + push/in-app alert) | Phone 9:16 (1080 × 1920) | Image in the thread; push shows the thumbnail. "Vote now" is the card's own button area plus the native tap target |
| SMS / MMS | Phone 9:16 (1080 × 1920, compressed to ≤ 600 KB JPEG for carriers) | MMS image + one short line + plan link. Carriers that block MMS get the text + link only |
| Email | Letter 8.5 × 11 (2448 × 3168) | Inline at 600 px wide (1200 px PNG for retina) + print-ready PDF attached. Real "Vote now" button under the image (official: "Add to calendar" + .ics) |
| Share sheet (save / Stories / other apps) | Phone 9:16 | Unchanged |

Letter-size rules:
- No button drawn inside the letter card; the email's real button replaces it.
- No vote tallies in vote mode (email rule): footer reads "Your vote is waiting"; 3C shows STATUS: OPEN.
- 1A's lime "!" is skipped when `plan.title` already ends in . ! or ?.
- After Dark cards are never emailed (existing rule), so they only exist at the size used for in-app and share sheet. SMS for After Dark: not allowed unless decided otherwise (open question).

## 3. Rules for all 36 templates
1. **Vote mode (initial invite):** every template ends with a "Vote now →" button. It's fixed text; button style varies by template.
2. **Official mode (it's on):** no button. Remove "See the plan →" everywhere. The plan is already on the card.
3. **No notes or sign-offs.** Remove from every template and both modes:
   - "— Jo" and any other name sign-off ("xo, Jo", "Sheriff Jo:", "Captain Jo:", "Jo says:", "Meanwhile, Jo:", "Organiser Jo…").
   - "Pick your favourite — I'll book it!" and any other note line ("Special move —", "Remarks:", "NOTE:").
   - The "Your note" input in the customise screen goes too.
4. **The subtitle under the title in vote mode is not "Vote now".** Use the option count ("3 ideas on the table"). "Vote now" only appears on the button.
5. **No made-up stats.** No "HYPE 98", "EST. 2026", room/seat/gate numbers, or other invented figures. Only show real plan data.
6. **Nothing hard-coded.** Names, title, options, decided item, date, time, city, vote counts and photo all come from data (see fields).

## 4. Fields
| Field | Example | Notes |
|---|---|---|
| `group.displayName` | Sam & Priya | Couples: "A & B". Groups: group name |
| `members[].firstName` | Sam, Priya | 4E only, max 4 rows |
| `memberCount` | 2 | 4D "Serves" |
| `plan.title` | Date night · Friday | |
| `plan.options[].name` | Noodle Bar Kōji | Vote mode, max 3 shown |
| `plan.optionCount` | 3 ideas on the table | Vote mode subtitle |
| `plan.decided.name` | Jazz at the Blue Room | Official mode |
| `plan.date`, `plan.time` | Fri, Oct 30 · 7:00 PM | Recipient locale. Drop time if unset |
| `plan.city` | San Francisco, CA | City only, never a full address |
| `plan.votedCount / memberCount` | 1 / 2 | 3C |
| `plan.resultLine` | You both said yes 💛 / 1 of 2 voted | Couples: "You both said yes 💛". Groups: "4 of 6 picked this" |
| `plan.photo` | (image) | Templates marked Photo = Yes in section 1. Group library photo, else venue photo (with its attribution caption), else hide the frame and use the template's solid colour |

Everything else on the board (kickers "IS IT A DATE?" / "IT'S ON.", "Vote now →", "WANTED", "KA-CHING!", "CASE CLOSED", "RECIPE FOR", "DATEMEUP PUBLIC LIBRARY", crossword letters, etc.) is fixed template text.

## 5. Reworked templates (2C, 2D, 2K, 3C, 3L)
- **2C Vinyl sleeve:** light cream sleeve (#F2ECE1) so the black record stands out. Tracklist under the record: options (vote) or the decided item (official).
- **2D Comic panel:** couple photo under the speech bubble (greyscale + halftone dots). Date caption box on the photo. Official: "KA-CHING! You both said yes 💛".
- **2K Badge / seal:** ribbon under the seal ("PICK ONE" / "OFFICIALLY ON"), option list with numbered checks, time and city.
- **3C Trading card:** couple photo fills the frame. Stats: DATE, TIME, WHERE, VOTES (real data only). "Picks:" / "Winner:" line.
- **3L Wanted poster:** sepia couple photo in the frame. "FOR NOT VOTING YET" / "FOR A GOOD TIME". Reward: "Your vote" / the decided item. Official: "CASE CLOSED" stamp.

## 6. New templates (4A–4E)
- **4A Fortune cookie** (playful): red, cookie halves with a fortune slip containing the date. Lucky picks = options. Official: "IT'S WRITTEN" + date block at the bottom.
- **4B Matchbook** (dark): deep teal cover, gold border, striker strip. Options listed on the cover. Official: "STRUCK · YOU BOTH SAID YES".
- **4C Crossword** (classic): fixed DATE / TRUE grid. Clues = options. Official: "Solved · …".
- **4D Recipe card** (classic): lined index card. Ingredients = options. Method is fixed text per mode. Official: "SERVED" + date, city and result sticker at the bottom.
- **4E Library card** (classic): borrower rows = member names, due date = plan date. Red stamp "VOTE BY" / "CHECKED OUT" + plan date, in its own bottom area (never over the list). List bullets are a fixed ▪, no call numbers.

Moods for the picker: 4A playful, 4B dark, 4C classic, 4D classic, 4E classic.

## 7. Picker
- Order: the table above (1A … 3L, then 4A–4E). Show 36 in "All".
- Template IDs are stable. Use the ID, not the position, in stored user choices. Users who had a deleted template selected fall back to 1A.

## 8. Acceptance check
For each of the 144 renders:
- Correct size for its channel (1080 × 1920 phone, 2448 × 3168 letter); all four corner pixels are the card background.
- No size picker anywhere in the send flow.
- Phone vote mode shows exactly one "Vote now →" button; letter vote mode shows none (the email has the real button). Official mode shows no button and no "See the plan".
- No "Jo", no note line, no sample names (Sam, Priya, Jake, Erin), no "Together Plans" or "DateNight" branding.
- No invented numbers (hype scores, seat/gate/room numbers, call numbers).
- Long data still fits: test with a 40-character plan title, 3 options of 30 characters each, and a 20-character name.
