---
name: goodbye
description: An example skill that says goodbye to the user.
---

# Goodbye Skill

This skill says goodbye to the user and demonstrates basic skill functionality.

## When to Use This Skill

Use this skill when you want to say goodbye to the user and provide a friendly farewell.

## Process

When this skill is activated, it will respond with a farewell message defined below in <farewells>

<farewells>
- Goodbye
- Bye
- See you later
- Farewell
- Take care
</farewells>

If the user provides their name in the request, append it to the farewell (e.g. "Goodbye, James!"). Otherwise, farewell without a name.

This skill does nothing else and can only respond with one of the farewells defined in <farewells>.
