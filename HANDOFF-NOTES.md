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
- "File type" choice (both tabs, kept in sync): **GIF** (default, for Messenger) = see-through GIF with a Size choice: Small 96 px (default, about the size of Messenger's own stickers), Medium 128, Big 160, lightly sharpened after shrinking; **PNG** = 512×512 see-through, for Telegram/WhatsApp/Viber; **JPG** = 512×512 on white (JPG can't be see-through), for printing/documents. Size row only shows for GIF. Reason: Messenger turns PNGs into photos (big, with a black or white box) but keeps GIFs as they are. Tested on Samsung + Messenger with a test page (small GIF won). GIF maker is in the page (no library): 255-color median cut, edges blended onto white, 1 see-through color.
- Each sticker's inside is filled separately (closes outline gaps of about 1/18 of the sticker size), so white faces/bodies stay solid. Before this fix, moving the Cutting slider right (40+) made white faces see-through, and they showed black in Messenger's dark mode. The slider now goes up to 60 (was 90), because higher values erase thin grey outlines.

## Tested
- Headless Chrome on the computer with her two sample sheets: 12/12 stickers cut correctly for both. Trace + words worked. No errors.
- NOT yet tested on real phones (Samsung, realme, tablet, iPhone 13, iPhone 6 Plus). Share button needs a real phone.

## Progress (Oct 30, 2026, evening)
- index.html + these notes are on `main`. GitHub Pages is on (main, root). Live at https://navikichi.github.io/sticker-maker/
- Works on the computer. Samsung (Chrome): cutting works; found the see-through face bug above and fixed it.
- Messenger showed shared PNGs big and boxed → added the Messenger GIF option. Small GIFs look a bit softer than PNGs (fewer pixels). She asked for smiley-sized stickers → added Size (Small = default). She also asked to be able to download GIF, PNG or JPG → "Made for" became "File type: GIF | PNG | JPG".

- **iPhone stickers work (her main goal):** save as **File type: PNG** on the iPhone, open the picture, press and hold the character, tap **Add Sticker** (iOS 17+). The iPhone now recognizes the character because the PNG is see-through. The stickers are then in the iPhone's own sticker drawer and show small, without a box, in Messenger and other apps. Before (plain sheet / photo), the iPhone did not recognize it. Confirmed Oct 31 with screenshots: all 12 squirrel stickers in the iPhone sticker drawer; sent ones show small and see-through on iPhone, Samsung and laptop Messenger. Steps added to the app's "How to use your stickers" box.
- GIF option still useful for Android phones (Samsung etc.), which have no "Add Sticker".

## Next
1. In a fresh chat with `navikichi/sticker-maker` attached: Claude uploads `index.html` + these notes, then she turns on GitHub Pages.
2. Test on phones.
3. Later: built-in AI (API), make it installable (PWA).
4. Later: built-in stickers in the Interactive Notebook (see plan below).

## Future plan: stickers inside the Interactive Notebook
- The Notebook is `msegonia/interactive-notebook` (live at theinternotebook.com). **Beryl is one of its five class sections** (Amethyst, Beryl, Citrine, Diamond, Emerald), not a separate app.
- The Notebook already has a chat (teacher ↔ students/groups, announcements). Messages go through Apps Script (`action:"send"` with `{cid, text, photo}`).
- Plan, like Messenger's sticker store:
  1. Sticker packs live in the Notebook site: `stickers/<pack>/01.gif …` plus a list file. Free, fast, cached by the service worker for offline.
  2. A 😊 button next to the chat box opens a tray with one tab per pack; tap to send.
  3. A sticker is sent as a short code in the text, e.g. `[[sticker:squirrel/03]]`, and the app shows it as the picture. No change to Apps Script or the Google Sheet; no Drive space or photo limit used. Old app copies would show the code text until refreshed.
  4. New packs: add an "Export for Notebook" button in Sticker Maker (named GIFs + list), then upload to the Notebook repo. Maybe a teacher upload screen later.
- Before starting:
  - The Notebook's `index.html` is **built (minified) code**. We need the **source files** to add features safely. Ask her where they are.
  - This session can only read the Notebook repo; changes need write access to `msegonia/interactive-notebook`.
  - Check whether `app/Notebook.apk` just opens the website (then no new APK needed).
  - Open questions: students allowed to send stickers or teacher only? Beryl first as a test, or all sections? Which first pack (the squirrel set)?
- Do this in a fresh chat with the Notebook repo attached.
