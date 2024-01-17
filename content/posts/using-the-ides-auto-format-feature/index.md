---
title: Saving time by using the IDE's linters and formatters
description: Modern IDEs can help keeping your code clean without the hassle to install, configure or maintain the linting tools yourself.
tags: [ "IDE", "WebStorm", "IntelliJ", "VS Code", "TypeScript", "Static Code Analysis", "Linter" ]
date: 2024-01-17
---

How should code be indented - with tabs or with spaces?
Should the indentation be two or four spaces wide?
Should the opening curly brace be on the same line as the function declaration or on the next line?
Should there be an empty line before each return statement?
Should "yoda" conditions be allowed or even enforced?
Should unused variables be deleted?

With each of those questions you can fill whole tech talks.
But does it make sense to discuss these questions in your team?
Does it make sense to define **custom** rules, just for your project?
Is it worth the time and effort?
Will it save you time or money in the future?
Will the code quality be better compared to just using the **standard** linting settings of the IDE? 
Let's explore the idea.

<!--more-->

## The IDE has you covered!

So, you are starting a new project and want to have consistent code styling from day one onwards? 
Or you have an existing project and want to improve the code quality, but because of the low test coverage don't want to change any internal logic?
You also want some tooling that shows you how to improve the code or even automatically apply those recommendations?
Then, what you want is a "static code analyzer".

A static code analyzer reviews your code at compile time (not at runtime) or directly while you are programming.
Those analyzers can come in the form of "formatters", "linters" or "type checkers".
A formatter checks the code for consistent "visual" styling.
It makes sure that the same indentation (tabs vs spaces), the same quotes (single vs double) or the same naming convention (camel case vs snake case) is used across the whole code base.
Linters go a bit deeper and check the code for hidden problems that might cause runtime issues in the future. 
They can locate unreachable code, loops that can only loop once, variables that could/should be made immutable, comparisons that are not type-safe, etc.
The linter might suggest some code restructuring (e.g. for yoda conditions), but the behavior of the application should not change because of that (assuming that piece of code was bug-free before).

For the popular programming languages there exist a bunch of static analyzers.
In the JavaScript world we have for example `prettier` for formatting and `eslint` for linting.
In general, those tools are very easy to install.
**But**, do you really want to invest the time to **configure** and **maintain** them yourself?

What settings would you choose for the rules?
Eslint has a few hundred rules, each with at least two options.
Either you let an authority decide (e.g. the CTO).
This is bad for team moral and expensive for the company (CTO time is precious).
Or you let the team decide.
Then, you will have many long discussions, because everyone has another opinion.
Finding a consensus will be very hard.

Also, you have to keep the formatter and linter up-to-date.
Over time, linting rules will change, new rules will be added and some will be deleted.
Have a look at the [`eslint` rules](https://eslint.org/docs/latest/rules).
Scroll down to the deprecated and removed rules.
This is a very long list.
Each entry means maintenance work for you - even if it is just to determine that this specific rule is not relevant for your case.

To avoid both problems you can just utilize your IDE.
Each modern IDE comes with a bunch of linters, formatters and code analysis tools pre-installed.
In the case of TypeScript all major IDEs (VS Code, WebStorm, IntelliJ Ultimate) use the same linter under the hood, with the **same** ruleset.
So, even if your team members use different IDEs (on maybe even different operating systems) the linting results will always be the same for everyone.

Yes, you are losing control over the linting rules.
Yes, you or your teammates will probably not like some of the rules.
But, do you have such a specific use case, that the default settings, that millions of developers use don't work for you? 
Can you justify the time-invest that is needed for your custom solution?

## Implementation Strategies

There are multiple options to get started with the IDE linting.
Depending on the age, size, code quality and test coverage of the project the strategy can be more aggressive or needs to be more defensive.

### New projects

For new projects you can start quite aggressive. 
The IDE should be configured to automatically format every file that is touched (see next chapter).
The linting rules should **not** be customized.
Just use the predefined rules as selected by the IDE.

Try it out for a few weeks, then evaluate.
Has it improved your productivity? 
Has it improved the code quality? 
Has it improved the code review process? 
E.g. are there now less comments about styling issues, but more feedback about the actual code architecture?

If there was anything annoying about the linting rules or the process itself, then try to improve it.
For example, by default VS Code applies two different sets of linting rules to TypeScript files.
To keep the code consistent across the whole team the IDE-specific rules need to be disabled.

Try again for a few weeks.
What might happen is that your team is not happy with some of the linting rules.
Should they really be a blocker, check if you can modify some of the rules.
In the case of TypeScript you can create a `tslint.json` or `tslint.yaml` file and adjust the rules in there.
Don't forget to check in the config file, so the whole team still works with the same ruleset.

But, beware! You have now opened Pandora's box.
The original idea was to avoid long discussions in the team - they will now return.
We wanted to avoid maintaining the linter - this will now be necessary (if only a little bit).

### Existing projects

In existing projects it might be sensible to start slow and in a very controlled manner.
The first step could be to only allow manually triggered reformatting - no automations (yet).
You can go even further and only allow the IDE to format the **changed** lines, not the **whole** file.

Try it out for a few weeks, then evaluate.
Has it improved your productivity?
Has it improved the code quality?
Has it improved the code review process?
Did the formatters break anything?

If everything worked fine, then you can get braver.
Let the IDE always reformat the whole file, not just the changed lines.
Try it out for a few weeks, then evaluate.

If everything worked fine, get braver again.
Let the IDE reformat your code automatically on each save.
Try it out for a few weeks, then evaluate.

This strategy allows you to slowly (but steadily) improve the code quality of an existing project.
Opposed to a big bang solution there is no time pressure.
Also, the team can decide when it is ready to do the next step.

## How to configure the IDE?

So, the IDE has all that linting power, but where is it?
How does is work?
Out of the box, the IDE will not clean up your code automatically.
That would be strange and I would feel offended, when the IDE just edits my code without my permission.
You have to trigger the reformatting manually or activate the automation options.

The IDEs usually use the same linters (the most popular ones), with the same configuration.
E.g. `VS Code`, `IntelliJ Ultimate` and `WebStorm` use the same `eslint` rules for TypeScript code.
This means, everyone in the team can use a different IDE, but has the identically formatted code in the end.

The following part shows you how to configure the formatters in `WebStorm`, `IntelliJ` and `VS Code`.

### Reformatting on demand

The most controlled way to clean up the code is to run the formatters manually.
This is the way to go when the code base is too messy to just let an automated script run over the whole file.

#### WebStorm (v2023.3)

In WebStorm the command to clean up the code can be found under:

`Code` → `Reformat Code`

For a more fluent workflow the command can also be triggered via keyboard shortcut:

* `CTRL` + `ALT` + `L` (Linux)
* `CTRL` + `ALT` + `L` (Windows)
* `CMD` + `OPT` + `L` (MacOS)

#### IntelliJ Ultimate (v2023.3)

In IntelliJ Ultimate the command to clean up the code can be found under:

`Code` → `Reformat Code`

For a more fluent workflow the command can also be triggered via keyboard shortcut:

* `CTRL` + `ALT` + `L` (Linux)
* `CTRL` + `ALT` + `L` (Windows)
* `CMD` + `OPT` + `L` (MacOS)

⚠️ Those commands and shortcuts also exist in the Community Edition, but might be limited.
TypeScript linting and reformatting for example is only supported in the Ultimate version.

#### Visual Studio Code

In Visual Studio Code the code cleanup can be triggered by those keyboard shortcuts:

* `CTRL` + `SHIFT` + `I` (Linux)
* `SHIFT` + `ALT` + `F` (Windows)
* `SHIFT` + `OPT` + `F` (MacOS)

⚠️ VS Code has additional formatting rules that might interfere with the default linters.
It might be necessary to deactivate those to get a consistent result across all IDEs.
See `File` → `Preferences` → `Settings` → `Extensions`.

### Going one step further: Auto-format on save

Manually triggering the reformatting means that you have full control over it.
You can decide which files should be cleaned up and which not.
But - being a human - sometimes you will forget to run the formatter.
Then, the next time you work on the same file you will be presented with a bunch of changes that have nothing to do with your current changes.

By using an auto-formatter your code will be cleaned up every time you save.
Because it is (nearly) impossible to forget to save you also cannot forget to clean up.
This also means, a PR cannot be rejected anymore because it violated one of the linting rules.

#### WebStorm (v2023.3)

In WebStorm you find the setting to auto-format on save here:

`Files` → `Settings` → `Tools` → `Actions on Save` → `Reformat Code`

When activated it gives you the option to select the filetypes for which the reformatting should be applied.
By default, every filetype is selected.
It might make sense to reduce that list to only the filetypes you are actively working on.
In a TypeScript project that could be for example `*.ts`, `*.tsx` and `*.json` files.

You can also decide whether the auto-formatter should run on the whole file or just the code you changed.
For new projects the default option `whole file` should be fine.
For existing (messy) projects it might be sensible to start with the `changed files` setting first.
Then, a one-liner doesn't bloat the whole commit, just because the linter found so many issues.

#### IntelliJ Ultimate (v2023.3)

In IntelliJ Ultimate you find the setting to auto-format on save here:

`Files` → `Settings` → `Tools` → `Actions on Save` → `Reformat Code`

When activated it gives you the option to select the filetypes for which the reformatting should be applied.
By default, every filetype is selected.
It might make sense to reduce that list to only the filetypes you are actively working on.
In a TypeScript project that could be for example `*.ts`, `*.tsx` and `*.json` files.

You can also decide whether the auto-formatter should run on the whole file or just the code you changed.
For new projects the default option `whole file` should be fine.
For existing (messy) projects it might be sensible to start with the `changed files` setting first.
Then, a one-liner doesn't bloat the whole commit, just because the linter found so many issues.

⚠️ In the Community Edition this feature works exactly the same, but is limited to the supported languages.
TypeScript for example is only supported in the Ultimate version.

#### Visual Studio Code (v1.85)

In VS Code you find the setting to auto-format on save here:

`File` → `Preferences` → `Settings` → `Text Editor` → `Formatting` → `Format On Save`

This will enable auto-format for every filetypes for which the IDE finds a linter/formatter. 

With `Format On Save Mode` you can decide whether the auto-formatter should run on the whole file or just the code you changed.
For new projects the default option `file` should be fine.
For existing (messy) projects it might be sensible to start with the `modifications` setting first.
Then, a one-liner doesn't bloat the whole commit, just because the linter found so many issues.

### Automate all the things! Auto-save + Auto-format

The ultimate combo is using the "format on save" feature together with "auto-save".
When activated, you can fully concentrate on writing code.
You don't have to remember to save anymore.
You don't have to remember to run the formatter anymore.
You just write your code.
The IDE will automatically save your changes and trigger the linters and formatters for you.

#### WebStorm (v2023.3)

By default, WebStorm already saves automatically.
So, if you activated the auto-format feature, then you already have the full combo.

The auto-save is event-based.
It will be triggered when you switch to the integrated terminal or the test runner or the git commit panel or switch to the browser.
It will not be triggered, when switching between files.

#### IntelliJ Ultimate (v2023.3)

By default, IntelliJ Ultimate already saves automatically (Community Edition works the same).
So, if you activated the auto-format feature, then you already have the full combo.

The auto-save is event-based.
It will be triggered when you switch to the integrated terminal or the test runner or the git commit panel or switch to the browser.
It will not be triggered, when switching between files.

#### Visual Studio Code (v1.85)

In VS Code the auto-save feature can be toggled in the main menu:

`File` -> `Auto Save`

⚠️ By default, the auto-save is time-based.
It will save the currently edited file after one second.
**But**, the time-based auto-save does **not** work together with the auto-format.
To combine both features the setting

`File` → `Preferences` → `Settings` → `Text Editor` → `Files` → `Auto Save`

needs to be changed to `onFocusChange` or `onWindowChange`.

Now, the auto-save is event-based and will automatically trigger the auto-format.

## Links

* "Formatters, linters, and compilers: Oh my!" - Josh Goldberg https://github.com/readme/guides/formatters-linters-compilers
* List of all linting rules in ESLint: https://eslint.org/docs/latest/rules
* Format / Auto Format in WebStorm: https://www.jetbrains.com/help/webstorm/reformat-and-rearrange-code.html
* Save / Auto Save in VS Code: https://code.visualstudio.com/docs/editor/codebasics#_save-auto-save
* Format / Auto Format in VS Code: https://code.visualstudio.com/docs/editor/codebasics#_formatting
