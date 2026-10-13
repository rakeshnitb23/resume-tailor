# Job application workflow

Full rules live in `RULES.md`. Read it before any resume or cover letter task; it overrides anything below if they differ.

## Setup check
- If `Resume_Original.md` does not exist, stop and help the user create it: copy `Resume_Original.example.md` and fill it with their real resume (or convert their resume PDF into this format). Never tailor from the example file.

## Resume
- Source of truth: `Resume_Original.md`. Tailored resumes are PDFs only: `Resume_<Company>/Resume_<Company>.pdf`.
- Never save `.md` files in this workspace. Write the Markdown in a temp folder and build: `python3 md2pdf.py /tmp/Resume_<Company>.md -o .` → `Resume_<Company>/Resume_<Company>.pdf`. On macOS this uses Safari's WebKit engine (needs PyObjC); elsewhere add `--engine chrome` (needs Google Chrome/Chromium).
- Tailor only from facts already in the original resume. You may add a Summary, reorder, and reword with the resume's own terms; never add skills, claims, or job-description words that aren't in it.
- Keep the header like the original; keep one A4 page.

## Cover letter
- When the user pastes a job's about/qualifications and asks for a cover letter, first read `Human_Style_Cover_Letter_Strategy.md` and follow it.
- Generate only what the user asks for, always with a PDF: a resume or cover letter request = write it, convert to PDF, save in the company's folder. No interest answer or other extras unless asked.
- Cover letter PDF: temp `CoverLetter_<Company>.md` → `python3 md2pdf.py <tmp>.md --css coverletter.css -o <tmp dir>` → move the PDF into `Resume_<Company>/`.
- Also save `CoverLetter_<Company>.txt` (250–350 words) in the same folder; `Interest_<Company>.txt` (4 sentences) only when asked.
- Candidate facts come only from the resume. No invented stories or stats.

## One folder per company
- Always `Resume_<Company>/`, for the resume and the cover letter alike, whichever is created first. Check whether it exists and reuse it. Never create `CoverLetter_<Company>/`.
- End every deliverable reply with the folder name, e.g. `Folder: Resume_Acme/`.
