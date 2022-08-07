# How setup a Emoji Commitizen + Commitlint + Husky? 
This is a quick tutorial of how setup a emoji commitizen with commitlint to verified every commit using husky to make sure that everthing is fine!

It's important to enfase this is my personal configuration but everthing is really easy to customize :happyface: 
<br/>
<br/>

### **Official References**
Commitlint repo
 https://github.com/conventional-changelog/commitlint

Commitizen repo
 https://github.com/commitizen/cz-cli

Leo Roese (tutorial video about this setup with more details about husky)
https://youtu.be/jNxDNoYEGVU

<br/>

## Install

commitlint install npm command
```
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```
Make sure to run this echo command to create commitlint.config.js file 
```
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```
<br/>

To lint commits before they are created you can use Husky's commit-msg hook:
```
# Automatic Init
npx husky-init && npm install       # npm
# or
npx husky-init && yarn              # Yarn 1
```
Husky-init creates a ```pre-commit``` hook that you can edit in husky folder. Here we gonna delete him because by default it will run ```npm test``` (something desnecessary to us) and create a ```commit-msg``` hook

If automatic init ins't working, manual commands: 
```
# Install Husky v6
npm install husky --save-dev
# or
yarn add husky --dev

# Activate hooks
npx husky install
# or
yarn husky install
```

Creating ```commit-msg``` hook
```
npx husky add .husky/commit-msg 'echo "pre-received"
```

Change commit-msg config from husky folder to: 
```
#!/usr/bin/env sh
if ! head -1 "$1" | grep -qE "^. (feat|fix|WIP|ci|chore|docs|test|style|codestyle|refactor|dependency|perf|build|revert|mock|poop)(\(.+?\))?: .{1,}$"; then
    printf "\n\e[0;31mCommit abortado. A \e[1;34mTAG\e[0m\e[0;31m do seu commit está inválida.\n\n\e[0m" >&2
    exit 1
fi
if ! head -1 "$1" | grep -qE "^.{1,88}$"; then
    printf "\n\e[0;31mCommit abortado. A\e[1;34m MENSAGEM \e[0m\e[0;31mdo seu commit está muito longa \e[0m\e[1;34m(mais de 88 caracteres). \e[0m\n\n" >&2
    exit 1
fi
```