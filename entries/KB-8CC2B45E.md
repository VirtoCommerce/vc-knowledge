---
id: KB-8CC2B45E
subject: A thumbnail task whose run aborted (invalid backgroundColor) keeps lastRun null, so the Admin run dialog keeps "Process changes" disabled for it
plane: experiential
question: Is lastRun set on a thumbnail task when its run fails, and what does the run dialog then offer?
status: active
appliesTo:
  - axis: module
    value: virtocommerce.imagetools
  - axis: surface
    value: admin-spa
anchors:
  - coordinate: POST /api/image/thumbnails/tasks/run
  - coordinate: POST /api/image/thumbnails/options
evidence:
  - method: observation
    deployment: vcptcore_qa1
    at: 2026-10-12T13:58:11.023Z
    by: session:49cbdcfd
    who: kutasinaelena
---
Saving a thumbnail option with backgroundColor "notacolor" returns 200 (no validation). A Regenerate run of a task using it ends with errorCount 1, the notification error is a raw .NET stack trace (ImageColor.Parse with CI build paths), and the run stops after the first file (later files in the folder get no thumbnails at all). The task's lastRun stays null afterwards, and in Admin > Thumbnails > Run the "Process changes" button is disabled for it exactly as for a never-run task; it is enabled for a task with a successful run. Observed on ImageTools 3.1007.0-pr-130.
