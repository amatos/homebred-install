# Brewfile: Homebrew bundle file for macOS or Linux app install (with notes/source repos)
#
# Author: Alberth Matos (alberth@matos.cc)
# Original Author  : Chad Mayfield (chad@chadmayfield.com)
# Original Source: https://gist.github.com/chadmayfield/ada07e4e506d7acd577a665541a70c9b
# License : GPLv3
#
# Last Modified: 2026-01-19
#
#
# INFO: * Links and comments are added because I'm too old to remember where everything is :)
#       * install.sh can be used, but it needs some love (some of it doesn't work correctly).
#       * TODO items (and additional information) is listed at the bottom
#
# Installation steps;
#   1. Install Xcode CLI Tools: sudo xcode-select --install
#   2. Install Homebrew: /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
#   3. Install Brewfile: brew bundle install
#   4. Verify Brewfile Install: brew bundle check --verbose
#
# Optional steps;
#   1. Mirror system to Brewfile: brew bundle --force cleanup (will delete non-existant pkgs)
#   2. Check for current updates: brew cu -a -f --include-mas
#   3. Setup autoupdate every 12 hours: brew autoupdate start 43200
#
# Ruby Modules
#            OS : https://rubydoc.brew.sh/OS.html (https://github.com/rdp/os)
#
# --- My Homebrew cheatsheet --------------------------------
#
# Brew Docs     : https://docs.brew.sh/
# Brew Manpage  : https://docs.brew.sh/Manpage
# Brewfile Docs : https://github.com/Homebrew/homebrew-bundle
# FAQs          : https://docs.brew.sh/FAQ
# Terminology   : https://docs.brew.sh/Formula-Cookbook#homebrew-terminology
# Linuxbrew     : https://docs.brew.sh/Homebrew-on-Linux
#
# brew help <cmd>            # print help for sub-command
# brew doctor                # diagnose brew issues
# brew analytics <on|off>    # turn analytics on or off
# brew shellenv              # display env variable exports
# brew --config              # display brew configurations
# brew --cache               # display cache download location
# brew --caskroom            # display caskroom location for GUI apps
# brew --cellar              # display location of CLI apps
# brew update                # update brew and cask
# brew upgrade               # upgrade all formulae
# brew upgrade --greedy      # upgrade all formulae and casks with auto_update: true
# brew autoupdate start      # start homebrew autoupdate (required interval)
# brew autoupdate status     # check status of autoupdate
# brew cu                    # upgrade mac apps using 'buo/cask-upgrade'
# brew list                  # list installed
# brew list --cask           # list installed applications
# brew list --versions       # list installed versions
# brew tap                   # list current tapped repos
# brew deps --tree <frmla>   # show dependencies
# brew outdated              # what is due for an upgrade
# brew leaves                # display unused formula to uninstall
# brew cleanup               # remove older formulae versions
# brew search <string>       # search for formulae
# mas search <string>        # use mas to search for App Store apps
# brew info <formula>        # display info on formulae
# brew install <formula>     # install formulae
# brew uninstall <formula>   # uninstall formulae
# brew pin <formula>         # pin at version (to prevent upgrades)
# brew cu pin <caskname>     # pin cask at version (to prevent upgrades)
# brew bundle list           # list deps in Brewfile
# brew bundle check -v       # check if apps from brewfile are installed
# brew bundle cleanup        # cleanup unused deps left
#
# -----------------------------------------------------------

require 'date'
require 'open-uri'

# brew version
hb = `/opt/homebrew/bin/brew -v`
# bash version
bv = `bash -c 'echo $BASH_VERSION'`
sh = `echo $SHELL`
# current date
now = DateTime.now
now.strftime("%B %d %Y")
# auto-update status
au = ENV.fetch("HOMEBREW_AUTO_UPDATE_COMMAND")
status = au ? "True" : "False"

# fail if Homebrew is not installed, (or if it's not in $
if !hb.include? "Homebrew"
  abort("ERROR: Homebrew does not appear to be installed!")
end

# display some basic system env information
puts("--------------------------------")
puts("HOMEBREW_PRODUCT    : " + ENV.fetch("HOMEBREW_PRODUCT"))
puts("HOMEBREW_SYSTEM     : " + ENV.fetch("HOMEBREW_SYSTEM"))
puts("HOMEBREW_OS_VERSION : " + ENV.fetch("HOMEBREW_OS_VERSION"))
puts("HOMEBREW_VERSION    : " + ENV.fetch("HOMEBREW_VERSION"))
puts("HOMEBREW_PROCESSOR  : " + ENV.fetch("HOMEBREW_PROCESSOR"))
puts("AUTO_UPDATE_ENABLED : " + status + " (" + au + ")")
puts("BASH_VERSION        : " + bv)
puts("CURRENT_USER_SHELL  : " + sh)
puts("--------------------------------")
puts("\n")

# give us time to CTRL-C
sleep(5)

###############################################################################
# Add some third-party repos to use.
#
# 'brew tap user/repo'
#
# Docs: https://docs.brew.sh/Taps
#       https://docs.brew.sh/Interesting-Taps-and-Forks
#
###############################################################################

# official taps
tap "homebrew/autoupdate"     # [https://github.com/Homebrew/homebrew-autoupdate]

# unnecessary taps in v4: https://brew.sh/2023/02/16/homebrew-4.0.0/
if ENV.fetch("HOMEBREW_VERSION") < '4.0.0'
  tap "homebrew/cask"         # [https://github.com/Homebrew/homebrew-cask]
  tap "homebrew/core"         # [https://github.com/Homebrew/homebrew-core]
end

# third-party taps
tap "buo/cask-upgrade"        # 'brew cu' [https://github.com/buo/homebrew-cask-upgrade]
tap "dracula/install"         # dracula [https://github.com/dracula/homebrew-install]
tap "felixkratz/formulae"   # Borders and Sketchybar [https://github.com/FelixKratz/homebrew-formulae]
tap "mfkrause/tap"          # Consul
tap "wader/tap"



###############################################################################
# Install most used formulae.
#
# 'brew install <formula>'
#
# Docs: https://docs.brew.sh/
#       https://docs.brew.sh/Bottles
#       https://docs.brew.sh/Tips-N'-Tricks
#
# Formula browser: https://formulae.brew.sh/
#
###############################################################################

### homebrew, appstore & docker deps
brew "mas"                   # mac app store cli [https://github.com/mas-cli/mas]
brew "whalebrew" if File.symlink?("/usr/local/bin/docker") # hombrew for docker [https://github.com/whalebrew/whalebrew]

### ansible
brew "ansible", link: :overwrite # config mgmt [https://www.ansible.com]
brew "ansible-lint", link: :overwrite # check best practices/behavior [https://ansible-lint.readthedocs.io/]

### backup utilities
brew "tarsnap" if OS.linux? # online backups for the paranoid [https://www.tarsnap.com/]
cask "arq" if OS.mac?       # Arq, if on macOS (https://www.arqbackup.com/]

### cloud
# brew "awscli", link: :overwrite # aws cli client [https://aws.amazon.com/cli/]
# brew "azure-cli"             # ms azure cli client [https://docs.microsoft.com/cli/azure/overview]

### communications
brew "bandwhich"             # bandwidth utilization tool [https://github.com/imsnif/bandwhich]
brew "curl"                  # cli data xfers [https://curl.se]
brew "curlie"                # power of curl, ease of httpie [https://github.com/rs/curlie]
brew "httpie"                # human-friendly http cli client [https://github.com/httpie/httpie] https://httpie.io/docs/cli
brew "netcat"                # network util [https://netcat.sourceforge.net/]
brew "speedtest-cli", link: true # speedtest.net cli bandwidth test [https://github.com/sivel/speedtest-cli]
brew "wget"                  # internet file retreiver [https://www.gnu.org/software/wget/]
brew "yt-dlp", link: :overwrite # download all the videos [https://github.com/yt-dlp/yt-dlp]

### development tools
# build
brew "act"                   # run GH actions locally [https://github.com/nektos/actions]
brew "make"                  # direct complication [https://www.gnu.org/software/make/]
# diff tools
brew "diff-so-fancy"         # good-lookin' diffs [https://github.com/so-fancy/diff-so-fancy]
# graphing/visualization
brew "graphviz"              # graphviz [https://graphviz.org/]
brew "graphviz2drawio"       # graphviz to drawio [https://github.com/awslabs/graphviz2drawio]
brew "plantuml"              # plantuml [https://plantuml.com/]
# filter tools (json/yaml/sql/binary)
brew "fx"                    # terminal json viewer [https://github.com/antonmedv/fx]
brew "htmlq"                 # jq for html [https://github.com/mgdm/htmlq]
brew "lemmeknow"             # identify any file [https://github.com/swanandx/lemmeknow]
brew "jo"                    # json ouput in terminal [https://github.com/jpmens/jo]
brew "jq"                    # cli json processor [https://jqlang.github.io/jq/]
brew "sq"                    # jq for sql [https://github.com/neilotoole/sq]
brew "yq"                    # yaml/csv/xml/json processor [https://github.com/mikefarah/yq]
brew "yamllint"              # yaml linter [https://github.com/adrienverge/yamllint]
brew "wader/tap/fq"          # "jq for binary formats" [https://github.com/wader/fq]
# git
brew "act"                   # run github actions locally [https://github.com/nektos/act]
brew "bfg"                   # repo cleaner [https://rtyley.github.io/bfg-repo-cleaner/]
brew "gh"                    # github cli [https://github.com/cli/cli]
brew "git"                   # distributed revision control [https://git-scm.com]
brew "git-extras"            # git utilities [https://github.com/tj/git-extras]
brew "git-flow"              # git-flow branching [https://github.com/nvie/gitflow]
brew "git-lfs"               # git-lfs [https://git-lfs.github.com/]
brew "git-town"              # git-town [https://github.com/git-town/git-town] (https://www.git-town.com/)
brew "lazygit"               # tui for git [https://github.com/jesseduffield/lazygit]
brew "tea"                   # gitea cli client [https://gitea.com/gitea/tea]
# golang
brew "go"                    # https://go.dev/
brew "golangci-lint"         # https://golangci-lint.run/
brew "staticcheck"           # https://staticcheck.io/
brew "delve"                 # https://github.com/go-delve/delve
# brew "tinygo"                # https://tinygo.org/
brew "upx"                   # https://upx.github.io/
# java
brew "groovy"                # [https://www.groovy-lang.org/]
brew "gradle"                # [https://www.gradle.org/]
brew "openjdk"               # Current OpenJDK
brew "openjdk@11"            # OpenJDK 11
brew "openjdk@17"            # OpenJDK 17
brew "openjdk@21"            # OpenJDK 21
brew "jenv"                  # Java version management [https://github.com/jenv/jenv]
brew "maven"                 # Java build tool [https://maven.apache.org/]
# python (should always be installed)
if OS.mac?
  brew "micropython"         # python for microcontrollers [https://www.micropython.org/]
  brew "pyenv", link: false  # simple python version mgmt [https://github.com/pyenv/pyenv]
  brew "pipx"                # exec bins from Python pkgs in isolated envs [https://pipx.pypa.io]
  brew "pyenv-virtualenv", link: false
  brew "python", link: false
end
brew "sphinx-doc"       # Sphinx documentation [https://www.sphinx-doc.org/]
brew "pylint"
brew "pyrefly"
# rpm build tools
brew "create-dmg"            # build fancy DMGs [https://github.com/create-dmg/create-dmg]
if !Hardware::CPU.arm?
  brew "rpm", link: :overwrite # 4.19+ now only bottled for x86_64 (>4.18.1 ventura required)
  brew "rpm2cpio", link: :overwrite
end
# rust
brew "rust"                  # [https://www.rust-lang.org/]
brew "rustup"                # [https://rust-lang.github.io/rustup/]
# Ruby
brew "rbenv"
# Text Editors
brew "neovim" if OS.mac?     # extensible vim-fork [https://neovim.io/]
brew "vim" if OS.linux?

# File/Filesystem utilities
brew "bat"                   # cat with syntax highlighting [https://github.com/sharkdp/bat]
brew "eza", link: :overwrite # modern ls (https://eza.rocks/) [https://github.com/eza-community/eza]
brew "lsd"                   # next gen ls [https://github.com/lsd-rs/lsd]
brew "ripgrep"               # blazing fast grep (https://github.com/BurntSushi/ripgrep)
brew "scc"                   # fast code counter [https://github.com/boyter/scc]
brew "tree"
brew "tre-command"           # tree, improved [https://github.com/dduan/tre]
brew "walk"                  # terminal file manager [https://github.com/antonmedv/walk]
brew "yazi"                  # fast file manager [https://github.com/sxyazi/yazi]
brew "zoxide"                # a smarter cd command [https://github.com/ajeetdsouza/zoxide]
brew "trash-cli"
brew "fzf"                   # fuzzy finder [https://github.com/junegunn/fzf]
brew "direnv"                # environment variables management [https://direnv.net/]

# misc
brew "chezmoi"               # securely sync dotfiles (https://www.chezmoi.io/) [https://github.com/twpayne/chezmoi]
brew "dos2unix"
brew "terminal-notifier"     # macOS notification system [https://github.com/julienXX/terminal-notifier]
brew "pv"                    # pipe viewer, monitor data through pipe [http://www.ivarch.com/programs/pv.shtml]
brew "tealdeer"              # user-friendly man [https://tealdeer-rs.github.io/tealdeer/]
brew "dockutil" if OS.mac?  # a cli utility for managing macOS dock items. [https://github.com/kcrawford/dockutil]
# multiplexers
brew "tmux"                  # https://github.com/tmux/tmux/wiki/Getting-Started (https://tmuxcheatsheet.com/)
brew "tpm"                   # tmux plugin manager [https://github.com/tmux-plugins/tpm]
brew "sesh"                  # tmux session manager [https://github.com/tmux-plugins/sesh]
brew "starship"              # Starship shell prompt
brew "fastfetch"             # system information

# processes/resource mgmt
brew "btop"                  # resource monitor [https://github.com/aristocratos/btop]

brew "htop"
brew "mactop" if OS.mac? && Hardware::CPU.arm?
brew "pstree"

# shells
if OS.mac? && bv.split('.').first >= '4'
  # only upgrade bash if it's been previously installed (stock version is ancient)
  brew "bash"
  brew "bash-completion"
  if !sh.include? "bash"
    # warn to chsh since running shell is not bash
    puts("ATTN: Run chsh to change default shell!")
  end
end
brew "fish"                  # shell for the 90s [https://github.com/fish-shell/fish-shell]
brew "fish-lsp"
# shell lint
brew "shellcheck"            # static analysis tool [https://github.com/koalaman/shellcheck]
brew "shfmt"                 # shell parser [https://github.com/mvdan/sh]

### multimedia tools
brew "ffmpeg"                # record/edit video [https://ffmpeg.org/]
brew "jhead"                 # extract EXIF data [https://github.com/Matthias-Wandel/jhead]
brew "exiftool"              # read/write EXIF data [https://exiftool.org]
brew "ghostscript"           # required for imagemagick
brew "imagemagick"#, args: ["with-webp"]
brew "poppler"               # pdf rendering library [https://poppler.freedesktop.org/]
brew "zbar"                  # barcode reader [https://github.com/ZBar/ZBar]
brew "qrencode"              # QR code generator [https://github.com/fukuchi/libqrencode]

###############################################################################
# Install apps not available as casks from Mac App Store using mas-cli
#
# 'mas install'
#
# Docs: https://github.com/mas-cli/mas
#
# https://github.com/mas-cli/mas#-usage
# list     : mas list
# search   : mas search Xcode
# install  : mas install <id>
# purchase : mas purchase <id>
# upgrade  : mas upgrade
#
# https://github.com/mas-cli/mas#-sign-in
# sign-in : mas signin --dialog mas@example.com
#
###############################################################################

if OS.mac? && File.exist?("/opt/homebrew/bin/mas")
    mas "1Password for Safari", id:1569813296
    mas "Amphetamine", id: 937984704
    mas "Auto HD FPS for YouTube", id:1546729687
    mas "AutoMounter", id:1160435653
    mas "Broadcasts", id:1469995354
    mas "Clean Links", id:6747395062
    mas "Compressor", id:6746516157
    mas "Copilot", id:1447330651
    mas "Dropover", id:1355679052
    mas "Fantastical", id: 975937182
    mas "Final Cut Pro", id:1631624924
    mas "Goban", id: 646372172
    mas "Hyperduck", id:6444667067
    mas "Ivory", id:6444602274
    mas "John's Background Switcher", id: 907640277
    mas "Keynote", id: 361285480
    mas "LanguageTool", id:1534275760
    mas "Logic Pro", id:1615087040
    mas "MainStage", id:6746637089
    mas "Microsoft Excel", id: 462058435
    mas "Microsoft PowerPoint", id: 462062816
    mas "Microsoft Word", id: 462054704
    mas "Motion", id:6746637149
    mas "NepTunes", id:1006739057
    mas "Numbers", id: 361304891
    mas "OneDrive", id: 823766827
    mas "Pages", id: 361309726
    mas "PastePal", id:1503446680
    mas "Photomator", id:1444636541
    mas "Pixelmator Pro", id:6746662575
    mas "Save to Raindrop.io", id:1549370672
    mas "Sink It", id:6449873635
    mas "Steam Link", id:1246969117
    mas "Tailscale", id:1475387142
    mas "Teleprompter", id:6463623914
    mas "Transmit", id:1436522307
    mas "uBlock Origin Lite", id:6745342698
    mas "Windows App", id:1295203466
    mas "WireGuard", id:1451685025
    mas "Xcode", id: 497799835
    mas "Drafts", id: 1435957248
end #OS.mac? && File.exist?("/opt/homebrew/bin/mas")

###############################################################################
# Add our casks (GUI applications) to the system.
#
# 'brew install --cask'
#
# Docs: https://formulae.brew.sh/formula/cask
#       https://github.com/Homebrew/homebrew-cask
#
# Cask browser: https://formulae.brew.sh/cask/
#               https://github.com/Homebrew/homebrew-cask/tree/master/Casks
#
# Pinning: https://apple.stackexchange.com/a/436413
#
###############################################################################

if OS.mac?
  # specify a directory to install
  cask_args appdir: "/Applications"#, require_sha: true

    # AI/LLM DEVELOPMENT CASKS
    cask "chatgpt" # https://chatgpt.com/

    # BASE INSTALL
    cask "1password"
    cask "1password-cli"
    cask "adobe-creative-cloud"
    cask "airfoil"
    cask "audio-hijack"
    cask "autodesk-fusion"
    cask "betterdiscord-installer"
    cask "betterdisplay"
    cask "crossover"
    cask "discord"
    cask "dracula-betterdiscord"
    cask "dracula-macos-color-picker"
    cask "dracula-terminal"
    cask "dracula-xcode"
    cask "elgato-capture-device-utility"
    cask "elgato-stream-deck"
    cask "elgato-studio"
    cask "fission"
    cask "ghostty"
    cask "gpg-suite@nightly"
    cask "handbrake-app"
    cask "hazel"
    cask "helium-browser"
    cask "hex-fiend"
    cask "iina"
    cask "inkscape"
    cask "iterm2"
    cask "itermai"
    cask "jetbrains-toolbox"
    cask "kaleidoscope"
    cask "keyboard-maestro"
    cask "little-snitch"
    cask "mactex"
    cask "makemkv"
    cask "obs"
    cask "obsidian"
    cask "plistedit-pro"
    cask "powerphotos"
    cask "qlcolorcode"
    cask "qlmarkdown"
    cask "qlstephen"
    cask "raycast"
    cask "setapp"
    cask "skim"
    cask "slack"
    cask "soundsource"
    cask "spamsieve"
    cask "spotify"
    cask "steam"
    cask "syncthing-app"
    cask "tower"
    cask "whatsapp"
    cask "winbox"
    cask "windows-app"
    cask "zed"                  # Zed text editor
    cask "zoom"                 # Zoom video conferencing
    cask "automounterhelper"    # Automounter Helper [https://www.pixeleyes.co.nz/automounter/helper/]
    cask "focusrite-control-2"  # Focusrite Control 2

    # MAIN FONTS (https://fonts.google.com/)
    #cask "font-ia-writer-duospace" # https://ia.net/topics/in-search-of-the-perfect-writing-font
    cask "font-anonymous-pro"      # https://www.marksimonson.com/fonts/view/anonymous-pro
    cask "font-bebas-neue"         # https://fonts.adobe.com/fonts/bebas-neue
    cask "font-courier-prime"      # https://quoteunquoteapps.com/courierprime/
    cask "font-fira-code"          # https://github.com/tonsky/FiraCode
    cask "font-ia-writer-duo"      # https://github.com/iaolo/iA-Fonts/
    cask "font-ia-writer-mono"     # https://github.com/iaolo/iA-Fonts/
    cask "font-ia-writer-quattro"  # https://ia.net/topics/a-typographic-christmas
    cask "font-inconsolata"        # https://levien.com/type/myfonts/inconsolata.html
    cask "font-input"              # https://input.djr.com/
    cask "font-intel-one-mono"     # https://github.com/intel/intel-one-mono/
    cask "font-iosevka"            # https://typeof.net/Iosevka/customizer
    cask "font-jetbrains-mono"     # https://www.jetbrains.com/lp/mono/
    cask "font-liberation"         # https://github.com/liberationfonts/liberation-fonts
    cask "font-red-hat-mono"       # https://github.com/RedHatOfficial/RedHatFont
    cask "font-tengwar-telcontar"
    cask "font-ubuntu-mono"        # https://design.ubuntu.com/font
    cask "font-victor-mono"        # https://github.com/rubjo/victor-mono
    cask "font-zed-mono"
    cask "font-zed-sans"

    # NERD FONTS (https://www.nerdfonts.com/)
    cask "font-dejavu-sans-mono-nerd-font"
    cask "font-inconsolata-go-nerd-font"
    cask "font-jetbrains-mono-nerd-font"
    cask "font-sf-mono-nerd-font-ligaturized"
    cask "font-zed-mono-nerd-font"
end # OS.mac?

###############################################################################
# Create aliases for Docker images and run them as native commands
#
# 'whalebrew install'
#
# Docs: https://github.com/whalebrew/whalebrew
#
###############################################################################

whalebrew "whalebrew/wget" if File.exist?("/opt/homebrew/bin/whalebrew")
