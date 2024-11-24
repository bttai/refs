
# Regular Expressions (Regex) Cheat Sheet

## Basic Syntax
| Pattern         | Description                                 |
|-----------------|---------------------------------------------|
| `.`             | Matches any single character except newline |
| `^`             | Matches the start of a string               |
| `$`             | Matches the end of a string                 |
| `*`             | Matches 0 or more repetitions of the preceding character |
| `+`             | Matches 1 or more repetitions of the preceding character |
| `?`             | Matches 0 or 1 repetition of the preceding character |
| `{n}`           | Matches exactly `n` occurrences            |
| `{n,}`          | Matches `n` or more occurrences            |
| `{n,m}`         | Matches between `n` and `m` occurrences    |
| `\`            | Escapes a special character (e.g., `\.` matches a literal `.`) |

## Character Classes
| Pattern         | Description                                 |
|-----------------|---------------------------------------------|
| `[abc]`         | Matches any one of `a`, `b`, or `c`         |
| `[^abc]`        | Matches any character except `a`, `b`, or `c` |
| `[a-z]`         | Matches any lowercase letter                |
| `[A-Z]`         | Matches any uppercase letter                |
| `[0-9]`         | Matches any digit                           |
| `[[:alpha:]]`   | Matches any alphabetic character            |
| `[[:digit:]]`   | Matches any digit                           |
| `[[:space:]]`   | Matches any whitespace character            |
| `[[:alnum:]]`   | Matches any alphanumeric character          |
| `[[:punct:]]`   | Matches any punctuation character           |

## Anchors
| Pattern         | Description                                 |
|-----------------|---------------------------------------------|
| `\b`           | Matches a word boundary                    |
| `\B`           | Matches a non-word boundary                |

## Groups and Alternation
| Pattern         | Description                                 |
|-----------------|---------------------------------------------|
| `(abc)`         | Captures the group `abc`                   |
| `(?:abc)`       | Matches `abc` without capturing (non-capturing group) |
| `a|b`           | Matches either `a` or `b`                  |

## Common Shorthand
| Pattern         | Description                                 |
|-----------------|---------------------------------------------|
| `\d`           | Matches any digit (`[0-9]`)                |
| `\D`           | Matches any non-digit                      |
| `\w`           | Matches any word character (`[a-zA-Z0-9_]`) |
| `\W`           | Matches any non-word character             |
| `\s`           | Matches any whitespace character           |
| `\S`           | Matches any non-whitespace character       |

## Useful Examples
| Pattern                         | Matches                                |
|---------------------------------|----------------------------------------|
| `^#`                            | Lines starting with `#`               |
| `^[[:space:]]*$`                | Empty or whitespace-only lines         |
| `[a-zA-Z]+`                     | Words with letters only               |
| `\b\d{3}-\d{2}-\d{4}\b`    | Social Security Number (e.g., 123-45-6789) |
| `https?://[\w.-]+`             | URLs starting with `http` or `https`  |

---
**Tip:** Use an online regex tester like [regex101.com](https://regex101.com) to experiment and test patterns!
