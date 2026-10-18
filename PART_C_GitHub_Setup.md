# Part C — GitHub Setup Checklist

## Suggested repository name
`ai-engineering-scholarguide-my`

## Suggested structure
```text
ai-engineering-scholarguide-my/
├── README.md
├── docs/
│   ├── week1_observation_sheet.md
│   ├── week1_system_map.png
│   └── week1_project_scope.md
├── data/
├── src/
├── notebooks/
├── evaluation/
└── prompts/
```

## Steps
1. Create the repository on GitHub with the suggested name.
2. Add a README file. Use Public visibility only if all files are suitable for public sharing.
3. Clone it:
   ```bash
   git clone <YOUR_REPOSITORY_HTTPS_URL>
   cd ai-engineering-scholarguide-my
   mkdir -p docs data src notebooks evaluation prompts
   ```
4. Copy your Part A worksheet and Part B system map into `docs/`.
5. Copy `week1_project_scope.md` into `docs/` and `README.md` into the repository root.
6. Commit and push:
   ```bash
   git add README.md docs/
   git commit -m "Week 1: observation sheet, system map, project scope and README"
   git push
   ```
7. Submit your GitHub repository URL on the course portal.

## Safety reminders
- Never commit API keys, passwords, tokens, personal identity information, or confidential documents.
- Do not redistribute copyrighted documents unless the licence/terms permit it.
- Keep a source log with each document title, URL, access date, and update date.
