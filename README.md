# AI_resume_filling_agent

Because of how job boards operate, the pipeline has to be designed to handle **two** completely different ingestion workflows:

```md
                  ┌───► 1. Individual ATS Links (Greenhouse / Workday)
                  │      ↳ Agent lands directly on form, fills it, and pauses.
──► TARGETS ──────┤
                  │
                  └───► 2. Aggregators (LinkedIn / Indeed)
                         ↳ Requires an orchestration pattern (Search ──► Loop ──► Apply)
```

The **Greenhouse** & **Workday** Workflow

- These are Applicant Tracking Systems (ATS). For these, you provide the direct link to the job posting.
- The script launches, navigates directly to the page, figures out where the application forms are, fills your details from top to bottom, writes the brief cover statement, and pauses right before hitting submit.

The **LinkedIn** & **Indeed** Workflow

- These platforms wrap the application process in their own proprietary UIs. You have two ways to handle them:
- The Direct Feed Method (Easier): You browse LinkedIn/Indeed on your regular browser, find 5 jobs you like, copy their URLs into a Python list in your script, and let the agent run through them sequentially.
- The Search-and-Apply Loop (Advanced): You instruct the agent: "Go to LinkedIn, search for 'Data Engineer', click the first 3 jobs that aren't 'Easy Apply', and fill out their application forms." This works, but it consumes far more tokens (and money) because the agent spends a lot of time just clicking around and navigating search feeds.

## Scope of the Project

### MVP

1. The **Baseline** Payload: A Python dictionary or markdown file containing standard personal details, contact links, and core technical experience block/resuem.

2. The **Human-in-the-Loop** Safeguard: The agent will run with headless=False (meaning the browser pops up on the monitor). It will fill everything out and stop at the final submit page. Once the work is reviewed and confirmed, user can submit the application and close the window.

3. MVP **Target**: A standard Greenhouse or Workday job link will be used first to prove out the parser before scaling to platform searches.
