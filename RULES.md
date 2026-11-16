# Rules for AI Assistants: Job Applications

Give this file to any AI working in this folder. It defines two tasks:

1. Tailor the existing resume to a job.
2. Write a company-specific cover letter.

Follow these rules. If a rule here conflicts with an explicit user request, ask the user before proceeding.

---

## Files in This Workspace

| File or folder | Purpose |
|---|---|
| `Resume_Original.md` | The original resume and the **source of candidate facts** (tailored resumes may also carry JD skills the user approved; see Ground Rule 1). Never modified by tailoring. Read it in full before tailoring a resume or writing a cover letter. |
| `Human_Style_Cover_Letter_Strategy.md` | The cover-letter strategy. Read it in full before every cover letter. |
| `md2pdf.py` | The existing Markdown-to-PDF conversion script (Safari/WebKit on macOS by default, `--engine chrome` elsewhere). Reuse it; do not rewrite it or create another conversion script. |
| `coverletter.css` | Styling for cover-letter PDFs. Pass it to `md2pdf.py` with `--css coverletter.css`. |
| `Resume_<Company>/` | The **single canonical folder for that company**. All requested deliverables for that company belong here. |

Temporary Markdown files are build inputs only. Create them outside this workspace (for example, in `/tmp`) and do not leave them in the workspace.

---

## Ground Rules

1. **Candidate facts come from `Resume_Original.md`, plus job-description skills the user approves.** Never add an employer, project, qualification, achievement, number, date, or personal story the original resume does not support. The one exception is skills and keywords from the job description: these may be added to a tailored resume **only through the JD-Gap Approval Loop** (Task 1), and they stay only if the user approves them. An approval covers that one tailored resume. It never changes `Resume_Original.md`.
2. **Gaps go through the approval loop.** A job requirement the resume does not show is proposed to the user as `Added from JD: ...` or `Reworded to fit JD: ...`. This includes **nearby skills**: a JD skill that is even loosely close to one on the resume (e.g. JD "Azure" next to resume "AWS") is proposed, never skipped. Anything the user rejects, and anything not proposed (degrees, years of experience, languages, certifications, clearances), is reported with the exact label `Not in your resume: ...`. A skill the candidate is learning appears as a `Currently learning: ...` line only when `Resume_Original.md` contains that line or the user approves one.
3. You may reword, reorder, emphasize, or remove existing content to match the role. Do not change the meaning of the candidate's experience.
4. Keep every number exactly as it appears in the original resume. Do not round, recalculate, inflate, or invent metrics.
5. **Keep every hyperlink intact.** Wherever `Resume_Original.md` has a markdown link (email, LinkedIn, GitHub, project/repo URLs), the tailored resume must carry that same markdown link syntax, not plain text. Never strip a link down to bare text, and never invent a link that is not in the original.
6. Use the company's job description to decide what to emphasize. It is never evidence that the candidate has a skill or experience. Only the user's approval is.
7. Generate only the document types the user requests. A resume-only request must not produce a cover letter; a cover-letter-only request must not produce a resume. Create an interest answer only if explicitly requested.
8. Reuse the existing `md2pdf.py` script for PDF generation. Do not write a new conversion script or modify the existing one.
9. Do not modify the original resume, strategy file, conversion script, or unrelated files.
10. Report the output paths, meaningful changes, missing qualifications, and PDF verification status. Never claim a file was created or verified if it was not.
11. **No silent job-description words.** A term that appears only in the job description may enter the tailored resume only as a listed, user-approved `Added from JD:` / `Reworded to fit JD:` item. Never slip one in unannounced (e.g. turning "LangGraph path" into "LangGraph agent path" without listing it). Before building, check every new word or phrase against `Resume_Original.md` and list every one that isn't there.
12. **End every deliverable reply with the folder name**, as the last line, e.g. `Folder: Resume_Acme/`.
13. **Open created files in VS Code only in the WellFound session.** When `wellFound_session.md` applies, open each deliverable in the user's current VS Code window with `code -r <path>` after it is created and verified (macOS, if `code` is not on PATH: `"/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code" -r <path>`). For a cover letter, open the `.txt`. Outside the WellFound session, do not open files.

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

## Company Folder Rule: One Folder Only

**There must be one folder per company, regardless of which document is requested first.**

1. Normalize the company name to CamelCase without spaces or symbols unless the user specifies a folder name. If the job post doesn't name the company, ask the user before creating the folder.
2. Use `Resume_<Company>/` as the canonical folder for all deliverables.
3. Before creating it, check whether `Resume_<Company>/` already exists. Reuse it if it does; create it only if it does not.
4. Never create a separate `CoverLetter_<Company>/` folder.
5. If the user requests a cover letter first, save it in `Resume_<Company>/`. If they later request a resume for the same company, add the resume PDF to that existing folder.
6. If the user requests a resume first, save a later cover letter in that same folder.
7. Never save duplicate copies of the same deliverable in another location.

Example folder contents when both are requested:

```text
Resume_Google/
├── Resume_Google.pdf
├── CoverLetter_Google.txt
├── CoverLetter_Google.pdf
└── Interest_Google.txt       # only if explicitly requested
```

The folder should contain only the deliverables the user requested.

---

## Task 1: Tailor a Resume

**Trigger:** The user provides company/job information and asks for a tailored resume.

### Steps

1. Read `Resume_Original.md` in full.
2. Read the job information and identify the role's priorities, required skills, responsibilities, and relevant keywords (the "hamburgers").
3. **JD gap check.** Compare every required skill or keyword with `Resume_Original.md` and sort it:
   - **Present:** already on the resume. Surface it near the top.
   - **Sub-topic:** fits inside something already on the resume (e.g. JD "Pandas" under resume "Python data processing"). Plan a reword of that existing item.
   - **Nearby:** not on the resume, but even loosely close to a skill that is. Examples: JD "Azure", "Azure OpenAI" or "Azure AI Services" next to resume "AWS" / "AWS Bedrock"; "LangSmith" next to "Langfuse"; "Flask" next to "FastAPI"; "GraphRAG" next to "RAG"; "Microsoft Copilot Studio" next to "LangGraph agents" / "AgentCore". Always propose these. Mark loose matches as *(weaker match)* so the user can reject them.
   - **Missing:** not on the resume at all and not near anything on it. Plan where it fits most relevantly.
   - **Not proposable:** degrees, years of experience, languages, certifications, clearances, employers. Report as `Not in your resume: ...`, never added.
4. Choose the company folder name, then check whether `Resume_<Company>/` already exists. Reuse it if it does.
5. Create the tailored resume Markdown in a temporary directory outside the workspace, for example `/tmp/Resume_<Company>.md`. Never save the tailored `.md` file in the workspace. Apply **Bob-Style Rephrasing** (below) to every bullet:
   - **Summary:** Lead with the job's must-have keywords that the resume (or an approved addition) supports.
   - **Experience:** Reorder and rephrase bullets so the most relevant evidence appears first. Add "Missing" skills into the bullet they fit, and reword "Sub-topic" items to use the JD's term.
   - **Technical Skills:** Put the job's required skills in the first group. Add "Missing" and "Nearby" skills to the group they belong to, or to a new group when several belong together (e.g. `Microsoft & Azure:`). "Nearby" skills go only in Technical Skills, never into Summary or experience bullets, because bullets describe work the candidate actually did.
   - **Education and Additional:** Preserve the original facts; do not invent or infer details.
6. Preserve the original resume's header and formatting unless the user asks for a change. Keep names, places and contact details spelled exactly as the original has them, even if they look like typos, and keep every hyperlink as a markdown link.
7. Convert the temporary Markdown file using the existing script:

   ```bash
   python3 md2pdf.py /tmp/Resume_<Company>.md -o .
   ```

   Run the existing `md2pdf.py` script from this workspace. Do not rewrite it or create another conversion script.

8. Ensure the PDF is saved at `Resume_<Company>/Resume_<Company>.pdf`.
9. Verify that the PDF exists, is readable, and is exactly one A4 page. If it is too long, trim the least relevant existing wording and rebuild; do not remove or alter facts in a misleading way.
   - Gotcha: a 2-page PDF whose second page is blank means page 1 is over-full by a line or so. Trim a short trailing line (a bullet or Summary line ending in one or two words) and rebuild.
10. **JD-Gap Approval Loop.**
    - **Any "Sub-topic", "Nearby" or "Missing" changes made:** open the PDF for the user and show every change under the heading **Adding to your resume (approve or reject)**, as a table (format below). Ask the user to approve, reject or edit each row. **Stop and wait for the reply.** Then remove rejected items, apply edits, rebuild, re-verify one page, and finalize.
    - **No gaps and nothing out of line with the resume:** skip approval. Rephrase Bob-style and finalize directly. The reply still shows the heading, followed by "Nothing. Every word comes from your original resume."
11. Report the saved path, the main tailoring changes, the **Adding to your resume** table, any `Not in your resume: ...` gaps, and whether PDF verification succeeded.

### Adding-to-Your-Resume Table

Every tailored-resume reply includes this section, placed before `Not in your resume`. One row per word or skill that is not in `Resume_Original.md`, numbered across rebuilds (already-approved rows are noted as approved, not asked again):

```text
| # | Added or changed | Section line (exact wording, new part in bold) | Your original had | Close to |
|---|---|---|---|---|
| 1 | Flask | Backend: "Python (FastAPI, **Flask**, Pydantic)" | "Python (FastAPI)" | FastAPI |
| 2 | Azure OpenAI | Microsoft & Azure: "**Azure OpenAI**, Azure AI Services" | not there | OpenAI, AWS Bedrock |
```

- **Your original had:** the original wording, or "not there".
- **Close to:** the resume skill the addition maps to; add *(weaker match)* for loose ones; "—" for a reworded sub-topic.
- Below the table, say whether the Summary or experience bullets gained anything new, and list anything trimmed to fit one page.
- If the user asks what was added, answer with this table.

### Bob-Style Rephrasing

"Bob" is the future boss or recruiter reading the resume: not technical, scanning fast. Bob hires for **hamburgers** (the exact skills the job asks for) and needs to see **why each one mattered to the business**.

- **Every bullet = hamburger + business impact.** Name the skill or tool the job asks for, then say in plain English what it achieved: time saved, cost cut, risk reduced, users served, decisions enabled. Keep the original numbers exactly.
- **Impact comes from the resume, never invented.** If the original states no result, describe the plain purpose, not a made-up metric.
- **No tool chains without an outcome.** Bad: "Engineered a resumable Python ingestion pipeline using arXiv API, Requests and BeautifulSoup with JSON checkpoints into SQLite." Better: "Built a Python and SQL pipeline that pulls research papers automatically, removing hours of manual collection."
- **Cut jargon** a non-technical reader can't follow. Keep the keyword itself and drop the internal detail around it.
- **Must-have keywords high up:** Summary, the first bullets of the most relevant role, the first skill group.
- **Communication:** show "explaining technical work to non-technical people" only when the resume supports it.
- **Relocation:** if the user says they are relocating, add under the header: `Moving to <place> by <date>, can move earlier if needed`, using only the place and date the user gives.

### Resume Format

Match the structure and visual conventions of `Resume_Original.md`:

- Keep the original header and contact details unchanged, including their markdown hyperlinks (email, LinkedIn, GitHub, project/repo links).
- Keep sections in this order when those sections exist in the original: `Summary`, `Experience`, `Technical Skills`, `Education`, `Additional`.
- Each job should use the original role/company heading style, italic dates line, and bullets.
- Skills should use the original grouped-line style.
- Target one A4 page.

Do not create a resume if the user requested only a cover letter.

---

## Task 2: Write a Cover Letter

**Trigger:** The user provides company information and asks for a cover letter.

### Steps

1. Read `Human_Style_Cover_Letter_Strategy.md` in full every time before writing.
2. Read `Resume_Original.md` in full to verify candidate facts. A tailored resume PDF may be consulted for context, but the original Markdown resume remains the only source of candidate facts.
3. Extract from the job/company context:
   - Exact company name and job title.
   - What the company does for its users or customers.
   - One specific, checkable company signal, such as mission wording, a product feature, or a stated culture detail.
   - The top job requirements.
   - Remote or time-zone requirements, if relevant.
4. Write a cover letter of **250–350 words** in 3–4 short paragraphs, following the strategy file:
   - **Opening:** Begin with a fresh, human, non-technical observation tied directly to a real company-specific signal. Do not begin with "I am writing to apply" or lead with skills/tools.
   - **Evidence:** Use 1–2 of the strongest relevant resume-backed examples. Preserve their real numbers exactly.
   - **Why this company:** Explain the connection to the company's actual work. Mention remote readiness, time-zone overlap, written communication, or independent delivery only if supported by the resume or explicitly stated by the user.
   - **Close:** Thank the reader and state a clear next step.
5. Do not invent personal stories, feelings, statistics, hardships, experience, or skills. If a required skill is missing, state honestly that the candidate would approach it as a learner only when appropriate; do not present it as an existing skill.
6. Create a plain-text version at `Resume_<Company>/CoverLetter_<Company>.txt`.
7. Create a temporary Markdown build file outside the workspace, such as `/tmp/CoverLetter_<Company>.md`. Its text must match the `.txt` version, including the role, greeting, body, and signature.
8. Convert it with the existing script and stylesheet:

   ```bash
   python3 md2pdf.py /tmp/CoverLetter_<Company>.md --css coverletter.css -o /tmp
   ```

   Move the generated PDF into `Resume_<Company>/CoverLetter_<Company>.pdf`.

9. Verify that the PDF exists, is readable, and is one page. Check that the text matches the TXT version.
10. Report both file paths and the cover letter's word count.

Do not create a tailored resume if the user requested only a cover letter.

### Cover Letter Text Layout

`CoverLetter_<Company>.txt`:

```text
Dear <Company> Hiring Team,

<paragraph 1: human hook>

<paragraph 2: resume evidence>

<paragraph 3: why this company and remote readiness, if applicable>

<closing line and thanks>

Sincerely,
<Use the candidate's exact name and contact details from Resume_Original.md>
```

Use the candidate's real name and contact details from the original resume. Do not copy example contact details from this rules file.

---

## Task 3: Interest-in-the-Company Answer

Only create this answer when the user explicitly asks for it.

- Read `Human_Style_Cover_Letter_Strategy.md` in full before writing, same as for a cover letter. The interest answer is governed by that file's "Short Interest in the Company Answer" section, not written ad hoc.
- Save it as `Resume_<Company>/Interest_<Company>.txt`.
- Write **4–5 sentences**, in this order:
  1. A specific, human, non-technical observation or company signal — the same kind of quiet, observational hook used to open a cover letter. Do not start with a restated job-description line or a credentials/technical lead-in.
  2. Why it resonates with the candidate — genuine, not a skills statement.
  3. What the candidate can contribute, based only on `Resume_Original.md`.
  4. A brief close.
- The opening sentence must read like the cover letter's human hook (sentence 1 of Paragraph 1 in Task 2), not like the body/evidence paragraph. Technical contribution belongs in sentence 3, not sentence 1 or 2.
- Use the same truthful, human tone as the cover letter.
- Do not create a PDF for the interest answer unless the user explicitly asks for one.

---

## Final Checklist

Before reporting completion, confirm:

- [ ] `Resume_Original.md` was read in full, used as the source of candidate facts, and not modified.
- [ ] The cover-letter strategy was read in full if a cover letter was requested.
- [ ] No unsupported employers, projects, qualifications, achievements, numbers, dates, or stories were added. Every JD-only skill or word was listed and approved by the user; rejected ones were removed.
- [ ] Every bullet is Bob-style: required skill + plain-English business impact, keywords near the top.
- [ ] Nearby skills (even loose matches, e.g. Azure for AWS) were proposed, only in Technical Skills.
- [ ] The reply shows the **Adding to your resume** table (or "Nothing...") before `Not in your resume`.
- [ ] If JD additions were made, the PDF was opened and the user's approval was received before finalizing.
- [ ] Every hyperlink present in `Resume_Original.md` is still a markdown link in the tailored resume, not plain text.
- [ ] Missing requirements are listed as `Not in your resume: ...`.
- [ ] Only the requested document types were generated.
- [ ] All deliverables for the company are saved in the one canonical `Resume_<Company>/` folder.
- [ ] No separate `CoverLetter_<Company>/` folder or duplicate deliverable was created.
- [ ] The existing `md2pdf.py` script was reused.
- [ ] Temporary Markdown files were kept outside the workspace and were not left in it.
- [ ] The resume PDF is exactly one A4 page if a resume was requested.
- [ ] The cover letter is 250–350 words and its PDF is one page if a cover letter was requested.
- [ ] The cover-letter TXT and PDF contain matching text.
- [ ] In the WellFound session, every created file was opened in the current VS Code window.
- [ ] The final report includes file paths, changes made, missing qualifications, and honest PDF verification results.
