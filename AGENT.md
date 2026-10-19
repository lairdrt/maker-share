# Coding-agent instructions

## 1. Role and scope

- Implement narrowly scoped tasks for this maker-project repository. Design intent comes from the user's task or design-agent prompt.
- Do not invent unrelated redesigns, add speculative features, or change geometry to suit an unstated preference.
- Do not guess critical dimensions. Report missing measurements, conflicting constraints, or ambiguous clearance definitions before making dependent changes.
- Separate supplied measurements from assumptions. Use millimeters unless the task explicitly specifies another unit.
- Follow the requested preservation boundary. If satisfying the task requires changing a protected feature, explain the conflict and obtain direction.
- Consult README.md for the human workflow only when needed; keep implementation focused on the task.

## 2. Repo discipline

- Work from the repository root. Establish Git status before editing so existing user changes remain identifiable.
- Identify exact target files and inspect only the source, references, and instructions needed for the task. Avoid broad unrelated exploration.
- Do not modify unrelated files. Never silently discard, overwrite, reset, or revert user changes.
- Preserve repository history. Do not rewrite history or move/delete existing tags unless explicitly instructed.
- Do not rename or reorganize files, create extra documentation, or add tooling unless requested or necessary to the task.
- The project's `.scad` source is normally authoritative. STL and 3MF files are derived artifacts.
- Regenerate exports only when requested or clearly required by the task. If exports remain unchanged after a source edit, report that they were not regenerated.
- When a changed design is being prepared for publication, release, or a known-good printable baseline, matching exports should normally be regenerated and verified.
- A plain geometry 3MF and a Bambu Studio project 3MF are different artifacts. Preserve an existing print project's orientation and slicer settings unless the task calls for changes.
- Keep output paths explicit. Do not overwrite a known-good export with an unverified revision.

## 3. OpenSCAD and printer rules

- Prefer parametric geometry with meaningful named dimensions. Derive dependent dimensions where practical and explain non-obvious constants.
- Use real measurements supplied by the user. Distinguish object dimensions, pocket dimensions, and clearance per side versus total clearance.
- Preserve known-good geometry outside the requested change, including mounting locations, interfaces, and external dimensions named as fixed.
- Avoid fragile features, unintended thin walls, disconnected solids, and invalid geometry where possible. Consider the intended print orientation.
- Keep normal 0.4 mm nozzle printability in mind unless the task specifies another nozzle. Do not silently enlarge small features or alter their function.
- Do not unnecessarily encode slicer-specific settings into CAD geometry. Keep print setup in Bambu Studio unless the task explicitly requires modeled features.

Default context, unless overridden:

| Item | Default |
| --- | --- |
| Printer | Bambu Lab P1S |
| Build volume | 256 x 256 x 256 mm |
| Nozzle | 0.4 mm |
| Material | PLA / PLA+ |
| Layer height | 0.20 mm |
| Wall loops | 3 |
| Sparse infill | 15–20% |
| Supports | Off where practical |

These are starting assumptions for implementation and review, not universal print settings or evidence of physical success.

## 4. Task execution

1. Read the request and identify the exact outcome.
2. Identify target files, critical dimensions, and what must remain unchanged.
3. Check initial Git status and inspect only necessary source/reference files.
4. Resolve blocking ambiguity before changing dependent geometry.
5. Make the smallest change that satisfies the task. Avoid opportunistic refactors.
6. Preserve names, file layout, unrelated parameters, and existing conventions.
7. Render and verify the changed features using available tools.
8. Regenerate requested exports only after successful render verification.
9. Review relevant Git diff and status for unintended edits or artifacts.
10. Report results concisely, including any remaining verification gap.

Keep inspection and tool use proportional to the change. Prefer direct implementation over lengthy planning. Do enough verification to prevent regressions without unrelated searches or unnecessary repeated checks.

## 5. Verification

- F5 is preview only and can show visual artifacts. F6 is the authoritative OpenSCAD render; complete it before final export. A completed command-line render may provide equivalent render evidence when available.
- Verify dimensions and features affected by the task, plus adjacent geometry whose dimensions depend on them.
- Check fixed features against the original source/diff. A successful render alone does not establish dimensional correctness.
- Maintain valid printable geometry and check that the intended orientation fits the available build volume.
- When practical, inspect exported STL geometry rather than trusting source alone. Confirm that it corresponds to the changed source and contains the intended part.
- When changing a Bambu project, check printer, nozzle, material, orientation, and sliced Preview if Bambu Studio is available.
- Never claim a render, export check, slicer check, or physical test that was not performed. If tools are unavailable, state what source review established and which checks remain.
- CAD correctness does not prove fit, strength, or print quality. Physical acceptance requires a printed part and observed results from the collaborator.

## 6. Git and reporting

- Do not commit or push unless explicitly asked. Authorization to edit or regenerate exports does not imply Git publication.
- Before a requested commit, inspect relevant status and diff, including staged changes. Stage only task-related files and use a descriptive commit message.
- Preserve existing version tags unless explicitly instructed otherwise. Documentation/photo updates do not justify moving a design tag.
- If asked to push, verify afterward that the local branch matches its remote branch. Report any failure or mismatch accurately.
- Leave unrelated user work intact and distinguish it from your changes in the final status report.

Return only information useful to the collaborator:

- **Files changed:** exact task-related paths.
- **Geometry/behavior:** what changed and any relevant preserved constraints.
- **Verification:** checks performed, results, and unrun checks.
- **Unresolved issues:** blocking ambiguity or remaining physical checks, if any.
- **Exports:** whether regenerated, which files, or intentionally deferred.
- **Git status:** remaining changes and whether a requested commit/push occurred.

Keep the report brief. Do not describe an unprinted revision as physically proven.
