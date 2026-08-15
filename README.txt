Japanese Vocabulary Trainer v16

GITHUB PAGES
------------
Versioned test link:
https://dif0815.github.io/Japanese-Vocabulary-Trainer/?v=16

Normal link:
https://dif0815.github.io/Japanese-Vocabulary-Trainer/

The ?v=16 ending is a cache-busting version marker. Increase it when uploading a
new version so the browser is forced to request the new files.

FILES
-----
Upload these files to the root of the GitHub Pages repository:
- index.html
- vocabulary.csv
- last-layout.txt
- training-layout.txt
- options-defaults.txt
Keep your existing manifest.json and icon.svg.

The version number is shown on the main page and in the HTML title.
The options-defaults.txt file controls startup defaults for Quiz and Training.

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
- group
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
Any new CSV column automatically becomes available to the configurable Last and Training layouts.

FILTERS
-------
The type column is an open-ended word classification field. The app does not hard-code the available
word types. It reads all unique non-empty values from the CSV and creates the filter choices dynamically.
This means new types such as adverb, pronoun, question_word, or other categories can be added without
changing the HTML/JavaScript.

The group column is an optional topic/grouping field. It replaces the old source column. Examples:
- body parts
- directions
- food

A word may belong to multiple groups by separating them with /. For example:
    food/restaurant

The Group filter treats each slash-separated value as an independent group. The same dynamic Group filter
is available in Quiz, Training, and Words. Statistics will use the same filters when the Statistics section
is implemented.

Type and Group filters do not change the vocabulary database. They only change the currently displayed/selected
vocabulary pool.

ANSWER SYNTAX
-------------
In the English field:
- / separates alternative accepted answers.
  Example: fun / enjoyable accepts either answer.
- (...) is an optional qualifier. It is shown as part of the vocabulary information,
  but the parenthetical qualifier is ignored when checking the answer.
  Example: to have (living things) can be answered with "to have" or "have".
- Verb English meanings are stored without the leading "to". Quiz and Training display "to " automatically for verbs.
- For verbs, "to" is optional when checking. For example, both "to do" and "do" are accepted.
- Japanese -> English checks the English meanings using the rules above.
- English -> Japanese accepts the stored reading or Japanese field.

LEARNING STATISTICS
-------------------
Vocabulary and learning history are separate.

vocabulary.csv = what the word IS.
Browser learning data = what YOU have done with the word.

Learning data is stored locally in the browser under the existing app storage key so updates
can retain the current learning history. It contains:
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

QUIZ
----
Options:
- Ask Type: ON/OFF
- Direction: Japanese -> English / English -> Japanese / Mixed
- Vocabulary: dynamically generated from the unique values in the CSV type column
- Group: dynamically generated from the unique group values in the CSV group column

Changing Ask Type does NOT advance or replace the current question. It only changes the
setting used for the current/next quiz flow. The Options frame is placed below the main Quiz content.

The Next button remains on the right side after an answer is checked.

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
Multiple fields may share one line.

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

TRAINING
--------
Training is separate from Quiz and NEVER changes learning statistics.
It has:
- Direction: Japanese -> English / English -> Japanese / Mixed
- Vocabulary: dynamically generated from CSV type values
- Group: dynamically generated from CSV group values
- Reveal All: ON/OFF

The question word is selected using the same direction/vocabulary/question selection logic as
the Quiz, but Training does not record an asked/correct/wrong result.

Changing the Training Reveal All setting does NOT advance, skip, clear, or replace the current
training word. It only changes what the next button does. Training labels remain visible while values are hidden. The Options frame is placed below the Training content.

Training reveal layouts are stored in training-layout.txt and are edited manually.
Mixed mode does NOT need a separate layout: the actual direction of each mixed question
selects the ja-en or en-ja configuration automatically.

TRAINING LAYOUT CONFIGURATION
-----------------------------
One config entry = one reveal item. Multiple entries can share the same visual line number.
The order of entries is the reveal order.

Syntax:
    type=verb; direction=ja-en; line=1; label=Dictionary form; db=japanese; under=true; gap=28

- type = verb / adjective / noun / phrase / all
- direction = ja-en or en-ja
- line = visual line number
- label = text shown above/beside the database value
- db = CSV/database field name or virtual field
- under=true = value below the label
- under=false = label and value beside the value
- gap = horizontal spacing after the item in pixels

If Reveal All is OFF, one non-empty configured item is added with each Next click.
If Reveal All is ON, the first Next reveals all non-empty configured items. The following
Next starts a new question.

If a database value is empty, that item and its label are ignored.

Virtual Training fields currently available:
- question
- correct_answer
- direction

Any field present in the CSV header can be used directly as db=.
This means new columns such as additional conjugation forms can be added to vocabulary.csv
and then referenced in training-layout.txt without changing the HTML.

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

VOICE INPUT
-----------
Voice input is intentionally NOT part of this version. It was tested separately and is
being postponed for a future version.


Version 16 data correction
---------------------------
All verb and adjective conjugation columns have been checked and regenerated from the reading column.
Conjugation fields are kana-only. Special forms such as いい, かっこいい, くる, する, ある, and いく were checked separately.
The existing clear adjective-group corrections for ゆうめい, きらい, and とくい were also applied because they affect their conjugations.
No other vocabulary content was intentionally changed.


OPTIONS DEFAULTS
----------------
options-defaults.txt controls startup defaults:
[quiz]
ask_type=on
direction=mixed
vocabulary=all
group=all

[training]
direction=mixed
vocabulary=all
group=all
reveal_all=off

Changing an Ask Type or Reveal All toggle never advances the current question/word.

VERSION 16 CHANGES
------------------
The former source column is now named group and is intended as an optional topic/grouping field.
Group values may contain multiple slash-separated groups.

Type and Group filters are now generated dynamically from the CSV and are available in Quiz, Training,
and Words. New type or group values therefore appear automatically without an app code change.

The existing Quiz/Training verb display logic remains: verb English meanings are stored without leading
to and the app displays to automatically.

The weather words はれ, くもり, and ゆき remain classified as nouns.
