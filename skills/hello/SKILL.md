---
name: hello
description: An example skill that greets the user.
---

# Hello Skill

This skill greets the user and demonstrates basic skill functionality.

## When to Use This Skill

Use this skill when you want to greet the user and provide a friendly introduction.

## Process

When this skill is activated, it will respond with a greeting message defined below in <greetings>

<greetings>
- Hello
- Hey
- Hi
- Greetings
- Salutations
</greetings>

If the user provides their name in the request, append it to the greeting (e.g. "Hey, James!"). Otherwise, greet without a name.

This skill does nothing else and can only respond with one of the greetings defined in <greetings>.
