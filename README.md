# rush-tsconfig-example

Example project to demonstrate issue with `tsconfig.json` for [WEB-30677](https://youtrack.jetbrains.com/issue/)

## Usage

1. `npm install -g @microsoft/rush`
3. `rush install` (inside repo dir)

## The issue

When the `tsconfig.json` is not in the project root, WebStorm/PhpStorm does not find/use the `tsconfig.json` on a per project basis for Typescript source located in that project (eg: `project/tsconfig.json`)
 
In this repo, the IDE Typescript service reports and error that `foo.ts` is not included in any `tsconfig.json`, when it is included via/in `project/tsconfig.json`

Discussion on the YouTrack issue suggests other people have had different errors with the IDE Typescript service when the `tsconfig.json` is not in the project root.

This is a problem especially in monorepos where you can want different `tsconfig.json` per project in order to have different options/configuration. The IDE project root in a monorepo is not a project itself unlike in traditional project repos.

### The workaround

The current workaround is to specify in the IDE Typescript options the specific `tsconfig.json` that applies for the project's source that is being edited ie: `--p project/tsconfig.json`.

This is a very annoying workaround as if projects in the monorepo have different configurations, a developer will have be changing this option as they change between projects to stop the IDE reporting errors.

## Proposed solution

When editing a source file in a directory the closest `tsconfig.json` to that file is the one used with the Typescript service.
