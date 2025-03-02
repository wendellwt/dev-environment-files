# My own version of Dev Environment Files

This is my own version of
[Josean's](https://github.com/josean-dev)
very excellent
[LazyVim](https://www.lazyvim.org/)
configuration.

He has many excellent
[videos](https://www.youtube.com/watch?v=NL8D8EkphUw)
that explain what these are and how they work.

## Build NeoVIM

Use decent gcc and cmake:

```
    gcc 8.x
    cmake-3.28.1
```

Compile NeoVIM from sources, using the guidelines from
[here](https://github.com/neovim/neovim/blob/master/INSTALL.md#install-from-source).

```
    git clone https://github.com/neovim/neovim
    cd neovim

    make CMAKE_EXTRA_FLAGS="-DCMAKE_INSTALL_PREFIX=$HOME/neovim -DCMAKE_BUILD_TYPE=RelWithDebInfo"
    make install
    export PATH="$HOME/neovim/bin:$PATH"
```

## Before starting NeoVIM

Before starting your new nvim the first time,
make sure all associated/support tools are
installed and in your PATH:

```
   gcc   --version    # Red Hat 8.3.1-3
   cmake --version    # cmake version 3.22.2

   nvim  --version    # NVIM v0.10.0-dev-2255+g4c9119461
   git   --version    # 2.29
   node  --version    # Node version must be >= 14
   npm   --version    # npm version must be >= 7

   cargo --version    # cargo 1.74.1
   rg    --version    # ripgrep 13.0.0
   fd    --version    # fd 8.6.0

   pylint  --version  # pylint 2.12.2
   pyright --version  # pyright 1.1.347
```

These may require:

```
    export PATH=/opt/rh/devtoolset-8/root/usr/bin:$PATH
    export PATH=$HOME/pkgs/git-2.29.3:$PATH
```

The rest of this section describes the misc procedures used to ensure that recent
versions of those tools are installed.

### Install workable versions of node, npm, etc.

**node, npm**:

```
    wget https://nodejs.org/dist/v16.20.2/node-v16.20.2-linux-x64.tar.gz
    tar zxvf node-v16.20.2-linux-x64.tar.gz
```

These may require:

```
    export PATH=$HOME/pkgs/node-v16.20.2-linux-x64/bin:$PATH
```

**cargo**:

```
    curl https://sh.rustup.rs -sSf | sh

    info: downloading installer
    Cannot execute /tmp/tmp.RzCLQH8sSn/rustup-init (likely because of mounting /tmp as noexec).
    $ mkdir cargo
    $ cd cargo
    $ cp /tmp/tmp.RzCLQH8sSn/rustup-init .
    $ ./rustup-init
    Welcome to Rust!
    ...
    info: default toolchain set to 'stable-x86_64-unknown-linux-gnu'
    stable-x86_64-unknown-linux-gnu installed - rustc 1.75.0 (82e1608df 2023-12-21)

    Rust is installed now. Great!
    To get started you may need to restart your current shell.
    This would reload your PATH environment variable to include
    Cargo's bin directory ($HOME/.cargo/bin).

    To configure your current shell, run:
    source "$HOME/.cargo/env"
```

**ripgrep**:

from: [BurntSushi/ripgrep](https://github.com/BurntSushi/ripgrep)

```
    git clone https://github.com/BurntSushi/ripgrep
    cd ripgrep
    cargo build --release
    cp target/release/rg ~/local/bin
```

and then:

```
    rg --version
    ripgrep 14.1.0 (rev 9b42af96f0)
```

**fd (find files)**:

from: [sharkdb/fd](https://github.com/sharkdp/fd) (do _not_ install the yum verion)

```
    cargo install fd-find
```

or possibly:

```
    cd ~/pkgs/fd
    mkdir tmp
    CARGO_TARGET_DIR=~/pkgs/fd/tmp cargo install fd-find
```

and then:

```
    fd --version
    fd 9.0.0
```

these are apparently needed also:

```
    npm install -g neovim
    pip3 install neovim pylint pyright
```

## Starting NeoVIM

if you already had nvim running (or attempted), move all of those configurations aside:

```
    cd ~/.config       ; mv nvim nvim_feb2
    cd ~/.local/share  ; mv nvim nvim_feb2
    cd ~/.local/state  ; mv nvim nvim_feb2
    cd ~/.cache        ; mv nvim nvim_feb2
```

## Install config

After viewing Josean's videos listed in his
[My Dev Environment Files](https://github.com/josean-dev/dev-environment-files)
page, I finally worked out a set of config files that actually work!
Note that this config is based on
[LazyVim](https://www.lazyvim.org/),
i.e., it uses the LazyVim plugin manager.

I started with that config, and then
made some of my own changes to it:

- I use other languages, so these LSPs were added:
  - VueJS, bash, awk, json
- I don't want source code automatically reformatted, so:
  - black, flake8, isort were explicitly commented out (in several places)

Now, do something like this:

```
    cd lazyvim/
    git clone git@github.com:wendellwt/dev-environment-files.git
    cd dev-environment-files/.config
    cp -R nvim ~/.config/
```

If it all seems in place, start nvim and watch it install everything:

```
    nvim

    :checkhealth
```

Yipee!!!, right?

## NerdFont on windows

refs:
Beautify your Windows Terminal using
[Nerd Fonts](https://www.nerdfonts.com/font-downloads)
and Oh-My-Posh

which says to:

```
  mkdir fira
  cd fira/
  wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.0.1/FiraCode.zip  unzip FiraCode.zip
```

and then, in File Explorer:

```
    highlight all .ttf files
      -> Install
```

then, in PuTTY:

```
  -> Load
  -> Appearance
    FiraCode Nerd Font Mono
```

However, take a look at these, they may be better:

- [Martian](https://www.programmingfonts.org/#martian-mono)
- [Meslo](https://www.programmingfonts.org/#meslo)
- [SauceCodePro](https://www.programmingfonts.org/#source-code-pro)
- [MonaSpace](https://www.programmingfonts.org/#monaspace-neon)

## Keymaps used in the above configuration

in the following, <> is <leader>, which is set to a space

```
formatting
  <>mp function()

telescope
  <>ff <cmd>Telescope find_files  Fuzzy find files in cwd
  <>fr <cmd>Telescope oldfiles    Fuzzy find recent files
  <>fs <cmd>Telescope live_grep   Find string in cwd
  <>fc <cmd>Telescope grep_string Find string under cursor in cwd

nvim-treesitter
  ; ts_repeat_move.repeat_last_move
  , ts_repeat_move.repeat_last_move_opposite
  f ts_repeat_move.builtin_f
  F ts_repeat_move.builtin_F
  t ts_repeat_move.builtin_t
  T ts_repeat_move.builtin_T

linting
  vim.<>l function()

nvim-tree:
  <>ee NvimTreeToggle         Toggle file explorer
  <>ef NvimTreeFindFileToggle Toggle file explorer on current file
  <>ec NvimTreeCollapse       Collapse file explorer
  <>er NvimTreeRefresh        Refresh file explorer

lsp/lspconfig
  gR <cmd>Telescope lsp_references       -- show definition, references
  gD vim.lsp.buf.declaration             -- go to declaration
  gd <cmd>Telescope lsp_definitions      -- show lsp definitions
  gi <cmd>Telescope lsp_implementations  -- show lsp implementations
  gt <cmd>Telescope lsp_type_definitions -- show lsp type definitions
  <>ca vim.lsp.buf.code_action           -- see available code actions, in visual mode will apply to selection
  <>rn vim.lsp.buf.rename                -- smart rename
  <>D <cmd>Telescope diagnostics bufnr=0 -- show  diagnostics for file
  <>d vim.diagnostic.open_float          -- show diagnostics for line
  [d vim.diagnostic.goto_prev            -- jump to previous diagnostic in buffer
  ]d vim.diagnostic.goto_next            -- jump to next diagnostic in buffer
  K vim.lsp.buf.hover                    -- show documentation for what is under cursor
  <>rs :LspRestart                       -- mapping to restart lsp if necessary

harpoon
  <>hn <cmd>lua require('harpoon.ui').nav_next() Go to next harpoon mark

auto-session
  <>wr <cmd>SessionRestore  Restore session for cwd
  <>ws <cmd>SessionSave     Save session for auto session root dir

keymaps
  jk <ESC> Exit insert mode with jk
  <>nh :nohl Clear search highlights
  <>+ <C-a> Increment number                        -- increment
  <>- <C-x> Decrement number                        -- decrement
  <>sv <C-w>v Split window vertically               -- split window vertically
  <>sh <C-w>s Split window horizontally             -- split window horizontally
  <>se <C-w>= Make splits equal size                -- make split windows equal width & height
  <>sx <cmd>close Close current split               -- close current split window
  <>to <cmd>tabnew Open new tab                     -- open new tab
  <>tx <cmd>tabclose Close current tab              -- close current tab
  <>tn <cmd>tabn Go to next tab                     --  go to next tab
  <>tp <cmd>tabp Go to previous tab                 --  go to previous tab
  <>tf <cmd>tabnew % Open current buffer in new tab --  move current buffer to new tab
```
