# Sticker Maker – Handoff Notes

## What it is
One web page (`index.html`) with 3 tools:
1. **✨ Make with AI** – builds a ChatGPT prompt (feelings, style, words). Teacher copies it into the ChatGPT app with a photo (free with her ChatGPT subscription).
2. **✂️ Cut a sheet** – upload a sticker sheet; it removes the plain background, finds each sticker, adds a white border, and makes 512×512 see-through PNGs. Share, save one, or save all as ZIP. Tap a sticker to add words, remove it, etc.
3. **📷 Photo sticker** – upload a photo, trace with a finger (or keep the whole picture, rounded/circle), add words (top/bottom, 5 colors), white border, share/save.

Everything runs in the browser. No server, no internet needed after opening, pictures never leave the device.

## Decisions (Oct 30, 2026)
- AI: start with the free "copy prompt to ChatGPT app" way (1C). A built-in way (OpenAI API, paid per picture, key hidden in Apps Script) comes later.
- Users: teacher + students later, inside the Interactive Notebook and Beryl (2B).
- Hosting: first planned on msegonia (3B), but changed the same day to her personal GitHub **navikichi**, repo `navikichi/sticker-maker` (created Oct 30, README only), so Claude can save changes directly. The empty `msegonia/sticker-maker` can be deleted later (optional).
- Messenger can't add custom sticker packs, so stickers are shared as PNG pictures. WhatsApp needs a helper app; Telegram uses @Stickers bot.

## Tech notes
- Written to work on old iPhones (iOS 12): no `?.`, no `??`, no CSS `inset`, touch fallback when Pointer Events are missing.
- Cutting: background color = most common edge color; "Cutting" slider = tolerance (default 30); gaps in outlines are closed so white insides stay; small bits (hearts, sparkles) join the nearest big sticker.
- ZIP is made in the page (no extra library).
- Each sticker's inside is filled separately (closes outline gaps of about 1/18 of the sticker size), so white faces/bodies stay solid. Before this fix, moving the Cutting slider right (40+) made white faces see-through, and they showed black in Messenger's dark mode. The slider now goes up to 60 (was 90), because higher values erase thin grey outlines.

## Tested
- Headless Chrome on the computer with her two sample sheets: 12/12 stickers cut correctly for both. Trace + words worked. No errors.
- NOT yet tested on real phones (Samsung, realme, tablet, iPhone 13, iPhone 6 Plus). Share button needs a real phone.

## Progress (Oct 30, 2026, evening)
- index.html + these notes are on `main`. GitHub Pages is on (main, root). Live at https://navikichi.github.io/sticker-maker/
- Works on the computer. Samsung (Chrome): cutting works; found the see-through face bug above and fixed it.

## Next
1. In a fresh chat with `navikichi/sticker-maker` attached: Claude uploads `index.html` + these notes, then she turns on GitHub Pages.
2. Test on phones.
3. Later: built-in AI (API), add to Interactive Notebook and Beryl, make it installable (PWA).
