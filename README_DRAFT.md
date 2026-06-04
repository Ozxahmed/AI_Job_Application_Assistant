# AI Job Application Assistant

This project is a human-in-the-loop browser automation assistant for repetitive job application forms. The goal is not to mass-submit applications. The goal is to reduce manual form entry while keeping the applicant in control of review, edits, and final submission.

The assistant works from a structured applicant profile instead of a hard-coded dictionary. For the MVP, a dictionary can work, but the project is designed around reusable profile files that are easier to maintain, safer to inspect, and better suited for repeated applications.

## Profile Structure

Private applicant data should live in `profile/`:

```text
profile/
  personal_info.yaml
  work_experience.md
  projects.md
  skills.yaml
  questions.yaml
  resume.pdf
  cover_letter_info.md
```

Public starter templates live in `profile_templates/` so other users can copy them into their own private `profile/` folder.

## Profile Files

`profile/personal_info.yaml` stores structured personal and contact details such as name, email, phone, location, LinkedIn, GitHub, portfolio, work authorization, sponsorship status, relocation preference, and target roles.

`profile/work_experience.md` stores human-readable work history pulled from the resume, including roles, companies, dates, responsibilities, achievements, tools used, and education.

`profile/projects.md` stores project descriptions with status, links, focus areas, reusable blurbs, and notes about when each project is useful in an application.

`profile/skills.yaml` stores structured skills grouped by category, including programming languages, databases, cloud tools, orchestration/devops tools, BI tools, business domains, professional skills, and confirmed years of experience.

`profile/questions.yaml` stores reusable answers for repeated application questions, such as work authorization, sponsorship, relocation, salary expectations, years of experience, and short interest statements. Unknown questions should trigger a pause for user input, then the answer can be saved and reused.

`profile/resume.pdf` stores the source resume used by the assistant.

`profile/cover_letter_info.md` stores a short reusable background summary, cover statement, strongest themes, and role-specific adaptation notes for "why this role" and brief cover statement fields.

## Question-Answer Memory

Applications repeatedly ask the same questions:

- Are you authorized to work in the United States?
- Will you now or in the future require sponsorship?
- How many years of SQL experience do you have?
- How many years of Python experience do you have?
- Are you willing to relocate?
- What is your desired salary?
- Why are you interested in this role?

The assistant should answer known questions from `profile/questions.yaml`. If a question is unknown or marked as needing confirmation, the assistant should pause and ask the user. Once the user answers, the new answer can be added to the question memory for future applications.

## Cover Statement Generation

The MVP should keep cover statement generation simple. The assistant can generate a short, role-specific response using:

- job title
- company name
- job description
- the applicant's core experience block
- reusable themes from `profile/cover_letter_info.md`

The goal is a clear, useful paragraph, not a perfect cover letter.

Example:

```text
I am interested in this role because my background combines Python, SQL, data pipeline development, PostgreSQL, AWS, and dashboarding. I have built and maintained data workflows that supported reporting and operational decision-making, and I am looking to apply that experience in a role focused on reliable, scalable data systems.
```

## Application Workflows

Because job boards operate differently, the pipeline needs to handle two ingestion workflows.

### Individual ATS Links

Examples: Greenhouse, Workday.

For these, the user provides a direct job application link. The assistant navigates to the page, identifies the form fields, fills the application from the profile files, writes a brief cover statement when appropriate, and pauses before final submission.

### Aggregators

Examples: LinkedIn, Indeed.

These platforms wrap applications in their own interfaces. Two approaches are possible:

- Direct feed method: the user collects a small list of job URLs and the assistant works through them sequentially.
- Search-and-apply loop: the assistant searches a platform, chooses jobs, and applies filters before filling forms. This is more expensive and complex because it spends more time navigating search feeds.

## MVP Scope

1. Structured applicant profile files under `profile/`.
2. Public starter templates under `profile_templates/`.
3. Human-in-the-loop automation with the browser visible.
4. Form filling for a standard Greenhouse or Workday posting first.
5. Pause before final submission.
6. Question-answer memory for common application questions.
7. Simple brief cover statement generation from role details and reusable profile context.

## Safety Principles

- Never submit applications automatically.
- Pause when the assistant is uncertain.
- Store reusable answers explicitly rather than guessing.
- Keep private applicant data out of the public template folder.
- Review all generated application responses before submission.
