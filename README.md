# How setup a Emoji Commitizen + Commitlint + Husky? 
This is a quick tutorial of how setup a emoji commitizen with commitlint to verified every commit using husky to make sure that everthing is fine!

It's important to enfase this is my personal configuration but everthing is really easy to customize :happyface: 
<br/>
<br/>

### **Official References**
Conventional Commits
https://www.conventionalcommits.org/en/v1.0.0/

Commitlint repo
 https://github.com/conventional-changelog/commitlint

Commitizen repo
 https://github.com/commitizen/cz-cli

Leo Roese (tutorial video about this setup with more details about husky)
https://youtu.be/jNxDNoYEGVU

Adapter of emojis in conventional
https://www.npmjs.com/package/cz-emoji-conventional

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
# Install Husky
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
if ! head -1 "$1" | grep -qE "^. (feat|fix|ci|chore|docs|test|style|refactor|perf|build|revert)(\(.+?\))?: .{1,}$"; then
    printf "\n\e[0;31mCommit aborted. The \e[1;34mTAG\e[0m\e[0;31m is an invalid type.\n\n\e[0m" >&2
    exit 1
fi
if ! head -1 "$1" | grep -qE "^.{1,88}$"; then
    printf "\n\e[0;31mCommit aborted. The\e[1;34m MESSAGE \e[0m\e[0;31mdis too long \e[0m\e[1;34m(more than 88 characters). \e[0m\n\n" >&2
    exit 1
fi
```
install commitizen to show a menu on terminal
```
npm install -g commitizen
```
adding the adapter to emojis
```
commitizen init cz-emoji-conventional --save-dev --save-exact
# if you use yarn: 
commitizen init cz-emoji-conventional --yarn --dev --exact
```

After that, add this config to yout ```package.json```: 
```
    "config": {
        "commitizen": {
            path": "node_modules/cz-emoji-conventional"
        }
    },
```

That's it! Your done! Now your project have a pattern with Conventional Commits using emojis to make everthing easier to see 😎

**And remember:** less ```git commit```, more ```git cz``` 

## Customization 
If you want to change some emojis or add some types that fits on your project its just need change 2 files: ```package.json``` and ```commit-msg```

first lets add some custom emojis and types on commitizen to that show on menu!

When you change a type in ```package.json```, it need to create every single type you want to have. A little tip: the actual text who is gonna be write on commit is the parent, not the title, keep that in mind.

```
"config": {
    "commitizen": {
      "path": "node_modules/cz-emoji-conventional",
      "types": {
        "feat": {
          "description": "A new feature",
          "title": "Features",
          "emoji": "🌟"
        },
        "fix": {
            "description": "A bug fix",
            "title": "Bug Fixes",
            "emoji": "🐛"
        },
        "WIP": {
            "description": "Working in Progress",
            "title": "WIP",
            "emoji": "🚧"
        },
        "docs": {
            "description": "Documentation change",
            "title": "Documentation",
            "emoji": "📚"
        },
        "style": {
            "description": "Add or update the UI and style files",
            "title": "UI Styles",
            "emoji": "🎨"
        },
        "codestyle": {
            "description": "Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)",
            "title": "Code Styles",
            "emoji": "💎"
        },
        "refactor": {
            "description": "A code change that neither fixes a bug nor adds a feature",
            "title": "Code Refactoring",
            "emoji": "🔨"
        },
        "dependency": {
            "description": "Add a dependency.",
            "title": "Dependency",
            "emoji": "➕"
        },
        "perf": {
            "description": "A code change that improves performance",
            "title": "Performance Improvements",
            "emoji": "⚡️"
        },
        "test": {
            "description": "Adding missing tests or correcting existing tests",
            "title": "Tests",
            "emoji": "🧪"
        },
        "mock": {
            "description": "Mock things",
            "title": "Mocks",
            "emoji": "🤡"
        },
        "build": {
            "description": "Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)",
            "title": "Builds",
            "emoji": "🔨"
        },
        "ci": {
            "description": "Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)",
            "title": "Continuous Integrations",
            "emoji": "📦"
        },
        "chore": {
            "description": "Other changes that don't modify src or test files",
            "title": "Chores",
            "emoji": "🧹"
        },
        "revert": {
            "description": "Reverts a previous commit",
            "title": "Reverts",
            "emoji": "⏪️"
        },
        "poop": {
            "description": "Bad code that needs to be improved",
            "title": "Poop",
            "emoji": "💩"
        }
      }
    }
  }
  ```
Here it's the pattern that I like and use. You are free to change everything in your own way. To get some emojis to use, https://gitmoji.dev have a really good selection with some description pre-made.


After that, it's need update the ```commit-msg``` of husky config. Make sure that every tag you added on previus step is here. Sometimes this config show a little bug with some emojis (you can predict the error if, on commitizen menu, the emoji show misaligned with text, in this times I change the emoji because I don't have any ideia off what's maybe going on in this part) .

```
#!/usr/bin/env sh
if ! head -1 "$1" | grep -qE "^. (feat|fix|WIP|ci|chore|docs|test|style|codestyle|refactor|dependency|perf|build|revert|mock|poop)(\(.+?\))?: .{1,}$"; then
    printf "\n\e[0;31mCommit aborted. The \e[1;34mTAG\e[0m\e[0;31m is an invalid type.\n\n\e[0m" >&2
    exit 1
fi
if ! head -1 "$1" | grep -qE "^.{1,88}$"; then
    printf "\n\e[0;31mCommit aborted. The\e[1;34m MESSAGE \e[0m\e[0;31mdis too long \e[0m\e[1;34m(more than 88 characters). \e[0m\n\n" >&2
    exit 1
fi
```