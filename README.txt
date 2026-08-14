Japanese Vocabulary Trainer v11

GITHUB PAGES WORKFLOW
- Upload these files to the root of the GitHub Pages repository:
  - index.html
  - vocabulary.csv
  - last-layout.txt
- Keep your existing manifest.json and icon.svg.
- vocabulary.csv is the authoritative vocabulary source.
- Learning statistics are NOT stored in vocabulary.csv.

VOCABULARY DATABASE (vocabulary.csv)
- Standard UTF-8 CSV. Edit it with Excel, Numbers, LibreOffice, etc.
- The first row is the database schema (column names).
- You may add new columns later. Add the field name to the header and one value/cell in the same position on every row. Empty cells are allowed.
- New database columns automatically become available to the Last layout and Modify view.
- The current conjugation columns are:
  Verbs:
    verb_stem
    verb_masu
    verb_te_form
    verb_short_negative
    verb_short_past
  Adjectives:
    adjective_group (i / na)
    adjective_negative
    adjective_past
    adjective_te_form

ENGLISH ANSWER SYNTAX
- In the english column, use / to separate alternative accepted answers.
  Example: fun / enjoyable
  Both "fun" and "enjoyable" are accepted.
- Parentheses (...) contain an optional qualifier/explanation.
  Example: to have (living things)
  The displayed answer includes the qualifier, but answer checking ignores the parenthetical part.
  Therefore "to have" and "to have (living things)" are both accepted.
- The two rules can be combined.
  Example: to have (living things) / to exist
  Each alternative is checked using the same parenthesis rule.
- For verbs only, a leading "to" is optional when checking an English answer.
  Example: to do and do are both accepted for a verb whose stored answer is "to do".
- This optional "to" rule does not apply to adjectives or nouns.
- Do not use / inside an answer unless you intend it to mean alternative accepted answers.

LEARNING STATISTICS
- Statistics are stored separately from vocabulary.csv in browser localStorage.
- The current browser storage key is jvt_v11_learning.
- Statistics contain only learning information, not vocabulary fields and not type-specific statistics.
- Stored fields per word:
  ja_asked
  ja_correct
  ja_wrong
  en_asked
  en_correct
  en_wrong
  last_added
  last_ja_correct
  last_en_correct
- Ask Type is part of the same quiz question. If Ask Type is ON, the translation AND the selected type must both be correct for the whole question to count as correct.
- Type questions do not have their own asked/correct/wrong statistics.
- If Ask Type is OFF, the type is not checked and cannot affect the statistics.
- A wrong type after a correct translation makes the whole question wrong.
- last_ja_correct / last_en_correct record when the complete question was last answered correctly in that direction.
- last_added records when the word first received a learning record in the browser.
- The trainer uses Japanese + reading as the stable matching key between vocabulary.csv and learning statistics. The CSV row number is NOT used.
- Therefore you can add, remove, reorder, or edit other CSV rows without shifting statistics to another word.
- If a word with the same Japanese + reading is present in the new CSV, its existing browser statistics are reused.
- A newly added word gets a new learning record.
- If a word is removed from the CSV, its learning record may remain in the browser backup/storage, but it is no longer used by the quiz until that word is present again.

STATISTICS BACKUP / RESTORE
- Use "Export statistics" in the Learning Data section to download:
  japanese-vocabulary-learning.json
- The JSON file is a backup of the browser learning data. It does not contain vocabulary.csv.
- Export regularly if you care about preserving learning progress.
- Use "Import statistics" to restore/merge a JSON backup into the current browser data.
- Import matches records by Japanese + reading, not by CSV row number.
- Only matching words in the current vocabulary are imported. Words that exist only in the backup are ignored until they appear in vocabulary.csv again.
- For a matching word, the imported learning values replace the current values for that word.
- This makes the workflow simple:
    1. Edit vocabulary.csv and upload it to GitHub Pages when vocabulary changes.
    2. Keep learning progress in the browser.
    3. Periodically export japanese-vocabulary-learning.json as a backup.
    4. On a new browser/device, load the same vocabulary.csv and import the JSON backup.

STATISTICS / REVIEW LOGIC
- Translation statistics count the completed quiz result, not the intermediate translation step.
- When Ask Type is ON, the result is not final until the type button is answered.
- Example: translation correct + type wrong = ja_wrong/en_wrong increases; correct does not increase.
- A skipped word does not count as asked, correct, or wrong.
- last_ja_correct and last_en_correct are intended for future recency-based review. A word can have many historical correct answers but still become a review candidate if it has not been answered correctly for a long time.

LAST FRAME
- last-layout.txt controls the Last frame.
- Multiple fields can be placed on the same line by giving them the same line number.
- label= is the visible label; db= is the database/virtual field; gap= controls horizontal spacing.
- under=true puts the value below its label; under=false puts label and value beside each other.
- Empty values hide the complete label/value item.
- Database fields come from the first row of vocabulary.csv.
- Virtual fields for the Last result: question, correct_answer, answer_given, result.

QUIZ OPTIONS
- Ask Type ON/OFF: asks ru/u/irr for verbs and i/na for adjectives when a type is stored.
- Type selection uses buttons instead of text input.
- Direction: Japanese -> English, English -> Japanese, Mixed.
- Vocabulary filter: All, Verbs, Adjectives, Nouns, Phrases.

CONJUGATION DATA
- Conjugation values are vocabulary data and belong in vocabulary.csv.
- Learning statistics never overwrite conjugation columns.
- When rebuilding or extending the database, fill conjugation columns carefully from the dictionary form and the stored verb/adjective group.

CSV NOTES
- Use UTF-8 encoding so Japanese text is preserved.
- Commas inside a value must be quoted by the spreadsheet program when necessary.
- If you add a column, give every row a value or leave the cell empty.
