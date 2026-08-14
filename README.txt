Japanese Vocabulary Trainer v6

Upload index.html, manifest.json, icon.svg and vocabulary.txt to the repository root.

Changes:
- Initial buttons: Skip | Check.
- After a wrong answer: Skip | Next →.
- Correct answers still advance automatically.
- iOS keyboard: the app now sets the input language hint to English or Japanese and disables spellcheck/autocorrect. IMPORTANT: iOS Safari does not provide a web API that can force the active keyboard language. The app therefore cannot reliably switch the actual iOS keyboard from English to Japanese (or vice versa) automatically.
- No service worker.
