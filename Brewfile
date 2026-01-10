# Brewfile: Homebrew bundle file for macOS or Linux app install (with notes/source repos)
#
# Author: Alberth Matos (alberth@matos.cc)
# Original Author  : Chad Mayfield (chad@chadmayfield.com)
# Original Source: https://gist.github.com/chadmayfield/ada07e4e506d7acd577a665541a70c9b
# License : GPLv3
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

# fail if Homebrew is not installed, (or if it's not in $PATH)
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
# tap "homebrew/bundle"         # [https://github.com/Homebrew/homebrew-bundle]
# tap "homebrew/cask-fonts"     # [https://github.com/Homebrew/homebrew-cask-fonts]
# tap "homebrew/services"       # [https://github.com/Homebrew/homebrew-services]

# unnecessary taps in v4: https://brew.sh/2023/02/16/homebrew-4.0.0/
if ENV.fetch("HOMEBREW_VERSION") < '4.0.0'
  tap "homebrew/cask"         # [https://github.com/Homebrew/homebrew-cask]
  tap "homebrew/core"         # [https://github.com/Homebrew/homebrew-core]
end

# third-party taps
tap "anchore/grype"           # grype [https://github.com/anchore/grype]
tap "aquasecurity/trivy"      # trivy [https://github.com/aquasecurity/trivy]
tap "boz/repo"                # kail [https://github.com/boz/kail]
tap "buo/cask-upgrade"        # 'brew cu' [https://github.com/buo/homebrew-cask-upgrade]
tap "charmbracelet/tap"       # softserve/pop/vhs [https://github.com/charmbracelet/homebrew-tap]
tap "hacker1024/hacker1024"   # coretemp [https://github.com/hacker1024/coretemp]
tap "hauler-dev/homebrew-tap" # hauler [https://github.com/hauler-dev/hauler]
tap "itchyny/tap"             # bed [https://github.com/itchyny/bed]
tap "knqyf263/pet"            # pet [https://github.com/knqyf263/pet]
tap "localstack/tap"          # localstack [https://github.com/localstack/homebrew-tap]
# tap "natesales/repo" || true  # q [https://github.com/natesales/q]
tap "neilotoole/sq"           # sq [https://github.com/neilotoole/sq]
tap "slp/krunkit"             # krunkit [https://github.com/containers/krunkit]
tap "tinygo-org/tools"        # tinygo [https://tinygo.org/getting-started/install/macos/]
tap "wader/tap"               # fq [https://github.com/wader/fq]
#tap "martido/homebrew-graph"  # 'brew graph' [https://github.com/martido/homebrew-graph]
tap "dracula/install"         # dracula [https://github.com/dracula/homebrew-install]

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
brew "brew-cask-completion"  # fish completion for brew-cask [https://github.com/xyb/homebrew-cask-completion]
#brew "cakebrew"             # homebrew GUI [https://www.cakebrew.com/]
brew "mas"                   # mac app store cli [https://github.com/mas-cli/mas]
brew "whalebrew" if File.symlink?("/usr/local/bin/docker") # hombrew for docker [https://github.com/whalebrew/whalebrew]

### ai/ml development, training, prompt-tuning
# brew "huggingface-cli", link: :overwrite # huggingface-cli [https://huggingface.co/docs/huggingface_hub/main/en/guides/cli]
# brew "llama.cpp"             # ramallama deps [https://github.com/ggerganov/llama.cpp]
# brew "mlx"                   # mlx for Apple silicon [https://github.com/ml-explore/mlx]
# brew "jupyterlab"            # ide for code/notebooks [https://github.com/jupyterlab/jupyterlab]
# brew "ollama"                # meta llm [https://github.com/ollama/ollama]

### ansible
brew "ansible", link: :overwrite # config mgmt [https://www.ansible.com]
brew "ansible-lint", link: :overwrite # check best practices/behavior [https://ansible-lint.readthedocs.io/]

### backup utilities
brew "tarsnap" if OS.linux?  # online backups for the paranoid [https://www.tarsnap.com/]

### cloud
# brew "awscli", link: :overwrite # aws cli client [https://aws.amazon.com/cli/]
# brew "azure-cli"             # ms azure cli client [https://docs.microsoft.com/cli/azure/overview]
# brew "doctl"                 # digitalocean cli client [https://github.com/digitalocean/doctl]
# brew "faas-cli"              # cli for openfaas [https://github.com/openfaas/faas-cli]
# brew "localstack-cli"        # local aws stack [https://github.com/localstack/localstack]
# brew "s3cmd"                 # s3 bucjets [https://github.com/s3tools/s3cmd]
# brew "vultr"                 # vultr cli client [https://jamesclonk.github.io/vultr]

### communications
brew "bandwhich"             # bandwidth utilization tool [https://github.com/imsnif/bandwhich]
brew "curl"                  # cli data xfers [https://curl.se]
brew "curlie"                # power of curl, ease of httpie [https://github.com/rs/curlie]
brew "doggo"                 # cli dns client in go [https://github.com/mr-karan/doggo]
brew "hey"                   # http load generator [https://github.com/rakyll/hey]
brew "httpie"                # human-friendly http cli client [https://github.com/httpie/httpie] https://httpie.io/docs/cli
brew "httpstat"              # curl statistics made simple [https://github.com/reorx/httpstat]
brew "iftop"                 # interface b/w stats [https://github.com/soarpenguin/iftop]
brew "ipinfo-cli"            # cli ip info from ipinfo.io [https://github.com/ipinfo/cli]
brew "k6"                    # k6 loadtest util (https://github.com/grafana?q=k6) https://k6.io
# brew "mailpit"               # email/SMTP testing tool [https://github.com/axllent/mailpit]
# brew "mosh"                  # the mobile shell [https://mosh.org/]
brew "mtr"                   # network diagnostics [https://github.com/traviscross/mtr]
# brew "natesales/repo/q"      # a tiny dns client [https://github.com/natesales/q]
brew "netcat"                # network util [https://netcat.sourceforge.net/]
brew "plow"                  # high-performance http benchmarking [https://github.com/six-ddc/plow]
# brew "pop"                   # send email from your terminal (https://github.com/charmbracelet/pop)
brew "speedtest-cli", link: true # speedtest.net cli bandwidth test [https://github.com/sivel/speedtest-cli]
brew "vnstat"                # network traffic monitor [https://github.com/vergoh/vnstat]
brew "wget"                  # internet file retreiver [https://www.gnu.org/software/wget/]
# brew "yt-dlp", link: :overwrite # download all the videos [https://github.com/yt-dlp/yt-dlp]
brew "zrok"                  # resource sharing platform, vis-à-vis ngrok [https://zrok.io/]

### containers/kubernetes
# brew "buildkit"              # buildkit dep for copa [https://github.com/moby/buildkit]
# brew "copa"                  # cli container patching [https://github.com/project-copacetic/copacetic]
# brew "hauler"                # airgap swiss army knife [https://docs.hauler.dev/]
# brew "krunkit"               # launch vms [https://github.com/containers/krunkit]
# brew "oras"                  # oci registry client [https://oras.land/]
# brew "podman"                # manage containers without docker [https://podman.io/]
# brew "podman-compose"        # run docker-compose.yaml with podman [https://github.com/containers/podman-compose]
# brew "podman-tui"            # podman terminal ui [https://github.com/containers/podman-tui]
# brew "trivy"                 # kubernetes vuln/misconfiguration scanner [https://github.com/aquasecurity/trivy]

### development tools
# build
brew "act"                   # run GH actions locally [https://github.com/nektos/actions]
#brew "bazel"                # google build tool [https://bazel.build/]
brew "circleci"              # local circleci env [https://circleci.com/docs/local-cli/]
brew "drone-cli"             # cli drone.io client [https://drone.io]
brew "make"                  # direct complication [https://www.gnu.org/software/make/]
#brew "travis"                # cli travis-ci client [https://github.com/travis-ci/travis.rb/]
# diff tools
# brew "colordiff"             # colored diff [https://www.colordiff.org/]
brew "diff-so-fancy"         # good-lookin' diffs [https://github.com/so-fancy/diff-so-fancy]
# brew "difftastic"            # structural diff [https://github.com/Wilfred/difftastic]
# brew "git-delta"             # syntax-highlighting pager [https://github.com/dandavison/delta]
# docs
brew "glow"                  # render markdown in terminal [https://github.com/charmbracelet/glow]
if OS.mac?
  brew "hugo"                # static site generator [https://gohugo.io/]
  brew "mkdocs", link: true  # project docs in markdown [https://www.mkdocs.org/]
  brew "asciidoctor"         # textfile formatter [https://asciidoc.org/]
end
# editors
brew "bed"                   # binary editor (in Go) [https://github.com/itchyny/bed]
brew "hexyl"                 # simple cli hex viewer (in Rust) [https://github.com/sharkdp/hexyl]
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
brew "gh" if Dir.exist?("~/src") # github cli [https://github.com/cli/cli]
brew "git"                   # distributed revision control [https://git-scm.com]
brew "git-cal"               # git contribs calendar for terminal [https://github.com/k4rthik/git-cal]
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
# brew "tinygo"                # https://tinygo.org/
brew "upx"                   # https://upx.github.io/
# java
brew "groovy"                # [https://www.groovy-lang.org/]
brew "gradle"                # [https://www.gradle.org/]
brew "openjdk"
# python (should always be installed)
if OS.mac?
  brew "micropython"         # python for microcontrollers [https://www.micropython.org/]
  brew "pyenv", link: false  # simple python version mgmt [https://github.com/pyenv/pyenv]
  # brew "pipenv", link: false # create virtenvs for projects [https://docs.pipenv.org/]
  brew "pipx"                # exec bins from Python pkgs in isolated envs [https://pipx.pypa.io]
  brew "pyenv-virtualenv", link: false
  brew "python", link: false
end
# rpm build tools
brew "create-dmg"            # build fancy DMGs [https://github.com/create-dmg/create-dmg]
if !Hardware::CPU.arm?
  brew "rpm", link: :overwrite # 4.19+ now only bottled for x86_64 (>4.18.1 ventura required)
  brew "rpm2cpio", link: :overwrite
end
# rust
brew "rust"                  # [https://www.rust-lang.org/]
brew "rustup"                # [https://rust-lang.github.io/rustup/]

### kubernetes tools
brew "stern"                 # multi-pod tail [https://github.com/stern/stern]

### linux utilities (https://bit.ly/2KLMXDp)
# archive utls
brew "cabextract"            # extract files from ms cabinet files [https://www.cabextract.org.uk/]
brew "p7zip"                 # posix port of 7-zip [https://p7zip.sourceforge.net/]
brew "unar"                  # the unarchiver [https://theunarchiver.com/command-line]
brew "xz"                    # lossless compression, improved LZMA [https://tukaani.org/xz/]
# editors
brew "helix"                 # post-modern modal text editor [https://github.com/helix-editor/helix]
brew "micro"                 # modern terminal-based text editor [https://micro-editor.github.io/]
brew "neovim" if OS.mac?     # extensible vim-fork [https://neovim.io/]
brew "rmate", link: :overwrite # HOWTO: https://github.com/textmate/rmate
brew "vim" if OS.linux?
# file/filesystem
brew "ast-grep"              # structural search, lint, rewriting [https://github.com/ast-grep/ast-grep]
brew "bat"                   # cat with syntax highlighting [https://github.com/sharkdp/bat]
brew "cloc"                  # count lines of code [https://github.com/AlDanial/cloc/]
brew "diskonaut"             # terminal visual disk space [https://github.com/imsnif/diskonaut]
brew "diskus"                # fast alt to 'du -sh' [https://github.com/sharkdp/diskus]
brew "duf"                   # a better df [https://github.com/muesli/duf]
brew "dua-cli"               # view disk space tui (in rust) [https://github.com/Byron/dua-cli]
#brew "exa"                   # modern ls [https://github.com/ogham/exa/issues/1243] UNMAINTAINED UPSTREAM!
brew "eza", link: :overwrite # modern ls (https://eza.rocks/) [https://github.com/eza-community/eza]
brew "fd"                    # find clone [https://github.com/sharkdp/fd]
brew "gdu"                   # view disk space tui (in go) [https://github.com/dundee/gdu]
brew "lsd"                   # next gen ls [https://github.com/lsd-rs/lsd]
brew "ripgrep"               # blazing fast grep (https://github.com/BurntSushi/ripgrep)
brew "scc"                   # fast code counter [https://github.com/boyter/scc]
brew "tree"
brew "tre-command"           # tree, improved [https://github.com/dduan/tre]
brew "walk"                  # terminal file manager [https://github.com/antonmedv/walk]
brew "yazi"                  # fast file manager [https://github.com/sxyazi/yazi]
brew "zoxide"                # a smarter cd command [https://github.com/ajeetdsouza/zoxide]
# hardware
brew "lsusb"
brew "smartmontools"
# misc
brew "chezmoi"               # securely sync dotfiles (https://www.chezmoi.io/) [https://github.com/twpayne/chezmoi]
brew "coreutils"             # chown/chmod, du, cut, uniq, shred, etc.
brew "dos2unix"
# brew "mackup"                # sync mac/linux app settings [https://github.com/lra/mackup]
brew "moreutils"             # sponge, pee, parallel, etc.
brew "pv"                    # pipe viewer, monitor data through pipe [http://www.ivarch.com/programs/pv.shtml]
brew "tldr"                  # user-friendly man [https://tldr.sh/]
brew "viddy"                 # a modern watch replacement [https://github.com/sachaos/viddy]
brew "watch"
# multiplexers
brew "tmux"                  # https://github.com/tmux/tmux/wiki/Getting-Started (https://tmuxcheatsheet.com/)
#brew "zellij"                # terminal workspaces [https://github.com/zellij-org/zellij]
# processes/resource mgmt
brew "btop"                  # resource monitor [https://github.com/aristocratos/btop]
#brew "bpytop"                # resource monitor [https://github.com/aristocratos/bpytop]
#brew "hacker1024/hacker1024/coretemp" # recommended for bpytop
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
# shell lint
brew "shellcheck"            # static analysis tool [https://github.com/koalaman/shellcheck]
brew "shfmt"                 # shell parser [https://github.com/mvdan/sh]
# shell recorders/share/snippets
brew "asciinema"             # record shell [https://asciinema.org/]
brew "knqyf263/pet/pet"      # cli snippet manager [https://github.com/knqyf263/pet]
brew "ttyd"                  # ttyd over web (required by vhs) [https://github.com/tsl0922/ttyd]
brew "vhs"                   # cli home video recorder [https://github.com/charmbracelet/vhs/]

### multimedia tools
#brew "spotify-tui"           # https://github.com/Rigellute/spotify-tui#connecting-to-spotifys-api
brew "ffmpeg"                # record/edit video [https://ffmpeg.org/]
brew "jhead"                 # extract EXIF data [https://github.com/Matthias-Wandel/jhead]
brew "exiftool"              # read/write EXIF data [https://exiftool.org]
brew "ghostscript"           # required for imagemagick
brew "imagemagick"#, args: ["with-webp"]

### network services
if OS.linux?
  brew "coredns", restart_service: true
  brew "sdns", restart_service: true
end

### security tools
#brew "afl-fuzz"              # DEPRECATED 9/17/22
brew "age"                   # simple, modern encryptionn tool [https://github.com/FiloSottile/age]
brew "aircrack-ng"           # next-gen auth cracker [https://aircrack-ng.org/]
brew "certbot" if OS.linux?  # formerly letsencrypt [https://certbot.eff.org/]
brew "ffuf"                  # fast web fuzzer (in Go) [https://github.com/ffuf/ffuf]
brew "fzf"                   # cli fuzzy finder (in Go) [https://github.com/junegunn/fzf]
brew "gnupg"
brew "grype"                 # container vuln scanner [https://github.com/anchore/grype]
brew "hydra"                 # net logon cracker [https://github.com/vanhauser-thc/thc-hydra]
brew "john"                  # password cracker [https://www.openwall.com/john/]
brew "nmap"                  # port scanner [https://nmap.org/]
brew "rustscan"              # a modern port scanner [https://github.com/RustScan/RustScan]
brew "socat"                 # bidirectional byte streams [http://www.dest-unreach.org/socat/]
brew "sslscan"               # sslscan test suite [https://github.com/rbsec/sslscan]

### stats
brew "archey4"               # system info [https://github.com/HorlogeSkynet/archey4]
brew "gtop"                  # system monitor [https://github.com/aksakalli/gtop]
brew "hyperfine"             # cli benchmarking tool [https://github.com/sharkdp/hyperfine]
# brew "node_exporter", restart_service: true

### virtualization tools
if OS.mac?
  brew "colima"              # container runtimes [https://github.com/abiosoft/colima]
  brew "docker"              # required by colima [https://www.docker.com/]
  brew "lima"                # linux virtual machines [https://github.com/lima-vm/lima] (https://lima-vm.io/docs/templates/)
end
# brew "packer"                # image builds [https://www.packer.io/]
# brew "terraform"             # automate infrastructure [https://www.terraform.io/]
# brew "xhyve" if OS.mac? && !Hardware::CPU.arm?

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
  # cask "anythingllm" # https://anythingllm.com/
  cask "chatgpt" # https://chatgpt.com/
  # cask "jan" # https://jan.ai/
  # cask "lm-studio" # https://lmstudio.ai
  # cask "miniconda" # install conda, req'd by unsloth.ai (if using CUDA) & mlx

  # BASE INSTALL
  # cask "aldente" # AlDente Pro
  # cask "balenaetcher"
  # cask "coconutbattery"
  # cask "daisydisk"
  # cask "dbngin" # https://dbngin.com/
  # cask "displays"
  # cask "expressions"
  # cask "firefox"#, greedy: true
  # cask "fork"
  # cask "garmin-express"
  # cask "goland"
  # cask "google-chrome"
  cask "handbrake-app"
  # cask "inkscape"
  # cask "intellij-idea"
  # cask "malwarebytes" # subscription exp-2/25
  # cask "micro-snitch"
  # cask "microsoft-auto-update"
  # cask "microsoft-excel"
  # cask "microsoft-powerpoint"
  # cask "microsoft-word"
  # cask "mitmproxy", link: :overwrite
  # cask "podman-desktop"
  # cask "postman"
  # cask "proton-drive"
  # cask "proton-mail"
  # cask "proton-pass"
  # cask "protonvpn"
  # cask "sqlpro-studio"
  # cask "textmate"
  # cask "the-unarchiver"
  # cask "transmit"
  # cask "versions"
  # cask "visual-studio-code"
  # cask "vlc"
  # cask "warp"
  cask "1password"
  cask "1password-cli"
  cask "betterdisplay"
  cask "bettermouse"
  cask "ghostty"
  cask "gpg-suite"
  cask "helium-browser"
  cask "hex-fiend"
  cask "iterm2"
  cask "itermai"
  cask "jetbrains-toolbox"
  cask "kaleidoscope"
  cask "keyboard-maestro"
  cask "linearmouse"
  cask "little-snitch"
  cask "mactex"
  cask "raycast"
  cask "setapp"
  cask "skim"
  cask "slack"
  cask "soundsource"
  cask "spamsieve"
  cask "windows-app"
  cask "yaak" # https://yaak.app/ - API client

  # if !Hardware::CPU.arm? # intel only as of 2/1/23
  #   cask "virtualbox"
  #   cask "virtualbox-extension-pack"
  # end

  # MY SPECIFIC VERSION CASK INSTALLS
  # Supposedly pinning isn't recommended since it causes issue with auto-update
  # (https://github.com/Homebrew/homebrew-cask/issues/49127). So these tasks will
  # install a "locked" version via the older ruby formulae. If the date check
  # evalutes to less than now, the Brewfile will abort with a message of which cask
  # to update with it's "last" formula file. This way I can lock the casks to
  # the versions for which I have licenses
  puts("===================================================================================")
  puts("Older casks must be downloaded and installed manually.")

  # # https://binarynights.com/
  # forklift_date = Date.new(2025,4,7)

  # if forklift_date < now
  #   abort("ERROR! Expiration is closing in, replace the URL with the latest forklift.rb.")
  #   download = URI.open('https://raw.githubusercontent.com/Homebrew/homebrew-cask/bcffca4b0a070995bd4b746f202ab341d05c45ad/Casks/f/forklift.rb')
  #   IO.copy_stream(download, './forklift.rb')
  #   puts("  - Downloaded formula: forklift.rb")
  # else
  #   cask "forklift"
  # end



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

  # OPTIONAL (install as needed)
  #cask "1password"
  #cask "1password-cli"
  #cask "appcleaner"
  #cask "burn"
  #cask "burp-suite"
  #cask "caffeine"
  #cask "datagrip"
  #cask "expandrive"
  #cask "fleet"
  #cask "ghidra"
  #cask "gimp"
  #cask "github"
  #cask "imazing"
  #cask "insomnia"
  #cask "intellij-idea-ce"
  #cask "jetbrains-gateway"
  #cask "jetbrains-space"
  #cask "keybase"
  #cask "ksdiff"
  #cask "lens"
  #cask "logseq"
  #cask "mattermost"
  #cask "microsoft-office" if !Dir.exist?("/Applications/Microsoft Word.app")
  #cask "microsoft-remote-desktop"
  #cask "ngrok"
  #cask "obs"
  #cask "protonmail-import-export"
  #cask "pycharm-ce"
  #cask "raspberry-pi-imager"
  #cask "rustrover"
  #cask "scribus"
  #cask "signal"
  #cask "slack"
  #cask "smartgit"
  #cask "spotify"
  #cask "steam"
  #cask "strongsync"
  #cask "terminus"
  #cask "thonny" # python ide for beginners [https://thonny.org/]
  #cask "tower"
  #cask "transmission"
  #cask "typora"
  #cask "vagrant"
  #cask "vagrant-manager"
  #cask "virtualbuddy" if !Hardware::CPU.arm?
  #cask "viscosity"
  #cask "vmware-fusion"
  #cask "vnc-viewer"
  #cask "wireshark"
  #cask "writerside"
  cask "adobe-creative-cloud"
  cask "airfoil"
  cask "alcove"
  cask "ammonite"
  cask "audio-hijack"
  cask "autodesk-fusion"
  cask "discord"
  cask "dracula-macos-color-picker"
  cask "dracula-terminal"
  cask "dracula-xcode"
  cask "dropbox"
  cask "elgato-capture-device-utility"
  cask "elgato-stream-deck"
  cask "elgato-studio"
  cask "fission"
  cask "makemkv"
  cask "microsoft-teams"
  cask "obs"
  cask "obsidian"
  cask "plistedit-pro"
  cask "powerphotos"
  cask "spotify"
  cask "steam"
  cask "syncthing-app"
  cask "textexpander"
  cask "tower"
  cask "whatsapp"
  cask "winbox"
  cask "zed"
  cask "zoom"

end

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
  mas "1Password for Safari", id: 1569813296
  mas "Auto HD FPS for YouTube", id: 1546729687
  mas "Broadcasts", id: 1469995354
  mas "Copilot", id: 1447330651
  mas "Dropover", id: 1355679052
  mas "Fantastical", id: 975937182
  mas "Goban", id: 646372172
  mas "Hyperduck", id: 6444667067
  mas "Ivory", id: 6444602274
  mas "John's Background Switcher", id: 907640277
  mas "Keynote", id: 409183694
  mas "LanguageTool", id: 1534275760
  mas "Microsoft Excel", id: 462058435
  mas "Microsoft PowerPoint", id: 462062816
  mas "Microsoft Word", id: 462054704
  mas "NepTunes", id: 1006739057
  mas "Numbers", id: 409203825
  mas "OneDrive", id: 823766827
  mas "Pages", id: 409201541
  mas "Photomator", id: 1444636541
  mas "Pixelmator Pro", id: 1289583905
  mas "Save to Raindrop.io", id: 1549370672
  mas "Sink It", id: 6449873635
  mas "Steam Link", id: 1246969117
  mas "Transmit", id: 1436522307
  mas "uBlock Origin Lite", id: 6745342698
  mas "Windows App", id: 1295203466
  mas "WireGuard", id: 1451685025
  mas "Xcode", id: 497799835
end

###############################################################################
# Create aliases for Docker images and run them as native commands
#
# 'whalebrew install'
#
# Docs: https://github.com/whalebrew/whalebrew
#
###############################################################################

whalebrew "whalebrew/wget" if File.exist?("/opt/homebrew/bin/whalebrew")

#################### TODO ####################
# * Fix flog tap and install it's not installing. Currently it fails with the following message;
#   - Error: flog: wrong number of arguments (given 1, expected 0)
#   - tap "mingrammer/flog"
#   - brew "flog" # fake log generator [https://github.com/mingrammer/flog]
# *
# *
# *
##############################################
