## Stage 1: Data cleaning and exploration

**Notebook:** `notebooks/01_explore_and_clean.ipynb`

### Findings

| Item | Result |
|------|--------|
| Raw dataset | 825 emails, 6 columns |
| Missing values | `subject` 43, `body` 11, `community_type` 47 |
| Raw label variants | 20 spellings for 4 true classes (case, separators, trailing spaces, abbreviations such as IL / AL / MC) |
| Missing labels | 47 = 31 truly empty + 16 `N/A` placeholders (Pandas hides `N/A` by default) |
| Duplicates removed | 24 |
| Final labeled dataset | 770 emails |
| Engineered features | `body_length` (mean 221 chars, max 302, 0 for empty bodies), `mentions_price` (614 of 770 emails, 79.7%), `month` (1 to 10) |
| Empty bodies | Some emails have a body length of 0, so for those rows the model only has the subject line |
|- `mentions_price` separates Other/Mixed (29% mention a price) from the three specific community types (91-93%), but it doesn't distinguish those three from each other, so the text itself has to carry that signal.|
|- 11 emails (1.4%) have an empty body, so the model must rely on the subject line alone for those rows.|

### Decisions

- **`N/A` mapped to Other/Mixed.** Of the 16 `N/A` emails, only 1 mentioned each community type, and the ones I read were newsletters or placement-advisor messages, so none promoted a single community type.
- **31 unlabeled rows set aside.** A supervised model can't learn from rows without labels. The missing labels looked random, so dropping them shouldn't bias the model.
- **Duplicates removed before any train/test split**, so copies of the same email can't appear in both sets and inflate the score (data leakage).

### Class balance after cleaning

| Class | Emails | Share |
|-------|-------:|------:|
| Independent Living | 293 | 38.1% |
| Assisted Living | 223 | 29.0% |
| Other/Mixed | 151 | 19.6% |
| Memory Care | 103 | 13.4% |

**Majority-class baseline accuracy: 0.381.** Any real model has to beat this.

### Observations that affect modeling

- Memory Care is only 13% of the data, so accuracy alone can hide poor performance on it. I'll track per-class recall and use stratified splits.
- Other/Mixed is a catch-all containing newsletters, placement advisors, multi-service campuses and `N/A` rows, so it's likely the hardest class to learn.
- Some emails have no subject or an empty body, so the model has to handle missing text.
- The emails come from a small set of templates, so scores on this data will be higher than on real inbox mail.