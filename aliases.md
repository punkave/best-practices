# MONGO SHORTCUTS
alias mongostart='mongod --dbpath ~/data/db --storageEngine wiredTiger';
alias mongorepair='mongod --dbpath ~/data/db --repair';
alias mongostop='mongod --shutdown';
alias mongorepair_noindexfail='mongod --dbpath ~/data/db --repair --setParameter failIndexKeyTooLong=false';
alias mongostart_noindexfail='mongod --dbpath ~/data/db --setParameter failIndexKeyTooLong=false';
alias mongo_legacy='~/scripts/run-mongo-2-6';
alias mongo_latest='~/scripts/run-mongo';
alias run_mongo='function() { ~/scripts/run-mongo "$@"; }'

# P'UNK DEV ENV SHORTCUTS
alias go='npm start';
alias godev='npm run dev';
alias monitor='npm run monitor';
alias npm-refresh="rm -rf node_modules && npm install"

# GIT SHORTCUTS
alias gs="git status"
alias co="git checkout"
alias ga="git add -A"
alias cm="git commit -m"
alias po="git push origin"
alias fo="git fetch origin"
alias pl="git pull origin"
alias br="git branch"
alias gdeletemerged='git branch --merged | egrep -v "(^\*|master|develop)" | xargs git branch -d'
alias gwcdelete='f() { git branch | grep $1 | xargs git branch -D };f'
alias gpsuo='git push --set-upstream origin';
alias clonepunk='f() { cd ~/Documents/Sites && git clone git@github.com:punkave/$1.git && cd $1};f'

# GENERAL SHORTCUTS
alias sites="cd [PATH TO P'UNK SITES];"
alias goharp='harp server . --port 3000'
alias comp='harp compile src'
alias compclean='rm -rf src/www'
alias cc='rm -rf src/www'
alias checknodelinks='ls -l node_modules | grep ^l'
alias ohmyzsh="code ~/.oh-my-zsh"
alias profile="code ~/.zshrc"
alias snippets="code ~/scripts/code-snippets"
alias refresh="source ~/.zshrc"
alias home='cd ~'
alias hosts='code /etc/hosts'
alias kill3000="fuser -k -n tcp 3000"
alias fixcam="sudo killall AppleCameraAssistant;sudo killall VDCAssistant"
alias simpleserver="python -m SimpleHTTPServer 8000"
alias cdatom='f() { cd ~/Documents/Sites/$1 && atom .};f'
<!-- # Turn wifi network on or off -->
<!-- # techrepublic.com/article/pro-tip-manage-wi-fi-with-terminal-commands-on-os-x/ -->
alias nonet="networksetup -setairportpower en0 off"
alias gonet="networksetup -setairportpower en0 on"

# M1 CHIP SHORTCUTS
alias c="code-insiders"
alias rosetta="arch -x86_64 zsh"