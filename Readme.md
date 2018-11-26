
# Reason Language Server

This project implements the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/specification#initialize) for Reason.

It is written in Reason, and compiled via bsb-native. The goal is for it to work equally well on Windows, MacOS, and Linux.

## Platform support

- Macos ✅
- Linux ✅
- Windows ✅ (but **not under WSL**, make sure you install bs-platform etc. from the windows side)

## Installation

### VS Code

Install through VS Code extensions. Search for `reason-vscode`:

[Visual Studio Code Marketplace: reason-vscode](https://marketplace.visualstudio.com/items?itemName=jaredly.reason-vscode)

Can also be installed with VS Code Quick Open: press `Cmd/Ctrl + P`, paste the following command, and press enter.

```
ext install jaredly.reason-vscode
```

### OniVim

Oni has support for Reason baked in, and it only needs a little bit of configuration to integrate this langauge server.

1. Download the `your-platform.zip` file from the [latest release](https://github.com/jaredly/reason-language-server/releases), unzip it, and put the `reason-language-server.exe` file somewhere.
2. Open the Oni config file (` Ctrl/Cmd + ,`), and add the following to the `export const configuration = {` block at the end:
```json
    "language.reason.languageServer.command": "/abs/path/to/your/reason-language-server.exe",
    "language.reason.languageServer.arguments": [],
    "language.reason.languageServer.configuration": {},
```
And you're done!

### Sublime Text

1. Install the [sublime reason](https://github.com/reasonml-editor/sublime-reason) plugin for syntax highlighting, etc. (has to be done manually)
2. Install the [LSP Plugin](https://github.com/tomv564/LSP) via the Sublime Text Package Manager
3. Download the `your-platform.zip` file from the [latest release](https://github.com/jaredly/reason-language-server/releases), unzip it, and put the `reason-language-server.exe` file somewhere.
4. `Ctrl/Cmd + Shift + P` and type "Preferences: LSP Settings" to bring up the settings file, and put in:
```json
{
  "auto_show_diagnostics_panel": false,
  "clients": {
    "reason": {
      "enabled": true,
      "command": ["/absolute/path/to/reason-language-server.exe"],
      "scopes": ["source.reason"],
      "syntaxes": ["Packages/Reason/Reason.tmLanguage"],
      "languageId": "reason"
    }
  }
}
```

### Vim

1. Download the `your-platform.zip` file from the [latest release](https://github.com/jaredly/reason-language-server/releases), unzip it, and put the `reason-language-server.exe` file somewhere.
2. Install [the vim-reason-plus plugin](https://github.com/reasonml-editor/vim-reason-plus), following the README. Add the following to your `.vimrc` file:
```vim
let g:LanguageClient_serverCommands = {
    \ 'reason': ['/absolute/path/to/reason-language-server.exe']
    \ }
```

#### Configuration

[LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim) supports configuring language servers via a configuration file. By default the configuration file is `.vim/settings.json` in the root of your project. For example to set the format width the file will contain:

```json
{
  "reason_language_server": {
    "format_width": 100
  }
}
```

##### Supported settings

Note that some of these might be VS Code specific and not relevant to vim use.

- `format_width` - defaults to 80 (int)
- `per_value_codelens` - show the type of each toplevel value in a lens (bool)
- `dependencies_codelens` - list a files dependencies at the top (bool)
- `opens_codelens` - show what values are used from an `open` (bool)
- `autoRebuild` — rebuild project on save (turned on by default) (bool)

##### Debug settings

Useful if you're developing the language server itself.

- `location` - provide a custom binary location for the langauge server (string)
- `refmt` - provide a custom location for refmt (string)
- `lispRefmt` - provide a custom location for reason-lisp's refmt (string)
- `reloadOnChange` - reload the server when the binary is updated (bool)
- `show_debug_errors` - pipe the server's stderr into the output pane (bool)

### Emacs

_TODO_ people have gotten it to work with emacs, but I don't know the steps.

### Troubleshooting

NOTE: reason-language-server runs bsb or dune *automatically* for you, it is not necessary to have a separate process running `bsb -w`. In fact, having a separate process running can sometimes get the editor into a weird state.

If your editor is in a weird state, the first recourse (if you're in VSCode) is to restart the reason-language-server, with `cmd+shift+p "restart reason language server"`. That often clears things up.

If that doesn't work, try cleaning built artifacts, with `bsb -clean-world` (or for dune, `dune clean`). Then try to build, with `bsb -make-world` (or `dune build @install`) and see if that works.

## What about the [ocaml-language-server](https://github.com/freebroccolo/ocaml-language-server/)?

That project uses [merlin](https://github.com/ocaml/merlin) under the hood, which is a very powerful and well-developed tool for IDE features in OCaml/Reason.
I had a couple of reasons for starting a new one. The biggest is that I wanted something with minimal dependencies, so that windows support would be relatively easy, and so that I might be able to ship it with bucklescript at some future point. (it also makes targetting JS easier). I also wanted a server that was written entirely in Reason (not part typescript, part reason), and something that was written from the ground up with the Langauge Server Protocol in mind, instead of taking a different IDE-support-tool and mapping the LSP onto it.

## Contributing

- Install `esy` if you don't have it (`npm install -g esy`)
- Clone this repo
- `cd` to the cloned dir
- Run `esy` from the main project dir
- Install the VS Code extension's dependencies `cd editor-extensions/vscode && npm i && cd ../..`
- Open this project in VS Code

## To test your changes in one of the example projects
- Open the "Debug" pane in VS Code. Select a debug target. Press "Run"
- It should start up a window titled `[Extension development host]`
- Run `npm install && npm run build` in the project directory
- To reload the server if something goes wrong: `Ctrl/Cmd + Shift + P` "Restart Reason Language Server"

You can then develop on the language server! When you change something, the server will automatically reload.

## Copyright & License

Copyright © 2018 Jared Forsyth and contributors.

Distributed under the MIT License (see [LICENSE](./LICENSE)).
