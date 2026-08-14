Japanese Vocabulary Trainer v10

Upload these files to the root of the GitHub Pages repository:
- index.html
- vocabulary.csv
- last-layout.txt

Keep your existing manifest.json and icon.svg.

DATABASE
- vocabulary.csv is the source of the vocabulary.
- It is a standard UTF-8 CSV file and can be edited in Excel, Numbers, LibreOffice, etc.
- The first row defines the database columns.
- Add a new column by adding its field name to the header and adding one value in the same position on every word row.
- New columns are automatically available to the Last-layout config and the Modify button.
- Current extra columns include adjective_group, additional_info, stem, masu, te, nai and past.
- Empty cells are fine.
- Statistics are kept separately in the browser and matched by Japanese + reading.

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
- Direction: Japanese -> English, English -> Japanese, Mixed.
- Vocabulary filter: All, Verbs, Adjectives, Nouns, Phrases.

CSV NOTES
- Use UTF-8 encoding so Japanese text is preserved.
- Commas inside a value are automatically handled when the field is quoted by your spreadsheet program.
- If you add a column, give every row a value (or leave the cell empty).
