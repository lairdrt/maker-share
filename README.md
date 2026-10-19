# maker-share

## 1. Overview

maker-share is a shared workspace for designing, developing, sharing, and printing 3D maker projects. Start with a measured problem, make a focused change, and record what actually worked on the printer.

**ChatGPT or Claude is your design agent:** discuss what to build and review results there. **Codex or Claude Code is your coding agent:** it changes project files in Visual Studio Code and handles Git/GitHub operations when instructed. OpenSCAD holds the 3D model; Bambu Studio prepares prints for the Bambu Lab P1S.

**Already comfortable with ChatGPT/Claude, Codex/Claude Code, VS Code, and agent-driven Git? Jump to the [TL;DR: OpenSCAD to P1S Fast Path](#tldr-openscad-to-p1s-fast-path).**

You are the bridge between the two agents:

1. Discuss your idea with ChatGPT or Claude. Supply measurements, photos, constraints, and desired behavior, then work through the design together.
2. When ready, ask the design agent to create a prompt for Codex or Claude Code. You generally do not need to invent implementation prompts yourself.
3. **Copy the entire prompt and paste it into the coding agent** running in the project repo. Let it make the requested changes, verify what it can, and return a concise report.
4. **Copy the coding agent's report and paste it back into the design-agent conversation.** Review the result together and decide the next step.
5. Repeat with the next prompt as needed, then render, slice, print, and physically test. Bring measured print results back to the same design conversation.

```mermaid
flowchart TD
    A[Idea] --> B[ChatGPT / Claude design discussion]
    B --> C[Requirements and measurements]
    C --> D[Design agent creates coding-agent prompt]
    D -->|COPY PROMPT / PASTE INTO| E[Codex / Claude Code in VS Code]
    E --> F[Coding agent edits project and returns report]
    F -->|COPY RESULT / PASTE BACK INTO| G[ChatGPT / Claude review and next decision]
    G -->|Next prompt| D
    G -->|Ready| H[OpenSCAD F5 preview / F6 render / STL export]
    H --> I[Bambu Studio slice / Preview]
    I --> J[Print on Bambu Lab P1S]
    J --> K[Physical test]
    K -->|Measured issues / revise| B
    K -->|Verified| L[Instruct coding agent to commit / tag / share]
```

For your first prototype, follow setup in Sections 3 and 5, gather inputs in Section 4, and use Section 6 at the printer.

## TL;DR: OpenSCAD to P1S Fast Path

For experienced AI/coding collaborators who already use VS Code and agent-driven Git/GitHub, but are new to OpenSCAD, Bambu Studio, and the P1S. Install OpenSCAD and Bambu Studio using the [tools table](#3-tools-and-installation) if needed, then follow this path from design conversation to first prototype.

1. **Design the part with ChatGPT or Claude.** Discuss the physical problem and provide measured dimensions, photos, constraints, and desired behavior. Resolve important fit/clearance questions, then ask the design agent for a focused implementation prompt. Use real measurements for fitted objects rather than guessing from photos.

2. **Send the prompt to Codex or Claude Code.** Start it from the repository/project root; at the beginning of a new session, have it read [AGENT.md](AGENT.md). Paste the design-agent prompt and have it create or modify the project's OpenSCAD `.scad` source while preserving unrelated geometry. Ask it to verify what it can and return a concise report; copy that report back to the design agent for review and the next decision.

3. **Review the OpenSCAD model.** Open the `.scad` file in OpenSCAD and leave it open while the coding agent works so you can review saved changes. Refresh if changes are not shown automatically. **F5 = fast preview**, which may show visual artifacts; **F6 = final, authoritative render before export**. Visually check important features and compare the relevant dimensions with your measurements.

4. **Export STL after a successful F6 render.** Choose **File → Export → Export as STL** and save into the project's `exports/` folder. If explicitly asked and able to do so, the coding agent may generate and verify this export instead. **SCAD is editable design source; STL is the geometry file for the slicer.** Resolve render errors before exporting.

5. **Open Bambu Studio and import the verified STL.** Select **Bambu Lab P1S**, confirm the installed nozzle and filament/profile, and orient the part for strength and minimum support. For simple PLA/PLA+ prototypes, start with a **0.4 mm nozzle, 0.20 mm layer height, 3 wall loops, 15–20% sparse infill, and supports off unless needed**. These are starting points, not universal settings; the selected profile must match the actual nozzle and filament.

6. **Slice and inspect.** Click **Slice plate**, then inspect **Preview**. Check first layers, holes, thin walls, bridges, overhangs, supports, and important fitted features. Review the sliced layers before sending the job; the solid model view alone does not show the actual print paths.

7. **Send to the printer.** Send the reviewed job to the **Bambu Lab P1S**, confirm the destination printer and material mapping as needed, and print. Check that the intended filament is loaded rather than relying only on the project filename or profile name.

8. **Test the real part.** Check fit, clearances, mounting, flex, warping, access/insertion/removal, and print quality. Bring measured problems and photos back to the design-agent conversation, ask for the next coding-agent prompt, and repeat until verified. A successful CAD render does not establish physical fit or strength.

9. **Save the proven print setup.** Once a print is proven, save a Bambu Studio project such as `model-p1s-print.3mf`: unlike the STL geometry export, this project includes orientation and slicer settings. For repeat prints, reopen the known-good 3MF → confirm printer/nozzle/filament → slice → Preview → print. If the SCAD source changes, update matching exports and recheck the setup before treating that project as current.

If this process is clear, use this section as your normal fast path. For step-by-step help, see [Section 5: VS Code and coding-agent workflow](#5-vs-code-and-coding-agent-workflow), [Section 6: OpenSCAD to Bambu Studio to print](#6-openscad-to-bambu-studio-to-print), or [Section 7: Git and GitHub with the coding agent](#7-git-and-github-with-the-coding-agent).

## 2. Printer and filament

Our normal printer is the [Bambu Lab P1S](https://us.store.bambulab.com/products/p1s).

| Feature | Working context |
| --- | --- |
| Build volume | 256 x 256 x 256 mm |
| Motion and enclosure | Enclosed CoreXY |
| Standard nozzle | 0.4 mm |
| Optional nozzles | 0.2 / 0.6 / 0.8 mm |
| Maximum hotend temperature | 300 °C |
| Common supported materials | PLA, PETG, TPU, ABS, ASA |
| Slicer | Works directly with Bambu Studio |

Use the 0.4 mm nozzle for general work. A 0.2 mm nozzle helps with fine lettering and small details; other nozzle and material choices depend on the project. Confirm the installed nozzle before choosing a slicer profile.

Start with [Bambu Lab PLA Basic](https://us.store.bambulab.com/products/pla-basic-filament), a 1.75 mm, easy general-purpose filament suited to prototypes. Other quality PLA or PLA+ brands are fine with suitable filament profiles. Record the actual material used when judging fit or strength so another collaborator can reproduce the result.

## 3. Tools and installation

| Tool | Purpose | Windows link |
| --- | --- | --- |
| ChatGPT | Discuss requirements and design | [Open ChatGPT](https://chatgpt.com/) |
| Claude | Discuss requirements and design | [Open Claude](https://claude.ai/) |
| Visual Studio Code | Edit source and run coding agents | [Download](https://code.visualstudio.com/) |
| Git for Windows | Version control and sharing | [Install](https://git-scm.com/download/win) |
| OpenSCAD | Preview, render, and export models | [Download](https://openscad.org/downloads.html) |
| Bambu Studio | Orient, slice, preview, and print | [Download](https://bambulab.com/en/download/studio) |
| Claude Code | Implement focused repo tasks | [Setup documentation](https://docs.anthropic.com/en/docs/claude-code/getting-started) |
| Codex | Implement focused repo tasks | [Official IDE documentation](https://developers.openai.com/codex/ide) |

Install Git, VS Code, OpenSCAD, and Bambu Studio using the links above. Section 5 walks through the extension, coding-agent, and repository setup.

OpenSCAD **2021.01** and Bambu Studio **02.08.02.61** have been used successfully. Newer stable versions are generally acceptable; no downgrade is required.

## 4. Design workflow

Start a conversation in ChatGPT or Claude to decide **what should be built** before files change. Describe the problem and possible shapes, then collect these inputs with the design agent:

| Input | Useful detail |
| --- | --- |
| Dimensions | Measured sizes, units, and which surfaces they describe |
| Photos or sketches | Views showing the fitted object and available space |
| Fit and clearance | Sliding, loose, snug, or removable; clearance per side or total |
| Mounting and hardware | Screw sizes, hole spacing, attachment method |
| Access | How the object enters, leaves, and is handled |
| Printing | Intended material, nozzle, and whether supports are acceptable |
| Use conditions | Indoor/outdoor, heat exposure, loads, and strength needs |
| Appearance | Visible faces, lettering, finish, and preferred orientation |

**Measure real fitted objects whenever possible.** A nominal product size or a photo without scale is a poor substitute for the dimension that controls fit. Mark unknowns explicitly so the agent can ask about them.

Agree on a testable outcome: for example, “the holder accepts the measured device and lets it slide out without removing the mounting screws.” If fit is uncertain, ask about a small fit sample before the full part. Once the constraints are settled, ask the design agent for the first coding-agent prompt.

## 5. VS Code and coding-agent workflow

![Visual Studio Code with OpenSCAD source, project files, and the Claude Code panel](docs/images/VSCode.png)

*Reference: VS Code with the cradle source open and a coding-agent panel alongside it.*

### First time

1. Install VS Code and Git for Windows using Section 3.
2. Open VS Code, click the **Extensions** icon on the left sidebar (or press **Ctrl+Shift+X**), search for **OpenSCAD Language Support** by **Leathong**, and click **Install**. This is our known working OpenSCAD extension.
3. Install/configure Codex and/or Claude Code using its linked setup instructions, including sign-in.
4. Ask the coding agent to clone the maker-share repository into your chosen local project folder; provide the repository URL from GitHub. If you already have a local copy, use that copy.
5. Choose **File → Open Folder** and open the **maker-share repository root**, not a broad parent directory. Run the coding agent from that root.

### Each work session

1. Start in the ChatGPT or Claude design conversation. Work out the model or change needed, then ask for a Codex/Claude Code prompt.
2. Copy the entire prompt and open the coding agent in VS Code with maker-share open.
3. At the start of a new coding-agent session, tell it: **“Read AGENT.md, then execute the following task.”** Paste the design-agent prompt.
4. Ask the coding agent to check Git status and pull the latest changes before editing (Section 7). Have it report existing changes or conflicts before proceeding.
5. Let it inspect only relevant files and implement the requested change. Keep OpenSCAD open to review saved model changes as it works.
6. Let it perform requested verification and review the relevant Git diff. Read its final report, including checks it could not perform.
7. Copy that report and paste it back into ChatGPT or Claude. Include your observations or screenshots if the model looks wrong.
8. Discuss the result with the design agent. Copy/paste its next prompt if another change is needed, or proceed to rendering and printing in Section 6.

[AGENT.md](AGENT.md) defines implementation rules. The collaborator carries prompts and reports between the agents; the coding agent does not automatically share its results with the design conversation. Resolve new design decisions with the design agent before expanding implementation scope.

Ask the design agent to include the goal, target file, dimensions, what must stay unchanged, verification, export requirements, and Git authorization in each prompt. A short example of what it might produce:

```text
Read AGENT.md, then update projects/device-holder/src/model.scad.
Goal: widen the internal pocket to 42.6 mm for a measured 42.0 mm device
(0.3 mm clearance per side). Keep exterior width, base thickness,
mounting holes, and all other dimensions unchanged. Report any conflict.
Verify the pocket dimension and F6 render. Do not regenerate exports.
Do not commit or push. Report files changed and checks performed.
```

Review fixed features too: a plausible-looking model can still have an incorrect clearance or shifted mounting hole.

## 6. OpenSCAD to Bambu Studio to print

### Edit, preview, render, export

![OpenSCAD showing the cradle model, source editor, and preview console](docs/images/OpenSCAD.png)

*Reference: OpenSCAD preview of the cradle model. Use F6 for the final render before export.*

1. Let the coding agent create or modify the project's `.scad` file, normally the design source of truth.
2. Open that file in OpenSCAD and keep it open while the agent works. Refresh saved changes if automatic reload is disabled.
3. Use **F5** for fast preview while developing. It may show visual artifacts.
4. Use **F6** for the final, authoritative OpenSCAD render before export. Resolve render errors before proceeding.
5. Visually inspect important geometry, especially fitted surfaces, holes, and features changed by the task. Compare dimensions with the agreed measurements.
6. Once verified, use **File → Export → Export as STL** and save into the project's `exports/` folder. If able and instructed, the coding agent may generate and verify the export instead.

Prefer named parameters, explain non-obvious values, and derive related dimensions where practical. Preserve known-good geometry during narrow changes; a successful render alone does not prove dimensions or fit.

Use a clear export filename. Geometry-only 3MF exports can also carry a mesh, but they do not automatically contain a prepared Bambu Studio print setup.

### Prepare the print in Bambu Studio

![Bambu Studio Prepare view with the cradle on the build plate and printer, filament, and process settings at left](docs/images/BambuStudio.png)

*Reference: Bambu Studio's Prepare view. Confirm your own printer, nozzle, filament, and process settings before slicing.*

1. Open Bambu Studio.
2. Import the verified STL, or open an existing known-good `*-p1s-print.3mf`.
3. Confirm printer = **Bambu Lab P1S**.
4. Confirm the profile matches the installed nozzle.
5. Confirm the loaded filament and selected filament profile match.
6. Confirm model orientation for strength and minimum support.
7. Confirm starting print settings using the table below.
8. Click **Slice plate**.
9. Open **Preview** and inspect layers, holes, bridges, overhangs, thin features, and supports.
10. Send the reviewed print to the printer and print it.
11. Inspect the real printed part using the physical checks below.
12. Report measured fit and issues back to the design-agent conversation.

For simple PLA prototypes, use these initial settings:

| Setting | Starting point |
| --- | --- |
| Printer / nozzle | Bambu Lab P1S / 0.4 mm |
| Filament | PLA / PLA+ with a suitable profile |
| Layer height | 0.20 mm |
| Wall loops | 3 |
| Sparse infill | 15–20% |
| Supports | Off unless needed |
| Orientation | Favor strength and minimum support |

**These are starting points, not universal settings.** Adjust for the part's function and inspect Preview again, especially fitted surfaces and mounting features.

### Save a repeatable print project

**SCAD** is editable design source; **STL** is geometry sent to the slicer; a **Bambu project 3MF** is a saved ready-to-print project with orientation and slicer setup. After a print is proven, save a project such as `model-p1s-print.3mf` alongside its matching export.

For repeat prints: open the known-good `*-p1s-print.3mf` → confirm printer/nozzle/filament → slice → Preview → print. If the source changes, update the geometry and recheck the setup before treating the saved project as current.

### Physically verify

A model is not fully proven because it looks correct in CAD. Test the printed part for:

- Fit, clearances, hole sizes, and mounting alignment.
- Flex, warping, and suitability for the intended load.
- Insertion, removal, and access to hardware.
- Bridges, overhangs, and surface quality.

Tell the design agent the material, relevant print settings, result, and useful photos. Describe failures with measurements, such as “pocket is 0.4 mm too narrow,” so it can create the next coding-agent prompt. Distinguish “rendered,” “sliced,” and “physically tested” when sharing progress.

## 7. Git and GitHub with the coding agent

Git keeps local version history; GitHub holds the shared remote copy. A commit records a version locally, and a push shares it. Install [Git for Windows](https://git-scm.com/download/win), then normally **tell Codex or Claude Code to perform Git operations**. Routine command-line Git work is not required.

Use **Pull → Work → Verify → Commit → Push**. The coding agent can check status, pull, inspect diffs, stage relevant files, commit, push, and verify the result. Give instructions such as:

| When | Tell the coding agent |
| --- | --- |
| Check the workspace | “Check Git status and tell me whether the repo is clean.” |
| Before editing | “Pull the latest changes from origin/main before we start.” |
| Share verified work | “Review the changes, commit only the files from this task with a descriptive message, and push to origin/main.” |
| Confirm completion | “Show me the current Git status and whether local main matches origin/main.” |

Have the agent report existing changes or conflicts before pulling or editing, preserve unrelated work, and review the relevant diff before committing. It must wait for explicit instructions to commit or push; the design agent can include those instructions in a prompt once you decide the work is ready to share.

Optional command reference—these are the operations the agent handles, not a script you need to run:

```text
git pull    # Bring in shared changes
git status  # Show changed files
git diff    # Inspect edits
git add     # Stage specified task files
git commit  # Record a version
git push    # Share committed work
```

Important known-good designs may receive tags such as `v1.0`, `v2.0`, or `v3.0` to mark meaningful baselines. Ask the coding agent to create/share a tag when appropriate and identify which project/version it represents. Do not casually move an existing tag because documentation or photos change later.

## 8. Project structure and collaboration rules

Use this general structure as projects are added; it is a recommendation, not a list of folders already created:

```text
maker-share/
  README.md
  AGENT.md
  projects/
    project-name/
      src/
        model.scad
      exports/
        model.stl
        model-p1s-print.3mf
      photos/
      reference/
      README.md
  docs/
  reference/
```

`.scad` is editable source; `.stl` is exported geometry; `*-p1s-print.3mf` denotes a known-good Bambu print project. Keep project-specific measurements and references with the project. Use root `docs/` and `reference/` for shared material.

Project documentation may capture useful measurements and results; ask the design agent to include it in a prompt when useful. Routine README maintenance is not expected. Flag stale exports or unprinted designs in reports.

- Ask the coding agent to pull before starting and work on one focused task at a time.
- Coordinate before modifying another collaborator's project.
- Preserve known-good versions and existing tags.
- Keep source and exports synchronized when publishing a printable revision; identify intentionally deferred exports.
- Have the coding agent review Git status/diff, use clear commit messages, and push completed work when authorized.
- Include measurements/photos for fitted-part requests and physically verify important designs.

Measure real objects. Make dimensions explicit. Use 3D models. Change one thing at a time. Render before exporting. Slice before printing. Physically test before declaring success. Keep known-good versions in Git.
