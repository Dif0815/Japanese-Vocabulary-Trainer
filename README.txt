Japanese Vocabulary Trainer v29

CURRENT VERSION
---------------
Version: 29
Status: Current development version

Versioned test link:
https://dif0815.github.io/Japanese-Vocabulary-Trainer/?v=29

The ?v=xx ending is a cache-busting version marker. Increase it when uploading a
new version so the browser is forced to request the new files.

Normal link:
https://dif0815.github.io/Japanese-Vocabulary-Trainer/

VERSION / DATA DOCUMENTATION
----------------------------
Version history is kept separately in:
    docs/version-changes.txt

Vocabulary additions, corrections, and other intentional CSV data changes are kept in:
    data/data-changes.txt

This README describes the current application only.

FILES
-----
The repository uses the following structure:

    index.html
    manifest.json
    README.txt

    data/
        vocabulary.csv
        data-changes.txt

    config/
        options-defaults.txt
        question-settings.txt
        learned-settings.txt
        type-config.txt
        last-layout.txt
        training-layout.txt
        word-popup-layout.txt

    icons/
        icon.svg
        learned.svg
        not_learned.svg
        needs_review.svg
        partially_learned.svg

    docs/
        version-changes.txt

The root contains the application entry point, PWA manifest, and current README.
The data directory contains the vocabulary source and its data-change history.
The config directory contains application customization/defaults and layout definitions.
The icons directory contains graphical assets. The docs directory contains version history.

The application loads all data and configuration files using these relative paths. Do not move
individual files without updating the corresponding application references.

The version number is shown on the main page and in the HTML title.

ARCHITECTURE / SOURCE OF TRUTH
------------------------------
The project separates application code, data, configuration, icons, and documentation.

- data/vocabulary.csv is the source of truth for vocabulary and for which word types/groups/subtypes
  actually exist.
- config/ contains customization and defaults. In particular, config/type-config.txt defines labels
  and keyboard aliases for discovered verb/adjective subtypes; it does not define which subtypes exist.
- icons/ contains visual assets referenced by the application.
- docs/ contains version history.

The v28 repository restructuring changed file locations only; v29 builds the queued filter, Last-result,
and Word Popup learning-statistics features on that structure without changing the vocabulary data model
or learning-data matching rules.

VOCABULARY DATABASE
-------------------
data/vocabulary.csv is the authoritative vocabulary database. Edit it manually and upload the
new CSV to GitHub Pages. The browser does not write changes back to the CSV.

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

Any new CSV column automatically becomes available to the configurable Last, Training,
and Words popup layouts.

Vocabulary and learning statistics are separate. Statistics are stored locally in the
browser and matched by normalized Japanese + reading, not by CSV row number.

ARCHITECTURE / DYNAMIC DATA-DRIVEN TYPES
----------------------------------------
The CSV is the source of truth for vocabulary types and subtype/group values. The application
discovers these values at runtime instead of keeping a fixed list in the HTML or JavaScript.

The top-level Vocabulary and Group filters are therefore automatically expanded when new values
are introduced in data/vocabulary.csv. The same principle is used for Quiz Ask Type: verb and adjective
subtypes are discovered from the corresponding CSV fields (verb_group and adjective_group), and the
type popup is generated from the values that actually exist in the vocabulary data.

Subtype presentation and keyboard aliases are separated from the vocabulary data in:
    config/type-config.txt

This file defines, for configured subtypes:
- the friendly display label
- the keyboard aliases used by the Ask Type popup

Keyboard aliases may contain both romaji and kana. For example:
    i-adjective -> i, い
    na-adjective -> n, na, な

The configuration file does NOT define which subtypes exist. A subtype only becomes available when
it is present in data/vocabulary.csv. This separation means new CSV subtype values are automatically
discovered without an application-code change, while presentation and keyboard mappings remain
explicit and maintainable.

There is intentionally no fallback subtype definition. If a subtype exists in data/vocabulary.csv but
is missing from config/type-config.txt, the raw CSV value remains available as a touch-selectable button and
the type popup shows a small, soft-grey warning that the subtype is not configured. No invented label
or keyboard mapping is applied.

The layout engine is also data/configuration-driven. It supports arbitrary numbers of logical lines
and items/columns. Empty items and empty logical lines are removed from the visual grid, so later
populated rows/columns are compacted automatically. The same engine is reused by Last, Training
Reveal, and the Word Popup.

FILTERS
-------
The type column is open-ended. The app reads all unique non-empty type values from the CSV
and creates the Vocabulary filter dynamically. New word types therefore do not require HTML
or JavaScript changes.

The group column is optional and is also generated dynamically. A word may belong to multiple
groups by separating them with /, for example:
    weather/object description

The Group filter treats each slash-separated value as a separate group.

Vocabulary and Group are multi-select filters in Quiz, Training, and Words. Selection is OR within one
filter and AND between Vocabulary and Group. Both filters dynamically constrain each other from the
loaded CSV. Each option remains visible; incompatible options are disabled and show (0), while
compatible options show their calculated count. Group also offers (empty) for words with no Group value.
The special All entry selects all currently compatible values; no selected values means zero results.

ANSWER CHECKING
---------------
English meanings may contain / or ; separated alternatives. Parenthetical qualifiers are
shown as vocabulary information but are ignored during answer checking.

For example:
    cold (object / drink)

is checked as:
    cold

The qualifier is removed before alternatives are split, so slashes inside parentheses do
not create additional answers.

Verb meanings are stored without the leading "to". Quiz and Training display "to" automatically
for verbs, and both forms are accepted when checking an answer.

Japanese -> English checks the English meanings. English -> Japanese accepts the stored reading
or Japanese field using the same normalization rules.

QUIZ
----
Quiz options:
- Direction: Japanese -> English / English -> Japanese / Mixed
- Vocabulary: dynamically generated from CSV type values
- Group: dynamically generated from CSV group values
- Ask Type: ON/OFF

Ask Type is an optional second step of the same quiz question. The available subtype buttons are
discovered dynamically from the current vocabulary data:
- Verbs use the unique values in verb_group.
- Adjectives use the unique values in adjective_group.

Current configured values are u / ru / irr for verbs and i / na for adjectives. The application does not
require this fixed set; new subtype values introduced in the CSV automatically become available, subject
to optional presentation/keyboard definitions in config/type-config.txt.

The type popup supports both touch and keyboard input. Typing a configured keyboard alias highlights the
matching button; prefix matching is supported when it identifies one subtype. Enter confirms the selected
subtype. Touching a button still confirms the type directly.

Quiz and Training also show the general vocabulary word type as a small, muted hint directly below the
question. The subtype is not shown there, so Ask Type remains a real subtype question.

If Ask Type is OFF, a correct translation counts as Correct.
If Ask Type is ON, both translation and type must be correct for the complete question to count as Correct.
There are no separate type statistics.

Enter handling:
- while entering a translation answer, Enter submits the answer
- while the type popup is open, Enter confirms the selected type
- after a wrong Quiz result, Enter activates Next

Skip does not count as wrong or correct and immediately starts the next question.

Further question-selection details are documented in:
    config/question-settings.txt

LAST
----
The Last frame shows the completed result of the most recently finished or skipped question.
It is configurable through:
    config/last-layout.txt

The layout engine supports an arbitrary number of configured logical lines and items.
Empty items do not create empty columns, and empty logical lines do not consume vertical space.
Subsequent populated lines are automatically compacted upward.

Basic syntax:
    line=1; label=Question word; db=question; under=true

Further layout syntax and settings are documented directly in:
    config/last-layout.txt

Last also supports the virtual field type_answer_given. It contains the learner's actual subtype answer,
using the configured subtype label. For English -> Japanese answers, the Last Correct answer automatically
shows the CSV reading below the Japanese answer when the reading differs from the displayed Japanese form.
The rule uses the effective direction of the individual question, including Mixed mode.

TRAINING
--------
Training is separate from Quiz and does not change learning statistics.

Options:
- Direction: Japanese -> English / English -> Japanese / Mixed
- Vocabulary: dynamically generated from CSV type values
- Group: dynamically generated from CSV group values
- Reveal All: ON/OFF

Training uses the same question-selection logic and repetition spacing as Quiz, but does not
record an answered result.

Training reveal layouts are configured in:
    config/training-layout.txt

The file supports type-specific layouts and a type=all fallback. A new CSV word type can be
customized simply by adding a matching configuration entry; otherwise the fallback is used.

The same layout engine handles optional fields and lines, including automatic removal of
empty columns and empty logical rows.

WORDS
-----
The Words section provides:
- Search
- Direction
- Vocabulary
- Group
- Learning/review status filters

Direction:
- Mixed = show Japanese -> English and English -> Japanese separately, plus one overall Status column
- Japanese -> English = show only that direction
- English -> Japanese = show only that direction

The status legend is also a multi-select filter. The checkboxes are intentionally compact and visually
subordinate to the status symbols. Matching is OR-based: a word is shown when it
matches at least one selected status. If no status filters are selected, all words are shown.

Available statuses:
- Learned
- Not learned
- Needs review
- Partially learned (Mixed mode only)

Partially learned is an overall word status:
- both directions learned = Learned
- exactly one direction learned = Partially learned
- neither direction learned = Not learned

In Mixed mode, each word has an overall Status position followed by the two directional columns.
Each of the three positions is labeled per word for consistent alignment. The directional columns
independently show learned/not-learned and Needs Review.

Needs Review is shown only when active, but its reserved position remains aligned so the learned
symbol does not move when review is inactive.

Partially learned is hidden from the legend and ignored as a filter while a directional Words
mode is selected. Its checkbox selection is retained and becomes active again when Mixed mode
is selected.

The four status symbols are stored as root-level files:
- icons/learned.svg
- not_icons/learned.svg
- icons/needs_review.svg
- partially_icons/learned.svg

Clicking a word opens a configurable popup.

WORDS POPUP
-----------
The popup uses one configuration file for all word types:
    config/word-popup-layout.txt

The file contains sections such as:
    [verb]
    [adjective]
    [fallback]

When a word is opened, the exact CSV type is matched first. If no matching section exists,
[fallback] is used.

Therefore a newly introduced word type can be customized by adding a matching section without
changing the application code.

The popup uses the same dynamic layout engine as Last and Training. Empty fields and empty
logical lines are automatically removed from the visual layout.

Detailed syntax, virtual fields, fallback behavior, and styling settings are documented in:
    config/word-popup-layout.txt

The Word Popup can also display all persistent per-word learning fields:
    ja_asked, ja_correct, ja_wrong, en_asked, en_correct, en_wrong,
    last_added, last_ja_correct, last_en_correct,
    ja_wrong_since_correct, en_wrong_since_correct,
    ja_review_active, ja_review_asked, ja_review_correct,
    en_review_active, en_review_asked, en_review_correct.
These are learning data stored separately in the browser and do not become CSV columns.

OPTIONS DEFAULTS
----------------
Startup defaults for Quiz, Training, and Words are configured in:
    config/options-defaults.txt

The file is intentionally self-documenting. Detailed option descriptions belong in that file
rather than being duplicated here.

The Words status filters use individual on/off defaults:
    status_filter_learned=on
    status_filter_not_learned=on
    status_filter_needs_review=on
    status_filter_partially_learned=on

These values define the initial checkbox state only. They do not lock the filters.
Partially learned remains Mixed-mode-only.

LEARNED / NEEDS REVIEW
----------------------
Learned and Needs Review are tracked independently for each direction.

Detailed thresholds and recovery settings are configured in:
    config/learned-settings.txt

Current settings:
    min_attempts=10
    min_accuracy=90
    min_correct=5
    review_days=30
    wrong_answers_to_review=1
    review_recovery_accuracy=80
    review_recovery_min_answers=5

A direction is Learned only when all three learned criteria are met.
Fully learned is derived, not stored separately:
- both directions learned = fully learned
- exactly one direction learned = partially learned
- neither direction learned = not learned

Needs Review is independent from Learned. A direction can therefore be Learned and still need review.
One wrong answer immediately triggers review with the current default.

Once Needs Review is triggered, it remains active until the configured recovery conditions are met.
Recovery is tracked separately per direction. With the current defaults, at least 5 recovery answers
are required and accuracy must be at least 80%.

See config/learned-settings.txt for the authoritative detailed documentation.

QUESTION SELECTION
------------------
Quiz and Training use the same weighted random question-selection logic.

The selector:
1. applies the selected Vocabulary and Group filters
2. chooses a direction according to the current Direction setting
3. weights words according to learning history
4. gives additional priority to difficult, new, or review-needed words
5. lowers the weight of learned directions
6. applies the configured recent-word exclusion before selection

Needs Review is prioritized but is not an exclusive queue.

Repetition spacing is configured in:
    config/question-settings.txt

Current setting:
    min_different_words_between_repetitions=5

This means five DIFFERENT words must be asked before the same word may be selected again.
The rule applies to the word itself, regardless of direction.
If the filtered pool is too small to satisfy the configured distance, the selector uses the
maximum separation possible.

BACKUP / RESTORE
----------------
Use the Backup tab to:
- Export statistics -> japanese-vocabulary-learning.json
- Import statistics -> select a previously exported JSON file

The backup contains learning statistics, not vocabulary. Importing matches records by Japanese + reading.

Recommended workflow:
1. Keep data/vocabulary.csv as the source of truth for vocabulary data.
2. Upload the CSV and application files to GitHub Pages.
3. Use the trainer normally; learning data remains in the browser.
4. Periodically export learning statistics.
5. On a new browser/device, open the deployed trainer and import the learning backup. The current
   data/vocabulary.csv is loaded automatically by the application; it does not need to be uploaded manually
   through the Backup tab.

Backup compatibility with vocabulary changes:
- A backup does not need to contain every word currently present in data/vocabulary.csv.
- Existing matching words receive their saved learning statistics.
- New vocabulary that was not present when the backup was created simply starts without learning history.
- Backup records for words that no longer exist in the current vocabulary are ignored.
- Matching is based on normalized Japanese + reading, not on CSV row position.

LEARNING DATA STORAGE / MIGRATION
---------------------------------
The current browser storage key is:
    jvt_learning

The current learning-data schema version is 1.

The app also reads older learning-storage keys and migrates matching learning history where possible.
The stable match key remains Japanese + reading.

VOICE INPUT
-----------
Voice input is not part of the current application version.

DEVELOPMENT
-----------
This application was developed collaboratively by Dif and OpenAI's ChatGPT (AI).
ChatGPT assisted with application architecture, programming, database design, configuration,
and documentation. The project requirements, learning concepts, testing, decisions, and
customization are defined and directed by Dif.
