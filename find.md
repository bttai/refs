# Find Command Cheat Sheet


## 1. What is `find`?

`find` is a versatile command used to search for files and directories based on various criteria like name, size, permissions, etc.

## 2. Basic Syntax

```bash

find [path] [expression]
  ```

    [path]: Directory to start searching from (e.g., /, . for the current directory).
    [expression]: Conditions to filter results (e.g., name, size, permissions).

## 3. Searching by Name

- **Find files by name:**
  ```bash
  find /path/to/start -name "filename"
  ```
    

- **Case-insensitive search:**
  ```bash
  find /path/to/start -iname "filename"
  ```


- **Use wildcards:**
  ```bash
  find /path/to/start -name "*.txt"   # Find all .txt files
  ```


## 4. Searching by Type

- **Find files:**
  ```bash
  find /path/to/start -type f
  ```

- **Find directories:**
  ```bash
  find /path/to/start -type d
  ```


- **Find symbolic links:**
  ```bash
  find /path/to/start -type l
  ```


## 5. Searching by Size

- **Find files larger than 1GB:**
  ```bash
  find /path/to/start -size +1G
  ```
    

- **Find files smaller than 10MB:**
  ```bash
  find /path/to/start -size -10M
  ```

- **Find files exactly 1KB:**
  ```bash
  find /path/to/start -size 1k
  ```


## 6. Searching by Permissions

- **Find files with specific permissions:**
  ```bash
  find /path/to/start -perm 644
  ```
    

- **Find files writable by others:**
  ```bash
  find /path/to/start -perm -o=w
  ```

- **Find executable files:**
  ```bash
  find /path/to/start -perm /a=x
  ```


## 7. Searching by Time

- **Find files modified in the last 7 days:**
  ```bash
  find /path/to/start -mtime -7
  ```

- **Find files modified more than 30 days ago:**
  ```bash
  find /path/to/start -mtime +30
  ```


- **Find files accessed in the last 2 days:**
  ```bash
  find /path/to/start -atime -2
  ```


## 8. Executing Commands on Found Files

- **Display number of lines found files**
  ```bash
  find /path/to/start -name "*.tmp" -exec wc -l {} \;
  ```

- **Move found files:**
  ```bash
  find /path/to/start -name "*.log" -exec mv {} /path/to/destination/ \;
  ```


- **List details of found files:**
  ```bash
  find /path/to/start -type f -exec ls -l {} \;
  ```
 
## 9. Combining Conditions

- **Find files matching multiple criteria:**
  ```bash
  find /path/to/start -type f -name "*.sh" -size +1M
  ```

- **Use logical operators:**

- **AND (default):**
  ```bash
  find /path -type f -name "*.sh" -size +1M
  ```
    
- **OR:**
  ```bash
  find /path \( -name "*.sh" -o -name "*.py" \)
  ```

- **NOT:**
  ```bash
  find /path ! -name "*.txt"
  ```

## 10. Practical Examples

- **Find all empty files:**
  ```bash
  find /path/to/start -type f -empty
  ```
    
- **Find all files owned by a specific user:**
  ```bash
  find /path/to/start -user username
  ```

- **Find files changed in the last 24 hours and list them:**
  ```bash
  find /path/to/start -mtime -1 -exec ls -l {} \;
  ```

- **Find and compress .log files:**
  ```bash
  find /path/to/start -name "*.log" -exec gzip {} \;
  ```
