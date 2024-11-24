# `sed` Command Cheat Sheet

## Modify Specific Lines

- **Replace the content of line 2 with a new line**:
  ```bash
  sed -i "2s/.*/This is the new line/" file.txt
  ```

- **Insert a new line before line 3:**:
  ```bash
  sed -i '3i This is a new line' file.txt
  ```
- **append a line:**:
  ```bash
  sed -i "\\$a This is a new line" file.txt
  ```


##  Add or Remove Characters
- **Prepend a # to line 5:**:
  ```bash
  sed -i "5s/^/#/" file.txt
  ```
- **Remove # only at the start of line 5:**:
  ```bash
  sed -i "5s/^#//" file.txt
  ```

- **Remove all # at the beginning of lines in a file:**:
  ```bash
  sed -i "s/^#//" file.txt
  ```
- **Remove all # at the beginning of each line (handles spaces too):**:
  ```bash
  sed -i "s/^[[:space:]]*#\\+ //" file.txt
  ```
## Replace Patterns

- **Replace the first occurrence of 'foo' with 'bar' in the entire file:**:
  ```bash
  sed -i "s/foo/bar/" file.txt
  ```
- ****:
  ```bash
  
  ```
- **Replace all occurrences of 'foo' with 'bar' in the entire file:**:
  ```bash
  sed -i "s/foo/bar/g" file.txt
  ```
- **Replace only in lines containing 'pattern':**:
  ```bash
  sed -i "/pattern/s/foo/bar/" file.txt
  ```
## Replace Patterns

- **Delete a specific line (e.g., line 5):**:
  ```bash
  sed -i "5d" file.txt
  ```
- **Delete all empty lines:**:
  ```bash
  sed -i "/^$/d" file.txt
  ```

- **Delete lines containing a specific pattern (e.g., "ERROR"):**:
  ```bash
  sed -i "/ERROR/d" file.txt
  ```

## Display Lines


- **Display only lines 3 to 7 from a file:**:
  ```bash
  sed -n "3,7p" file.txt
  ```


## Search

- **Find lines from a text file that have between m and n characters:**
  ```bash
  sed -n '/^[^[:space:]]\{m,n\}$/p' file.txt
  ```


## Notes

    -i: Edits the file in place.
    s/old/new/: Substitutes old with new.
    /pattern/: Applies the command only to lines matching the pattern.
    p: Prints lines.
    d: Deletes lines.
    [[:space:]]: Matches spaces or tabs.
    ^: Matches the start of a line.
    $: Matches the end of a line. """

