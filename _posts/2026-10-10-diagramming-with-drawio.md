---
title: Choosing draw.io for my GCP landing zone diagrams
tags: [gcp, documentation, tools]
---

My GCP demo now has enough moving parts (bootstrap, folders, org policy, network host projects, and soon the VPCs) that I need pictures, not just Terraform and READMEs. Before drawing anything I asked Claude which tool to use, and settled on [draw.io](https://www.drawio.com/) (also known as diagrams.net).

## Why draw.io

- **It's free.** No license, no account required, and it's open source.
- **The official Google Cloud icons are built in.** Diagrams look like the ones in Google's own architecture docs without hunting down an icon pack.
- **It runs wherever I am.** In the browser at [app.diagrams.net](https://app.diagrams.net), as a Windows desktop app, or inside VS Code through an extension, all editing the same files.
- **Diagrams can live in the repo.** A file saved as `.drawio.svg` (or `.drawio.png`) is a normal image that GitHub displays in a README, but it still carries the diagram inside it, so it opens back up for editing. No separate "source" file to keep in sync.

## What I passed on

- **[Mermaid](https://mermaid.js.org/):** You write the diagram as text inside Markdown and GitHub renders it. Great for a simple flow like "which repo runs first," and it diffs nicely in git. But there are no GCP icons and you don't control layout, so it falls short for architecture pictures.
- **[Excalidraw](https://excalidraw.com/):** A hand-drawn, whiteboard look. Nice for quick sketches, but not the polished, icon-accurate style I want for a landing zone.

## Several small diagrams, not one big one

Rather than one poster that tries to show everything, I'm using a set of diagrams that each answer one question. They live as four pages in a single file, `gcp-landing-zone.drawio`:

1. **Resource hierarchy:** organization, folders, and projects, with where org policies and the `shared-vpc-host` tag attach.
2. **Identity and access:** groups, the Terraform service accounts they can impersonate, and what each account may change.
3. **Network:** per environment, the host project, Shared VPC, subnets, CIDR ranges and region.
4. **Deployment pipeline:** the repos in run order, with the state bucket and service account each one uses.

Claude generated the starting file from my repos; I've opened it in draw.io and am refining from there.

![GCP landing zone resource hierarchy]({{ '/assets/images/posts/2026/gcp-landing-zone-resource-hierarchy.png' | relative_url }})

## Installing draw.io on Windows

The desktop app is published by JGraph on GitHub ([jgraph/drawio-desktop releases](https://github.com/jgraph/drawio-desktop/releases)); version 32.4.1 is current as I write this.

### Option 1: winget (my choice)

From PowerShell or Windows Terminal:

```
winget install --id JGraph.Draw -e
```

That installs for all users (you'll get an admin prompt). If you'd rather install for just your own account, which needs no admin rights:

```
winget install --id JGraph.Draw -e --scope user
```

Later, keep it current with:

```
winget upgrade --id JGraph.Draw
```

### Option 2: download the installer

From the [releases page](https://github.com/jgraph/drawio-desktop/releases), download `draw.io-<version>-windows-installer.exe` (or the `.msi`, or the `arm64` installer on an ARM laptop) and run it. These are the same files winget uses.

### Option 3: VS Code extension

Since I live in VS Code, I also added the **Draw.io Integration** extension by Henning Dieterichs. Search for it in the Extensions panel, or:

```
code --install-extension hediet.vscode-drawio
```

After that, opening a `.drawio`, `.drawio.svg` or `.drawio.png` file in VS Code opens the editor right in a tab.

### Turning on the GCP icons

The Google Cloud shapes ship with draw.io but aren't shown by default. In the left panel click **+ More Shapes**, tick **GCP** (in the Networking section), and click **Apply**. A search like "cloud nat" or "vpc" in the shape search box also finds them.

### Saving so GitHub shows it

Use **File > Save As** and pick the editable SVG (or PNG) format so the file ends in `.drawio.svg`. Commit that file and reference it from a README like any other image.

## Next

I'll keep the diagrams in step with the Terraform repos as I build out the VPCs and, later, a VPC Service Controls perimeter around them.
