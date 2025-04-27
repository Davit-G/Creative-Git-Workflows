# Creative-Git-Workflows
This repository outlines some thoughts I've been having about using Git for creative projects.


# Background
## Creative motivations

For context, I've been producing electronic music as a hobby for a very long time. Often I've brought music or my general creative interests into programming as well, creating things such as 3d browser visualisations or using ray marching techniques and ANSI escape codes to draw 3d scenes in the terminal using ASCII characters.

When I work on creative programming projects, I have lots of moments where I make interesting mistakes or changes to my code which I really want to revisit or utilise later, especially if the programming project creates artifacts such as images / visuals, or audio, I often find that I want to use these things and be able to reference them later down the line.

In my opinion, creative work can be interpreted as state exploration. Often it's very freeform, often initially driven by inspiration or other random processes especially in the beginning, and over time the idea gets refined and worked on more until polished final ideas are produced (last step is optional 😭). Jonas Tyroller uses the analogy of a 2d ocean exploration [in their video about game dev ideation](https://youtu.be/o5K0uqhxgsE?si=TWy7WMzEj6kFkXg7).

Why this feels like the case to me personally (speaking to my anecdotal experience with music) could be driven by sunk cost fallacy on projects and a fear of letting go of existing ideas, or maybe it's simply not novel or interesting enough to create ideas that are so loose and unrealised. For creative projects driven by financial incentives it would only make logical sense that this emergent behaviour pops up, since being stuck on ideation very early on can take time away from pushing a finalised piece of work out, which can lead to losing money. It's unfortunate since this process is where most of the value of creative work lies.

I also like the idea of preserving creative work, and I also like the idea of deferring judgement on it as late as possible in the hopes I stumble on something great, but realistically I'm constantly finetuning ideas throughout the process. Regardless, I think it's still a good idea not to judge ideas too early otherwise they will never be brought to execution.

## Existing uses of git

Often, coding projects are versioned using Git. Standard conventional git workflows in companies (create your feature branch off production, work on changes, make a PR, get review etc) are designed for massive collaboration on one project / product / platform. Often, companies have complicated CI setups alongside this git workflow because they can (most of the time) rely on the main branch being safe and non-breaking. It's important to note these workflows focus on perfecting a SINGLE artifact / product / platform / end result, which is why there is always a "main" branch.

For personal projects with little to no contributors (and no plan of future contributions), people often don't branch their code off as it only hinders in the way of writing code and pushing the project forward. Instead, versioning for the sake of a code backup system seems to be the use case, but the main branch gets used in this same way.

## Naive proposal for creative coding + git, and issues

In order to facilitate state space exploration in a creative coding project, you'd want to be able to synthesize a bunch of ideas at once from a source, create variations of existing ideas, quickly view their differences and then use all of these together in some way to push a finalised creative project. Often, people create duplicate projects / files and version them with different names, which is messy to organise and loses it's usefulness quickly.

Essentially, instead of creating 10 different versions of the same files, can we defer this process to git so our workspace is consistent? Git is often used for the single-product collaborative workflow but flexible enough to be used in any way deemed fit, so theoretically people can just make branches off of their existing work whenever they find interesting variations and indefinitely repeat this process. 

Hypothetically, let's imagine a project versioned with git designed to generate abstract art images using shaders.

Creating different varations of the project with branches results in quite a cluttered tree of changes:
- It's confusing to compare different branches in the project just by branch name.
- Only the person who created the branches would have context on what each branch's image looked like
- Branches don't inherently give info on the output / visuals generated from the code.
- There is no way to see what the changes on the branch looks like without checking it out (checkout) or using CI pipelines to automate artifact generation (in a similar way to frontend visual regression tests).
- Working on this project with other people can become very difficult over time without an easy way to view branches.

We can rethink how we use git to help fester and preserve original ideas. Part of the purpose of writing this README is for me to think through how git might be used in this way, and hopefully it might be a nice inspirational point for somebody to start their own work and model their creative process in this way. I can't guarantee it's usefulness

# Goals

In a creative workflow we want to be able to:
- Synthesize as many ideas as possible.
- Treat different ideas as their own artifacts / end results.
- Quickly compare different ideas and pick ideas which can be explored further.
- Keep beginning ideas in case they can be reused in the future.
- Potentially make it easy to show versions and changes of work to stakeholders.

A little bit more contentious but:
- Make it easy to combine all the ideation together if one wants to.

Since we're still creating code, we need boilerplate that will be shared across the entire project, which can be committed on main like usual. When we get to the stage where we can diverge on ideas, a perspective shift is needed.

# Proposal

The proposal aims to align the idea of "creative state space exploration" to the tools that git offers us in order to facilitate creative workflows. This can be done by relaxing some constraints on standard workflows and enforcing others.

This proposal is for small scale projects with either one or a few collaborators.

Treat a branch as an isolated creative idea which has a unique output / artifact to the other branches, so instead of merging everything into `main` early on, branch off as wide as possible in a BFS pattern, with the heuristic being your stylistic preference for what you think is worth extending.

- Rethink `branches` as creative ideas
    - Whenever an idea starts differing significantly to the previous commit, branch off / split off your current work
    - Old branches should never be deleted during ideation phase
    - If you want to extend work on an old idea / branch, two things you could try:
        - Branch off of it AGAIN to preserve the old branch HEAD if you like the old work
        - OR keep adding commits as usual if you don't like it (not preferred, "dont judge ideas early")
- Make it easier to `view changes` to output 
    - Include builds / artifacts / images / creative output in one folder for later analysis
    - Keep output from previous branches in newer ones
    - Don't overwrite output, but rename them either via commit hash or some other way.
- Rethink `merging` workflow as "combining ideas"
    - If ideation has finished, merging could be used to combine all the good variations of work into one. Choose what you want to merge and what you want to leave.
    - Don't think of merging as "picking between ideas", but start with both and figure out what you'd like to keep from both to create something even better than the two ideas on their own.
    - Merging is optional, old branches can still be abandoned or cherry picked where necessary.
- `fork` ideas which have massive project level changes.
    - If an idea has largely surpassed the original concept, then it can be forked into a new repository to work on independently.

## Potential efforts
- This leaves room for potentially creating a new CLI tool which automatically updates READMEs on different branches to provide quick previews to assets.
- Previews could also be provided through an alternative git graph visualiser tool using force directed graphs and 2d rendering. This can be written as a VS Code extension which instantly displays all the different artifacts to the user, and allows instant checkouts to different variations.
- CI can be configured for automatic asset generation:
    - Pre commit hooks can be ran locally in cases where generating output can be automated (in the case of scripts which do image / video / audio creation)
    - External services like Github can use CI pipelines to generate artifacts, which might be analysed by other tools (but this might be harder to visualise, and leads to lock in with the specific vendor)

## Issues
This approach might not be the most scalable approach
- Large output files will lead to a bad developer experience when switching between branches and downloading / cloning asset files.
- Not all projects have output items that are easily previewable. For example, instead of images / videos / audio, it could be binaries for the demoscene space or something similar, which might be hard to automate collecting previews for in CI.
- The entire git repo needs to be tracked and stored locally if one wants to observe all the different variations and changes.