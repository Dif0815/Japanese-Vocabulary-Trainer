Japanese Vocabulary Trainer v17

GITHUB PAGES
------------
Versioned test link:
https://dif0815.github.io/Japanese-Vocabulary-Trainer/?v=17

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

Global layout settings:
    column_gap=28
    row_gap=16
    label_font_size=12
    label_color=#777777

- column_gap = horizontal space between columns, in pixels
- row_gap = vertical space between visual lines, in pixels
- label_font_size = label font size, in pixels
- label_color = label text color

Color examples:
    label_color=#777777
    label_color=#999999
    label_color=darkslategray
    label_color=rgb(100,100,100)

The column grid is calculated across ALL visual lines together. Column 1, column 2, etc.
therefore stay aligned from one line to the next.

Entry syntax example:
    line=1; label=Question word; db=question; under=true

- line = visual line number
- label = displayed label
- db = CSV/database field or virtual field
- under=true = value below the label
- under=false = label and value beside the value

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
The order of entries in this file is the reveal order.

Global layout settings:
    column_gap=28
    row_gap=16
    label_font_size=12
    label_color=#777777

- column_gap = horizontal space between columns, in pixels
- row_gap = vertical space between visual lines, in pixels
- label_font_size = label font size, in pixels
- label_color = label text color

Color examples:
    label_color=#777777
    label_color=#999999
    label_color=darkslategray
    label_color=rgb(100,100,100)

The column grid is calculated across ALL visual lines together. This keeps columns aligned
when different rows contain different labels or values.

Entry syntax:
    type=verb; direction=ja-en; line=1; label=Dictionary form; db=japanese; under=true

- type = verb / adjective / noun / phrase / all
- direction = ja-en or en-ja
- line = visual line number
- label = text shown above/beside the database value
- db = CSV/database field name or virtual field
- under=true = value below label / under=false = label and value side-by-side

If Reveal All is OFF, one non-empty configured item is added with each Next click.
If Reveal All is ON, the first Next reveals all non-empty configured items. The following
Next starts a new question.

If a database value is empty, that item and its label are ignored.

Virtual Training fields currently available:
- question
- correct_answer
- direction

Any field present in the CSV header can be used directly as db=. This means new columns such as
additional conjugation forms can be added to vocabulary.csv and then referenced in
training-layout.txt without changing the HTML.

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


Version 17 data correction
---------------------------
All verb and adjective conjugation columns have been checked and regenerated from the reading column.
Conjugation fields are kana-only. Special forms such as いい, かっこいい, くる, する, ある, and いく were checked separately.
The existing clear adjective-group corrections for ゆうめい, きらい, and とくい were also applied because they affect their conjugations.
No other vocabulary content was intentionally changed.


OPTIONS DEFAULTS
----------------
options-defaults.txt controls startup defaults. It is intentionally self-documenting.

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

Valid direction values are ja, en, and mixed. Vocabulary and Group values are taken from the
CSV dynamically; use all for no filter. Group values may be slash-separated in the CSV.
Ask Type and Reveal All are on/off settings. Changing either toggle during a session never
advances the current question/word.

QUESTION SELECTION
------------------
Quiz and Training use the same weighted question-selection logic. The current system is weighted
random selection, not simple uniform random selection.

1. The available pool is created from the selected Vocabulary and Group filters.
2. In Mixed direction, Japanese -> English or English -> Japanese is chosen randomly for each question.
3. Each available word receives a base weight and additional priority based on learning history.
4. Words with more wrong/unmastered answers receive higher priority.
5. Lower accuracy increases priority.
6. Words with little or no practice receive additional priority.
7. The time since the last correct answer increases priority gradually, up to a maximum contribution
   after roughly 150 days.
8. A weighted random selection chooses the next word.

This is not a fixed spaced-repetition schedule. A correctly answered word becomes less likely, but it
can still appear again relatively soon because selection remains weighted random. Difficult, new, or
long-unreviewed words become more likely.

Quiz learning history is direction-specific. Japanese -> English and English -> Japanese maintain
separate asked/correct/wrong counts and separate last-correct timestamps.

LEARNED WORD DEFINITION
-----------------------
The Statistics section will use the following working definition of a learned word in a direction:
- at least 10 attempts
- at least 90% accuracy
- at least 5 correct answers

A word is fully learned when it meets the criteria in BOTH directions. A word can therefore be
partially learned when only one direction meets the criteria.

Recency is handled separately: a learned word can be marked as needing review when it has not
been answered correctly for a defined period. The current working review threshold is 30 days.
A word does not stop being learned simply because it needs review.

VERSION 17 CHANGES
------------------
- Added global layout controls to last-layout.txt and training-layout.txt for column spacing, row spacing,
  label font size, and label color.
- Improved dynamic horizontal spacing: columns are calculated globally across all visual lines so they stay aligned.
- Added color examples to the layout documentation.
- Words filters now visibly show the existing Vocabulary and Group labels.
- Quiz Options are ordered consistently: Direction, Vocabulary, Group on line 1; Ask Type on line 2.
- Training Options remain unchanged.
- Expanded options-defaults.txt documentation.
- The current group values were cleared from vocabulary.csv so useful topic groups can be added deliberately later.
- Documented the current weighted-random question selection algorithm and the working learned-word criteria.

DEVELOPMENT
-----------
This application was developed collaboratively by Dif and OpenAI's ChatGPT (AI).
ChatGPT assisted with application architecture, programming, database design, configuration, and documentation.
The project requirements, learning concepts, testing, decisions, and customization are defined and directed by Dif.

