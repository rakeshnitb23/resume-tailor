# Job Application Workflow

This file is the workflow for three tasks: tailor the resume to a job, write a cover letter, and write an interest-in-the-company answer. If a rule here conflicts with an explicit user request, ask the user before proceeding.

Cover letters and interest answers are not written from the summary in this file. Before either one, read `Human_Style_Cover_Letter_Strategy.md` in full and follow it. When `wellFound_session.md` is present, its session rules sit on top of this file (default interest answer, `WellFound/` folders).

## Files

| File or folder | Purpose |
|---|---|
| `Resume_Original.md` | The original resume and the **source of candidate facts** (tailored resumes may also carry JD skills the user approved; see Ground Rule 1). Never modified by tailoring. Read it in full before every resume, cover letter, or interest answer. If it is missing, stop and help the user create it from their real resume (convert their PDF into Markdown with the same header / Experience / Technical Skills / Education / Additional structure). |
| `Human_Style_Cover_Letter_Strategy.md` | The **only writing strategy** for cover letters and interest answers. Read it in full before every cover letter and every interest answer. Do not replace it with a shortened copy. |
| `wellFound_session.md` | Wellfound session overlay, when present: interest answer by default, company folders under `WellFound/`. |
| `md2pdf.py` | The Markdown-to-PDF script (Safari/WebKit on macOS, needs `markdown` + `pyobjc-framework-WebKit`; `--engine chrome` elsewhere). Reuse it; never rewrite it or create another conversion script. |
| `coverletter.css` | Styling for cover-letter PDFs, passed with `--css coverletter.css`. |
| `Resume_<Company>/` | The **single folder for that company**. All requested deliverables go here. Under the Wellfound overlay, that folder is `WellFound/Resume_<Company>/`. |

**Do not leave generated Markdown in this workspace.** Tailored-resume and cover-letter build files go in a temp folder (e.g. `/tmp`) and are not left behind. Do not delete project Markdown: this file, `RULES.md`, `README.md`, `Human_Style_Cover_Letter_Strategy.md`, `Resume_Original.md`, `Resume_Original.example.md`, or `wellFound_session.md`.

---

## Ground Rules

1. **Candidate facts come from `Resume_Original.md`, plus job-description skills the user approves.** Never add an employer, project, qualification, achievement, number, date, or personal story the original doesn't support. The one exception is skills and keywords from the job description: these may be added to a tailored resume **only through the JD-Gap Approval Loop** (Task 1), and they stay only if the user approves them in chat. An approval covers that one tailored resume. It never changes `Resume_Original.md`.
2. **Gaps go through the approval loop.** A job requirement the resume doesn't show is proposed to the user as `Added from JD: ...` or `Reworded to fit JD: ...`. Anything the user rejects, and anything not proposed (degrees, years of experience, languages, certifications, clearances), is reported with the exact label `Not in your resume: ...`. A skill the candidate is learning appears as a `Currently learning: ...` line only when `Resume_Original.md` contains that line or the user approves one.
3. You may reword, reorder, emphasize, or remove existing content to match the role. Do not change the meaning of the experience.
4. Keep every number exactly as in the original. No rounding, recalculating, inflating, or invented metrics.
5. **Keep every hyperlink intact.** Every markdown link in the original (email, LinkedIn, GitHub, project/repo URLs) stays a markdown link in the tailored resume, never plain text. Never invent a link.
6. Use the job description to decide what to emphasize. It is never evidence that the candidate has a skill. Only the user's approval is.
7. **Generate only what the user asks for, always with a PDF.** A resume-only request must not produce a cover letter, and vice versa. Create an interest answer only if explicitly asked.
8. Do not modify `Resume_Original.md`, `Human_Style_Cover_Letter_Strategy.md`, `md2pdf.py`, or unrelated files.
9. Report output paths, meaningful changes, missing qualifications, and PDF verification status. Never claim a file was created or verified if it wasn't.
10. **No silent job-description words.** A term that appears only in the job description may enter the tailored resume only as a listed, user-approved `Added from JD:` / `Reworded to fit JD:` item. Never slip one in unannounced (e.g. turning "LangGraph path" into "LangGraph agent path" without listing it). Before building, check every new word or phrase against `Resume_Original.md` and list every one that isn't there.
11. **End every deliverable reply with the folder name** as the last line, e.g. `Folder: Resume_Acme/`.
12. **Non-English job posts:** if the pasted job description is in another language, first translate it to English (show the user a short English version), work from the English version, then generate the deliverables. **Resumes, cover letters and interest answers are always written in English**, unless the user explicitly asks for another language. Missing language requirements (e.g. German fluency) are reported as `Not in your resume: ...` gaps.
13. **Open created files in VS Code only in the WellFound session.** When `wellFound_session.md` applies, open each deliverable in the user's current VS Code window with `code -r <path>` after it is created and verified (macOS, if `code` is not on PATH: `"/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code" -r <path>`). For a cover letter, open the `.txt`. Outside the WellFound session, do not open files. Exception: whenever the user asks for an interest answer, open its `.txt` after generating it, in any session.

### What tailoring may and may not change

| Allowed | Not allowed |
|---|---|
| Add a Summary built from resume facts and approved JD skills | Any title, employer, project, number or achievement not in the original |
| Reorder bullets, skills and skill groups to match the job | JD terms added without listing them for approval |
| Add or reword JD skills through the approval loop | Keeping a JD addition the user rejected |
| Rephrase bullets Bob-style: skill + plain-English business impact | Rewording that changes or inflates the meaning, or invents an impact |
| Merge duplicate bullets, shorten or drop the least relevant ones to fit one page | Changing, rounding or recalculating numbers |
| Keep every existing hyperlink as a markdown link | Flattening a hyperlink to plain text, or inventing a new one |

---

## One Folder per Company

1. Normalize the company name to CamelCase without spaces or symbols unless the user specifies a folder name. If the job post doesn't name the company, ask before creating the folder.
2. Always use `Resume_<Company>/`, for the resume and the cover letter alike, whichever is created first. Check whether it exists and reuse it.
3. Never create a `CoverLetter_<Company>/` folder, and never save duplicate copies elsewhere.
4. The folder contains only requested deliverables:

```text
Resume_Google/
├── Resume_Google.pdf
├── CoverLetter_Google.txt
├── CoverLetter_Google.pdf
└── Interest_Google.txt       # only if explicitly requested
```

---

## Task 1: Tailor a Resume

**Trigger:** the user provides job information and asks for a tailored resume.

1. Read `Resume_Original.md` in full.
2. Identify the role's priorities, required skills, responsibilities, and keywords (the "hamburgers").
3. **JD gap check.** Compare every required skill or keyword with `Resume_Original.md` and sort it:
   - **Present:** already on the resume. Surface it near the top.
   - **Sub-topic:** fits inside something already on the resume (e.g. JD "Pandas" under resume "Python data processing"). Plan a reword of that existing item.
   - **Missing:** not on the resume at all. Plan where it fits most relevantly (the bullet or skill group it belongs with).
   - **Not proposable:** degrees, years of experience, languages, certifications, clearances, employers. Report as `Not in your resume: ...`, never added.
4. Choose the folder name; reuse `Resume_<Company>/` if it exists.
5. Write the tailored Markdown at `/tmp/Resume_<Company>.md` (never in the workspace), applying **Bob-Style Rephrasing** (below) to every bullet:
   - **Summary:** lead with the job's must-have keywords that the resume (or an approved addition) supports.
   - **Experience:** reorder and rephrase bullets so the most relevant evidence comes first. Add "Missing" skills into the bullet they fit, and reword "Sub-topic" items to use the JD's term.
   - **Technical Skills:** put the job's required skills in the first group. Add "Missing" skills to the group they belong to.
   - **Education and Additional:** preserve the original facts.
6. Keep the header exactly like the original: names, places and contact details spelled as written (even if they look like typos), with every hyperlink as a markdown link.
7. Build: `python3 md2pdf.py /tmp/Resume_<Company>.md -o .` → `Resume_<Company>/Resume_<Company>.pdf`
8. Verify the PDF exists, is readable, and is exactly **one A4 page**. If too long, trim the least relevant wording and rebuild without removing or distorting facts.
   - Gotcha: a 2-page PDF with a blank second page means page 1 is over-full by a line. Trim a short trailing line (a bullet or Summary line ending in one or two words) and rebuild.
9. **JD-Gap Approval Loop.**
   - **Any "Missing" or "Sub-topic" changes made:** open the PDF (`open Resume_<Company>/Resume_<Company>.pdf`, in any session) and list every change in chat, one per line, as `Added from JD: <term> → <where>` or `Reworded to fit JD: <old> → <new>`. Ask the user to approve, reject or edit each one. **Stop and wait for the reply.** Then remove rejected items, apply edits, rebuild, re-verify one page, and finalize. Repeat if the user asks for more changes.
   - **No gaps and nothing out of line with the resume:** skip approval. Rephrase Bob-style and finalize directly.
10. Report the path, main changes, approved JD additions, `Not in your resume: ...` gaps, and verification result.

### Bob-Style Rephrasing

"Bob" is the future boss or recruiter reading the resume: not technical, scanning fast. Bob hires for **hamburgers** (the exact skills the job asks for) and needs to see **why each one mattered to the business**.

- **Every bullet = hamburger + business impact.** Name the skill or tool the job asks for, then say in plain English what it achieved: time saved, cost cut, risk reduced, users served, decisions enabled. Keep the original numbers exactly.
- **Impact comes from the resume, never invented.** If the original states no result, describe the plain purpose ("so analysts could find papers in seconds"), not a made-up metric.
- **No tool chains without an outcome.** Bad: "Engineered a resumable Python ingestion pipeline using arXiv API, Requests and BeautifulSoup with JSON checkpoints into SQLite." Better: "Built a Python and SQL pipeline that pulls research papers automatically, removing hours of manual collection."
- **Cut jargon** a non-technical reader can't follow. Keep the keyword itself (ATS and Bob both scan for it) and drop the internal detail around it.
- **Must-have keywords high up:** Summary, the first bullets of the most relevant role, the first skill group. Never bury a required skill at the bottom.
- **Communication:** show "explaining technical work to non-technical people" only when the resume supports it.
- **Relocation:** if the user says they are relocating, add under the header: `Moving to <place> by <date>, can move earlier if needed`. Only with the place and date the user gives.

**Format:** match `Resume_Original.md`. Section order (where present): `Summary`, `Experience`, `Technical Skills`, `Education`, `Additional`. Same role/company heading style, italic dates line, bullets, and grouped skill lines.

---

## Task 2: Write a Cover Letter

**Trigger:** the user pastes a job's about/qualifications and asks for a cover letter.

1. Read `Human_Style_Cover_Letter_Strategy.md` in full, then read `Resume_Original.md` in full. A tailored resume PDF may give context, but the original Markdown is the only source of facts. Follow that strategy file for the hook, the research, the paragraph order, and the tone. Do not copy its example opening.
2. Extract from the job/company context: exact company name and job title; what the company does for its users; one specific, checkable company signal (mission wording, product feature, culture detail); the top requirements; remote / time-zone notes.
3. Write **250–350 words** in 3–4 short paragraphs, as the strategy file specifies:
   - **Opening:** a fresh, human, non-technical observation tied directly to a real company signal. Never "I am writing to apply", never lead with skills/tools.
   - **Evidence:** 1–2 of the strongest relevant resume-backed examples, numbers exact.
   - **Why this company:** the connection to their actual work. Mention remote readiness, time-zone overlap, written communication, or independent delivery only if the resume or the user supports it.
   - **Close:** thanks and a clear next step.
4. No invented stories, feelings, statistics, hardships, experience, or skills. If a required skill is missing, at most say honestly the candidate would approach it as a learner.
5. Save `Resume_<Company>/CoverLetter_<Company>.txt`.
6. Write `/tmp/CoverLetter_<Company>.md` with text identical to the `.txt`, then build: `python3 md2pdf.py /tmp/CoverLetter_<Company>.md --css coverletter.css -o /tmp` and move the PDF to `Resume_<Company>/CoverLetter_<Company>.pdf`.
7. Verify the PDF exists, is one page, and matches the TXT. Report both paths and the word count.

**TXT layout.** No "COVER LETTER" title or "Role: ..." line; start directly with the greeting:

```text
Dear <Company> Hiring Team,

<paragraph 1: human hook>

<paragraph 2: resume evidence>

<paragraph 3: why this company and remote readiness, if applicable>

<closing line and thanks>

Sincerely,
<candidate's exact name and contact details from Resume_Original.md>
```

The writing strategy is `Human_Style_Cover_Letter_Strategy.md`, read in full before this task. Use its first principles, research questions, hook structure, and reminders. Do not copy the example opening in that file.

---

## Task 3: Interest-in-the-Company Answer

When this task is requested, or when `wellFound_session.md` makes it the default, read `Human_Style_Cover_Letter_Strategy.md` in full first. Follow its **Short "Interest in the Company" Answer** section. Do not write the answer from memory of this file.

- Save as `Resume_<Company>/Interest_<Company>.txt`. No PDF unless asked.
- **4–5 sentences**, in the order that section gives:
  1. A specific, human, non-technical observation or company signal, like a cover letter hook. Not a restated job-description line or a credentials lead-in.
  2. Why it resonates with the candidate (genuine, not a skills statement).
  3. What the candidate can contribute, only from `Resume_Original.md`.
  4. A brief close.
- Technical contribution belongs in sentence 3 only.

---

## Final Checklist

- [ ] `Resume_Original.md` read in full and used as the fact source; it was not modified.
- [ ] `Human_Style_Cover_Letter_Strategy.md` read in full before any cover letter or interest answer.
- [ ] No unsupported employers, projects, numbers, dates or stories. Every JD-only skill or word was listed and approved by the user; rejected ones removed.
- [ ] Every bullet is Bob-style: required skill + plain-English business impact, keywords near the top.
- [ ] If JD additions were made: PDF opened, approval asked in chat, reply received before finalizing.
- [ ] Every original hyperlink is still a markdown link.
- [ ] Remaining gaps listed as `Not in your resume: ...`.
- [ ] Only requested documents generated, all in the one `Resume_<Company>/` folder.
- [ ] `md2pdf.py` reused; no generated resume or cover-letter Markdown left in the workspace. Project Markdown was not deleted.
- [ ] Resume PDF is exactly one A4 page; cover letter is 250–350 words, one-page PDF, TXT and PDF match.
- [ ] In the WellFound session, every created file was opened in the current VS Code window.
- [ ] Report includes paths, changes, gaps, honest verification, and ends with `Folder: Resume_<Company>/`.
