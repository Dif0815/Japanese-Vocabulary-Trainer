Japanese Vocabulary Trainer v12

FILES
-----
Upload these files to the root of the GitHub Pages repository:
- index.html
- vocabulary.csv
- last-layout.txt
Keep your existing manifest.json and icon.svg.

VOCABULARY DATABASE
-------------------
vocabulary.csv is the authoritative vocabulary database. The browser does NOT edit it.
Edit the CSV manually (Excel, Numbers, LibreOffice, etc.) and upload the new CSV to GitHub.
You can add/remove/reorder vocabulary rows without changing learning statistics.

The first CSV row is the database header. Current columns include:
- japanese
- reading
- english
- type
- verb_group
- adjective_group
- source
- additional_info
- verb_stem
- verb_masu
- verb_te_form
- verb_short_negative
- verb_short_past
- adjective_negative
- adjective_past
- adjective_te_form

Conjugation fields are stored separately for verbs and adjectives. adjective_group is i or na.

ANSWER SYNTAX
-------------
In the English field:
- / separates alternative accepted answers.
  Example: fun / enjoyable accepts either answer.
- (...) is an optional qualifier. It is shown as part of the vocabulary information,
  but the parenthetical qualifier is ignored when checking the answer.
  Example: to have (living things) can be answered with "to have" or "have".
- For verbs, "to" is optional when checking. For example, both "to do" and "do" are accepted.
- Japanese -> English checks the English meanings using the rules above.
- English -> Japanese accepts the stored reading or Japanese field.

LEARNING STATISTICS
-------------------
Vocabulary and learning history are separate.

vocabulary.csv = what the word IS.
Browser learning data = what YOU have done with the word.

Learning data is stored locally in the browser under the current app storage key.
It contains:
- ja_asked / ja_correct / ja_wrong
- en_asked / en_correct / en_wrong
- last_added
- last_ja_correct
- last_en_correct

Statistics are matched to vocabulary by a stable key:
    normalized Japanese + "|" + normalized reading

The CSV row number is NOT used. Therefore adding, deleting, or reordering CSV rows does
not move statistics to another word.

When a word is new, the trainer creates fresh statistics and records last_added.
Existing statistics are retained when the same Japanese + reading is found again.

ASK TYPE
--------
Ask Type is an optional second step of the SAME quiz question.
- Verbs: ru-verb / u-verb / irregular
- Adjectives: i-adjective / na-adjective

The type question uses buttons rather than typed input.
If Ask Type is OFF, type does not affect the result or statistics.
If Ask Type is ON, the whole question is correct only when BOTH the translation and type
are correct. A wrong type therefore makes the whole question wrong.
There are NO separate type statistics.

LAST
----
The Last frame is updated only after a question is completely finished, including the type
question when Ask Type is ON. If the translation is wrong, the type question is not asked
and the question is finished immediately.
Skip also updates Last but does not count as an answer wrong/correct.

last-layout.txt controls the Last frame. It is edited manually, not through the browser.
Multiple fields may share one line number.
Syntax example:
    line=1; label=Question word; db=question; under=true; gap=28

- line = visual line number
- label = displayed label
- db = CSV/database field or virtual field
- under=true = value below the label
- under=false = label and value beside the value
- gap = horizontal spacing after the item in pixels

If a database value is empty, the complete label/value item is ignored.
Virtual fields available for Last include:
- question
- correct_answer
- answer_given
- result

Any field present in the CSV header can be used as db= without changing the HTML.

BACKUP / RESTORE
----------------
Use the Backup tab to:
- Export statistics -> japanese-vocabulary-learning.json
- Import statistics -> select a previously exported JSON file

The JSON backup contains learning statistics, not vocabulary. Importing only applies records
whose Japanese + reading match a word currently present in vocabulary.csv.

Recommended workflow:
1. Edit vocabulary.csv manually.
2. Upload vocabulary.csv to GitHub Pages.
3. Play the trainer; learning statistics stay in the browser.
4. Periodically use Backup -> Export statistics.
5. On a new browser/device, upload the same vocabulary.csv and then Backup -> Import statistics.

OLD STATISTICS MIGRATION
------------------------
The app contains migration support for older learning-storage keys so existing history can
survive the transition to the current architecture. Old type-statistic fields are ignored.

QUIZ OPTIONS
------------
- Ask Type: ON/OFF
- Direction: Japanese -> English / English -> Japanese / Mixed
- Vocabulary: All / Verbs / Adjectives / Nouns / Phrases

The voice-input feature is NOT part of this version. It is being tested separately first.
