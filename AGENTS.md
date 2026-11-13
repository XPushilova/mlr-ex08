# Project rules for Codex — MLR Exercise 8

1. Before every edit, read `SKILL.md` and the relevant cells in `ex08_student.ipynb`. Follow the notebook's Task 4 requirements.
2. Use simple, short, readable Python and PyTorch. Keep comments and Markdown brief. Explain only non-obvious logic or tensor shapes. Omit decorative text.
3. Start from the existing Task 3 pipeline. Preserve Tasks 1–3, their results, and the existing notebook structure. Make only the small changes each Task 4 experiment needs. Avoid unnecessary rewrites, refactoring, frameworks, abstractions, classes, helpers, or files.
4. Leave the Task 3 exercise to the student and keep the Task 4 training loop easy to follow.
5. Use a fixed baseline, identical train and validation splits, and fixed seeds. Compare one change at a time against the baseline before trying combinations. Repeat important runs with two or three seeds. Report only measured results.
6. Keep group and historical validation images out of training and tuning. Evaluate every variant on the same clean and camera-like validation sets. Generate the camera-like set once with a fixed seed and reuse it unchanged. Evaluate historical validation separately when used.
7. Keep the full group dataset and ZIP files out of Git. The assignment-required `check_image.jpg` is the only group image eligible to be committed, and only after checking that it contains nothing sensitive.
8. Leave `REVIEW.md` and `REPORT.md` for the student to write and edit. You may provide factual experiment tables or logs for the student to interpret.
9. Require an explicit user request before changing the Task 2 phone app or its ONNX assets, using Git or GitHub, deploying, or adding collaborators. Preserve the working Task 2 submission. Report only checks actually run and deployment success only when verified. Disclose tool use and credit authorship accurately.
10. At the end of each step, briefly list the changed files, the checks actually run, and any blockers. Stop after the requested step.
