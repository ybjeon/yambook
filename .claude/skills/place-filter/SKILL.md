---
description: Filter places.csv by a natural-language condition and save the matching rows as a markdown file under blog/<filter>.md.
---

Arguments: `$ARGUMENTS` — a free-form filter condition (Korean or English), e.g. `강남 분당 대형 카페`, `데이트 야경`, `주차 가능한 맛집`.

## Steps

1. **Read the source**: Read `places.csv` from the project root. Columns: 이름, 설명, 네이버지도url, 대략적인 위치, 오픈 시간, 주차.

2. **Match rows against the condition**: The CSV has no explicit category/size/mood columns, so use judgment — not exact string matching — to decide which rows satisfy `$ARGUMENTS`:
   - Use 이름, 설명, 대략적인 위치 as the primary signals (e.g. "카페" matches names/descriptions implying a cafe; "강남" or "분당" matches location text mentioning those areas or known neighborhoods within them).
   - Combine multiple conditions as AND (e.g. "강남 분당 대형 카페" = must plausibly be a large cafe located in Gangnam or Bundang).
   - If a condition can't be evaluated from the available columns (e.g. no way to know "large" from the data), use general knowledge about the place name if you recognize it; otherwise don't exclude solely on that unverifiable criterion — note the limitation in the confirmation message.

3. **Determine the output path**: Slugify `$ARGUMENTS` (trim, collapse whitespace to single hyphens, strip characters illegal in filenames; keep Korean characters as-is) and write to `blog/<slug>.md`. If the file already exists, overwrite it.

4. **Write the markdown file**:
   ```markdown
   # <filter condition as given>

   <N>곳

   | 이름 | 설명 | 위치 | 오픈 시간 | 주차 |
   |---|---|---|---|---|
   | [이름](네이버지도url) | 설명 | 위치 | 오픈 시간 | 주차 |
   ...
   ```
   - Link 이름 to 네이버지도url when the URL is present; otherwise show plain text.
   - If no rows match, still create the file with the title and a line stating no matches were found.

5. **Confirm**: Report the output path and number of matches to the user, and mention any condition you couldn't verify from the data (per step 2).

## Rules

- Never modify `places.csv`.
- `blog/` and `places.csv` are both at the project root.
