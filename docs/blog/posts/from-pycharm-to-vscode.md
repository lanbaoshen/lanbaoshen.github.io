---
date: 2026-02-11
categories:
  - Efficiency
---

# From PyCharm to VS Code

After five years of extensively using PyCharm Professional, I’ve encountered several persistent issues that ultimately led me to switch to VS Code. These include:

1. **Plugin Development and Maintenance**: Challenges in developing and maintaining PyCharm plugins.
2. **Performance Issues**: Slow operation and excessive resource consumption.
3. **Remote Development Limitations**: Weak support for remote development workflows.
4. **AI Extension Bugs**: Frequent bugs in AI-powered features.
5. **Critical Bugs**: Increasingly severe issues requiring manual deletion of cache and library files to restore functionality.

Given these frustrations, I decided to transition to VS Code. This document outlines the steps I took during the migration from PyCharm to VS Code.

<!-- more -->

## Install

First, download and install VS Code from the official website: [https://code.visualstudio.com/](https://code.visualstudio.com/).

!!! Tips

    If you installed VS Code before, you can move VS Code from `Application` to trash, and then execute `rm -rf ~/Library/Application\ Support/Code` and `rm -rf ~/.vscode`.

## GitHub Copilot

Afterward, when you open VS Code, the tutorial **Get started with VS Code** will appear.
We can refer to the tutorial to complete the configuration of GitHub Copilot.

!!! Tips

    You can enter `> walkthrough` and then select `Welcome: Open Walkthrough...` to reopen tutorial

## Theme

I set `GitHub Dark Default` as the theme. You can click `Browser Color Themes` to search it,
or enter `> theme` into the `Command Palette` and select `Preferences: Browse Color Themes in Marketplace`.

## Extensions

- `Python`
- `Remote - SSH`
- `Docker`
- `IntelliJ IDEA Keybindings`
- `SQL Database Projects`

## Settings

Press `⌘` + `,` to open settings.

- Set `Files: Auto Save` to `afterDelay`, and the default delay in milliseconds is `1000`.
- Enable `files.trimFinalNewlines`
- Enable `files.insertFinalNewline`
- Enable `files.trimTrailingWhitespace`
- Set `workbench.colorCustomizations`

```json title="~/Library/Application Support/Code/User/settings.json"
{
    "github.copilot.nextEditSuggestions.enabled": true,
    "workbench.colorTheme": "GitHub Dark Default",
    "files.autoSave": "afterDelay",
    "files.trimFinalNewlines": true,
    "files.insertFinalNewline": true,
    "files.trimTrailingWhitespace": true,
    "workbench.colorCustomizations": {
        "gitDecoration.modifiedResourceForeground": "#5a9bea",
        "gitDecoration.addedResourceForeground": "#00FF00",
        "gitDecoration.untrackedResourceForeground": "#ff5757",
        "gitDecoration.ignoredResourceForeground": "#e2d227",
        "gitDecoration.conflictingResourceForeground": "#FF69B4",
        "gitDecoration.deletedResourceForeground": "#535151"
    },
    "git.autofetch": true
}
```

PS: I use `ruff` to format my code, so I haven't configured the formatter-related features in VS Code.

## Keyboard Shotcuts

```json title="~/Library/Application Support/Code/User/keybindings.json"
// Place your key bindings in this file to override the defaults
[
    {
        "key": "cmd+t",
        "command": "-workbench.action.showAllSymbols"
    },
    {
        "key": "cmd+t",
        "command": "-git.sync",
        "when": "!operationInProgress"
    },
    {
        "key": "cmd+t",
        "command": "workbench.action.terminal.new",
        "when": "terminalProcessSupported || terminalWebExtensionContributedProfile"
    },
    {
        "key": "ctrl+shift+`",
        "command": "-workbench.action.terminal.new",
        "when": "terminalProcessSupported || terminalWebExtensionContributedProfile"
    },
    {
        "key": "cmd+w",
        "command": "workbench.action.terminal.chat.close",
        "when": "chatIsEnabled && terminalChatFocus && terminalChatVisible || chatIsEnabled && terminalChatVisible && terminalFocus"
    },
    {
        "key": "escape",
        "command": "-workbench.action.terminal.chat.close",
        "when": "chatIsEnabled && terminalChatFocus && terminalChatVisible || chatIsEnabled && terminalChatVisible && terminalFocus"
    },
    {
        "key": "delete",
        "command": "-workbench.action.terminal.killActiveTab",
        "when": "terminalHasBeenCreated && terminalTabsFocus || terminalIsOpen && terminalTabsFocus || terminalProcessSupported && terminalTabsFocus"
    },
    {
        "key": "cmd+backspace",
        "command": "-workbench.action.terminal.killActiveTab",
        "when": "terminalHasBeenCreated && terminalTabsFocus || terminalIsOpen && terminalTabsFocus || terminalProcessSupported && terminalTabsFocus"
    },
    {
        "key": "ctrl+r",
        "command": "python.execInTerminal-icon"
    },
    {
        "key": "cmd+w",
        "command": "-workbench.action.terminal.killEditor",
        "when": "terminalEditorFocus && terminalFocus && terminalHasBeenCreated || terminalEditorFocus && terminalFocus && terminalProcessSupported"
    },
    {
        "key": "cmd+w",
        "command": "workbench.action.terminal.kill",
        "when": "terminalFocus"
    },
    {
        "key": "cmd+k alt+cmd+s",
        "command": "-git.stageSelectedRanges",
        "when": "editorTextFocus && !operationInProgress && resourceScheme == 'file'"
    },
    {
        "key": "cmd+k cmd+n",
        "command": "-git.unstageSelectedRanges",
        "when": "editorTextFocus && isInDiffEditor && isInDiffRightEditor && !operationInProgress && resourceScheme == 'git'"
    },
    {
        "key": "alt+cmd+z",
        "command": "-git.revertSelectedRanges",
        "when": "editorTextFocus && !editorReadonly && !operationInProgress"
    },
    {
        "key": "cmd+k cmd+r",
        "command": "-git.revertSelectedRanges",
        "when": "editorTextFocus && !operationInProgress && resourceScheme == 'file'"
    },
    {
        "key": "alt+cmd+k",
        "command": "-git.pushTo",
        "when": "!inDebugMode && !operationInProgress && !terminalFocus"
    },
    {
        "key": "f4",
        "command": "-git.openFile",
        "when": "config.git.enabled && isInDiffEditor"
    },
    {
        "key": "cmd+k",
        "command": "-git.commitAll",
        "when": "!inDebugMode && !operationInProgress && !terminalFocus"
    },
    {
        "key": "alt+cmd+a",
        "command": "git.stage",
        "when": "explorerViewletVisible && filesExplorerFocus && !inputFocus"
    }
]
```
