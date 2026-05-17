
# Syrian Parliament Elections 2025 — Data Extraction & Analysis

End-to-end Python pipeline that extracts candidate and voter records from 24 official Arabic-language PDFs published for Syria's 2025 People's Assembly elections, merges the records into a clean dataset, and visualises the results in a Power BI dashboard.

---

## Background

In 2025, Syria held parliamentary elections under a two-tier indirect system. Electoral colleges in each governorate first selected a body of voters (الهيئة الناخبة), who then elected members of the People's Assembly (مجلس الشعب). Three sets of official documents were published across the process:

- **Preliminary voter lists** (12 PDFs, one per governorate)
- **Final voter lists** (11 PDFs, post-exclusions and additions)
- **Master candidates list** (1 PDF, all 1,571 candidates nationally)

All documents were published in Arabic by the Higher Committee for the Elections of the People’s Assembly. This project extracts the data from these PDFs and turns it into a structured, analysable dataset.

---

## What the pipeline does

1. **Parses 24 native-text Arabic PDFs** using `pdfplumber`.
2. **Handles Arabic text correctly** with `arabic_reshaper` and `python-bidi` for shaping and right-to-left rendering.
3. **Extracts records line by line** using regex parsers that handle three different row formats found across the documents (name + birthplace + slash + year; name + birthplace + year; name + year only).
4. **Normalises inconsistent district-header phrasing** across governorates (e.g. two different word orderings of *الدائرة الانتخابية في*).
5. **Merges records on a composite key** (name + city + district + birthplace + birth_year) across the three list types, with binary flags (`in_first`, `in_final`, `is_candidate`) tracking each person through the electoral process.
6. **Exports a clean UTF-8 CSV** ready for analysis in Power BI or any other tool.

---

## Dashboard

The Power BI dashboard analyses the cleaned dataset across several dimensions:

- **Headline counts:** 5,935 voters in the preliminary list, 5,915 in the final list, 877 excluded, 857 added, 1,571 total candidates, 21.49% candidate-to-voter ratio.
- **Seat distribution by governorate** (treemap): Aleppo (22.86%), Idlib (8.57%), Hama (8.57%), Homs (8.57%), Damascus countryside (8.57%), Damascus (7.14%), Hasakah (7.14%), Tartus (7.14%), and others.
- **Candidate counts by city / electoral district** (sortable table).
- **Age distribution** of candidates and voters, in five bands (20–25, 25–34, 35–44, 45–59, 60+).
- **Gender breakdown** — candidates: 86.38% men / 13.62% women; voters: 81.97% men / 18% women.
- **Geographic map** of candidates by city.
- **Governorate filter** for interactive drill-down.

A screenshot is included in `/screenshots/dashboard.png`. The full `.pbix` file is in the repo for anyone with Power BI Desktop.

---

## Tech stack

- **Python** (Pandas, regex)
- **pdfplumber** — PDF text extraction
- **arabic_reshaper** — Arabic character shaping
- **python-bidi** — bidirectional text handling
- **Power BI** — visualisation

---

## Repository structure

```
syrian-parliament-2025-extraction/
├── README.md
├── notebook/
│   └── Syrian_Parliament_Election_2025.ipynb
├── data/
│   └── clean_master.csv
├── dashboard/
│   └── syria_elections_2025.pbix
└── screenshots/
    └── dashboard.png
```

The 24 source PDFs are not stored in this repository; they are publicly available from the official Syrian electoral authorities.

---

## Notes on methodology

- The PDFs were assessed as native text and parsed directly with `pdfplumber`; no OCR was needed.
- Regex parsers were written to handle three distinct row formats observed across the documents, rather than assuming a single layout.
- District-header phrasing was normalised before extraction to keep `district` values consistent across governorates.
- The composite key used for merging is intentionally strict (five fields) to avoid false matches between people with the same name in different districts.

---

## Author

Heba Ajami — Data Scientist | Data Analyst
[LinkedIn](https://linkedin.com/in/heba-ajami-502009ab) · [GitHub](https://github.com/Heba-Ajami)
