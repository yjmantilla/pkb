---
title: README
---

# README

This is a base repository to make my flavor of an hypertext graph knowledge base system using Jekyll. The main feature is custom management of references and custom graph generation usable on the browser. All of this was inspired by [foam](https://github.com/foambubble/foam).

Use gen_static_data.py to generate the static data for the site. This script will generate the data for the nodes and the graphs.
You need Jekyll to run the site.

You can create references to other notes using the ``(())`` where the parentheses are actually square brackets. The text inside the brackets is the filename of the note. If the note doesnt exist, it will be redirected to the stub.md file.

Each new directory containing notes is a new "category", and should have a directory-name.md file in the dirs folder. The collect-stuff function in gen_static_data.py will generate the list of notes, so you can use it to generate the list of categories in that script.

Assumptions:

- each .md has a unique filename (even beyond different directories, root/nodes/a.md and root/a.md cannot happen).
- stub.md is the default file for a non-existing note.
- No notes are beyond the first level of sub-directories. root/subdir/note.md is the deepest you can go.
- Each note has its front matter with the title at least.
- No double square brackets in the text unless it is a reference.
- Each note has a unique title (not sure what happens if not...)
- Using the [paste image vscode extension](https://github.com/mushanshitiancai/vscode-paste-image) is recommended but you may need to configure it (e.g. path of images copied to assets, etc).
- Currently, it is better to use this repo as template. Or you may download this as a zip and unzip to the root of your project. Then you can delete the .git folder and use it in your own repository. Forking is not recommended as GitHub does not allow multiple forks of the same repository into the same account through their gui (rather you need to do it manually which is cumbersome).
- Might be worthwhile experimenting with dendron-like hierarchy (root.subdir.note.md). For example, people.john.md . Though the functionality might overlap with the category system and/or the tag system. Might be a way to impose hierarchy inside the nodes folder.

Preview: https://yjmantilla.github.io/jekyll-hypertext-network/
