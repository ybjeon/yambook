---
description: Add a new row to places.csv from a free-form place description (name, naver map url, location, hours, parking).
---

Arguments: `$ARGUMENTS` — free-form text describing the place, in any order/format. Can be a single naver map URL, a comma/line-separated blob, or a short paragraph. Example: `스타벅스 강남역점, 카페, https://naver.me/xxxxx, 강남구 강남역, 07:00-22:00, 주차 가능`

## Steps

1. **Extract fields** from `$ARGUMENTS` for the six columns: 이름, 설명, 네이버지도url, 대략적인 위치, 오픈 시간, 주차.
   - Fields may appear in any order or be missing punctuation between them — infer by content (a `naver.me`/`map.naver.com` URL is 네이버지도url, a time range like `09:00-21:00` or `연중무휴` is 오픈 시간, words like `주차 가능`/`주차 불가`/`발렛` are 주차, etc).
   - 이름 is required. If it can't be determined, ask the user for it rather than guessing.
   - For any other field that's missing or unclear, leave it blank in the row rather than inventing a value — do not fabricate hours, parking info, or descriptions not present in the input.

2. **Check for duplicates**: Read `places.csv` and check whether a row already has the same 이름 or the same 네이버지도url. If so, tell the user and ask whether to add it anyway, update the existing row, or skip — don't silently create a duplicate.

3. **Append the row**: Add one line to `places.csv` with the six fields in column order (이름, 설명, 네이버지도url, 대략적인 위치, 오픈 시간, 주차). Quote a field with double quotes if it contains a comma, double quote, or newline (doubling any internal `"`), per standard CSV escaping. Leave unresolved fields as empty strings.

4. **Confirm**: Show the user the row that was added (or the duplicate-resolution outcome from step 2).

## Rules

- Never fabricate field values — leave blank rather than guess.
- Never overwrite or reorder existing rows; only append (except when the user explicitly chooses "update" in step 2).
- `places.csv` is at the project root.
