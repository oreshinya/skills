---
name: ore-doc
description: Writes out a settled plan as a plain-text plan document. Trigger when, after a plan or direction has been decided, the user asks for it to be written up, e.g. "put this into a plan" or "turn this into plan text".
---

Write the settled plan as text that an AI in a separate session, with no conversation context, can execute as-is, and that the user can read and verify without friction. Unless told otherwise, write it to `plan-<slug>.txt` directly under the current directory, and report the path. Write the plan in the language the user is using in the conversation, including the section labels (translate `Goal`, `Scope`, `Prerequisites`, and `Done criteria` accordingly).

Write it as task-procedure notes, not as prose. Express structure with indentation only (2 spaces). Do not use bullet markers, Markdown, or code fences. Write headings and major items as noun phrases, steps as imperatives, and checks as statements that should hold (e.g. "X exists", "Y matches Z"). Keep technical terms in their original form, and write each command and path on a single line as-is. Keep lines short, and leave out anything self-evident. Do not write secrets.

Put `Goal`, `Scope`, `Prerequisites`, and `Done criteria` at the top. Steps are executed from top to bottom. Only blocks that may run in parallel go under `The following may run in parallel`.

A line containing only `>` is a progress bookmark; everything above it is already done. When updating, do not delete items: append in the same format and move `>` to just below the finished items. Do not move it past a parallel block until the whole block is finished. For a check that did not pass, write the reason in a child line.

Sample:

```
Generate mp4 for video lessons and store a checksum

Goal
  On upload, create an mp4 in addition to hls, so that integrity can be verified with a checksum

Scope
  example_app

Prerequisites
  Split mp4 generation and checksum into separate steps and separate PRs
  This system is pre-release, so backward compatibility is not a concern

Done criteria
  <uuid>/video.mp4 exists in the bucket after upload
  videos.checksum matches the md5 of the mp4

step1 mp4 generation
  Add mp4_status to videos
    add_column :videos, :mp4_status, :string, null: false, default: "none"
  app/jobs/video_transcode_job.rb
    Generate the mp4 with ffmpeg after hls generation
    Upload it to <uuid>/video.mp4 in the bucket
  lint
  Write tests
    mp4_status becomes failed when mp4 generation fails
  Verify behavior
    video.mp4 exists in the bucket after upload
    Uploading to the same video from another tab returns 409
  PR

>
step2 checksum
  Add checksum to videos
    add_column :videos, :checksum, :string, limit: 32
  Compute the md5 after mp4 generation and store it
  The following may run in parallel
    lint
    Write tests
      checksum matches the md5
  Verify behavior
    curl -O https://staging.example.com/<uuid>/video.mp4
    md5 -q video.mp4
      Matches the value stored in the db
  PR
```
