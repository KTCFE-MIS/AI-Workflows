# Skills

Claude Code loads project skills from this folder. The skill files themselves live in `.github/skills/`, which is the source of truth.

| Skill | Source | How it is linked |
| --- | --- | --- |
| `reusable-prototype` | `.github/skills/reusable-prototype/` | Junction |
| `match-prototype` | `.github/skills/match-prototype/` | Junction |
| `guided-tdd` | `.github/skills/guided-tdd/` | Junction |
| `story-to-implementation-spec` | `.github/skills/story-to-implementation-spec/` | Junction |

Each skill folder here is a junction to the matching folder in `.github/skills/`. The files you see under it are the same files as the ones in `.github`.

The junctions are not committed (see `.gitignore`). After cloning, create them from the repo root in PowerShell:

```powershell
Get-ChildItem .github\skills -Directory | ForEach-Object { New-Item -ItemType Junction -Path ".claude\skills\$($_.Name)" -Target $_.FullName }
```

To change a skill, edit the file in `.github/skills/`, not the copy under `.claude/skills/`.

To remove a link without touching the source, run `Remove-Item .claude\skills\<skill>`. Do not delete the folder with `Remove-Item -Recurse`, because that can delete the files in `.github`.
