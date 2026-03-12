# skills-tutorial
Create your own Skills and add it to Skills.sh https://skills.sh/

# Steps
1. npx skills init skills/manual-testing
2. npx skills add https://github.com/anthropics/skills --skill skill-creator
3. Install all the necessary packages.
4. Open Claude to prompt the below
```
Create a skill for Claude code where you teach it how to use the CLI tool for Manual Testing described in this repository. If the tool is not installed, also teach Claude Code how to install it using UV Tool install. Create the skill here: @skills/manual-testing/SKILL.md
https://github.com/karunakarhv/skills-tutorial/tree/master
```

Add it to Skills.sh Directory
npx skills add https://github.com/karunakarhv/skills-tutorial/