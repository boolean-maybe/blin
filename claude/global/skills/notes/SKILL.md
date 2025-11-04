---
name: blin notes
description: view, create, update, delete blin notes
allowed-tools: Read, Grep, Glob, Update
---

# blin notes
 
A blin note is a Markdown file in blin format saved in the project `.doc/notes` directory
with a name like `uno-1234.md`
If this directory does not exist prompt user for creation

## blin note number

Every blin note has a number in format:
`UNO-1234`
where 
- `UNO` is the project name shortcut in all capital letters
- `1234` is the next unassigned number

## blin system file

The blin system file is a special file located at `.doc/system.yaml`
It's a yaml formatted config file that contains blin notes configuration for example:

```yaml
shortName: UNO
nextNumber: 1234
```

if this file does not exist prompt user for short project name and set `nextNumber` to 1

## blin note format

A blin note format is Markdown with some requirements:

### frontmatter

```markdown
---
id: UNO-1234
title: My Note
type: feature
tags:
  - markdown
  - frontmatter
  - metadata
status: open
---
```

where fields can have these values:
- type: bug, feature, task, story, epic
- status: open, in progress, resolved, closed
- resolution: fixed, won't fix, duplicate
- priority: critical, high, medium, low

### body

The body of a blin note is normal Markdown

if a note needs an attachment it is implemented as a normal markdown link to file syntax for example:
- Logs are attached [logs](mylogs.log)
- Here is the ![screenshot](screenshot.jpg "check out this box")
- Check out docs: <https://www.markdownguide.org>
- Contact: <user@example.com>

# Describe

When asked a question about a blin note find its file and read it then answer the question 
If the question is who created this blin note or who updated it last - use the git user name
For example:
- who created this blin note? use `git log --follow --diff-filter=A -- <file_path>` to see who created it
- who edited this blin note? use `    git blame <file_path>` to see who edited the file

# View

created is taken from git file creation
author is taken from git creator

# Creation

When asked to create a blin note:
- Check out [blin system file](#blin-system-file) for the next unassigned number to use and increase this number 
by one in system file
- create its file in [blin note format](#blin-note-format) with a name in format `projectShortName-currentUnassignedNumber`
- If status is not specified use `open`
- If type is not specified - prompt the user or use `task` by default

# Update

When asked to update a note - edit its file
For example when user says "set UNO-1234 in progress" find its file and edit its frontmatter line from
`status: open` to `status: in progress`

# Deletion

When asked to delete a blin note - delete its file