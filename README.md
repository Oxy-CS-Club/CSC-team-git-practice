# CSC Team Git Practice

Practice repo for the Oxy CS Club project team. Here we learn how 8–10 people can work on the same code without breaking each other's work.

## Start here

1. **[docs/SOP.md](docs/SOP.md)**: the team rulebook. How we use Git and GitHub, step by step, plus what to do when something goes wrong.
2. **[docs/PRACTICE.md](docs/PRACTICE.md)**: the Meeting 1 hands-on practice.

## The rules in one minute

- Nobody changes `main` directly. Every change goes: **Issue → branch → commits → Pull Request → review → merge.**
- Every PR needs **1 approval** and **green checks** before it can be merged.
- Always start a new branch from an up-to-date `main` (`git switch main && git pull`).
- Never `git push --force`. Never commit secrets.
- When unsure, stop and ask.

## What's in this repo

| Path | What it is |
|---|---|
| `members/` | One profile file per team member (`members/<firstname>.md`) |
| `team-board.md` | Shared file for the merge-conflict exercise |
| `docs/` | SOP and practice instructions |
| `.github/` | PR template and CI checks |
