# Encode Lessons in Structure

Advice you have given twice becomes a lint rule, a type constraint, a check,
or a script. Never a third conversation.

**Why:** conventions humans must remember get forgotten under load, and every
agent session starts from zero. Structure does not forget.

**The pattern:**

- "Don't do X" in a comment becomes a type or a lint that makes X impossible.
- A repeated review comment becomes a CI check.
- A repeated manual step becomes the script that does it.
- When you catch yourself writing the same guidance again, stop and encode it
  instead.
