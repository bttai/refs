
# Bash Cheat Sheet

## Basics
- `pwd` - Print current working directory  
- `ls` - List files and directories  
- `cd <directory>` - Change directory  
- `touch <file>` - Create an empty file  
- `cp <source> <destination>` - Copy files  
- `mv <source> <destination>` - Move/rename files  
- `rm <file>` - Remove files  

## Permissions
- `chmod <mode> <file>` - Change file permissions  
- `chown <owner>:<group> <file>` - Change ownership  

## File Content
- `cat <file>` - View file content  
- `less <file>` - View file one screen at a time  
- `head <file>` - Show first 10 lines of a file  
- `tail <file>` - Show last 10 lines of a file  
- `grep "<pattern>" <file>` - Search for a pattern in a file  

## Processes
- `ps` - Display running processes  
- `top` - Interactive process viewer  
- `kill <PID>` - Terminate a process  

## Variables
- `$VARIABLE` - Access variable  
- `export VARIABLE=value` - Set environment variable  
- `unset VARIABLE` - Remove variable  

## Using Parameters in Scripts
1. **Accessing Script Parameters**:  
    - `$0` - Script name  
    - `$1, $2, ...` - Positional parameters passed to the script  
    - `$@` - All parameters  
    - `$#` - Number of parameters  

2. **Example Script**:  
   **`script.sh`**  
   ```bash
   #!/bin/bash
   echo "URL: $1"
   echo "Command to execute: $2"
   echo "Output:"
   eval $2
   ```  

   **Run**:  
   ```bash
   ./script.sh http://192.168.56.1 "ps -aux"
   ```  

   **Output**:  
   ```
   URL: http://192.168.56.1
   Command to execute: ps -aux
   Output:
   USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
   ...
   ```  

## Conditionals
```bash
if [ $1 -gt 10 ]; then
  echo "Greater than 10"
else
  echo "10 or less"
fi
```

## Loops

### For Loop
```bash
for i in {1..5}; do
  echo "Number: $i"
done
```

### While Loop
```bash
count=1
while [ $count -le 5 ]; do
  echo "Count: $count"
  ((count++))
done
```

## Functions
```bash
function my_function() {
  echo "Hello, $1!"
}
my_function "World"
```

## Useful Commands
- `find <dir> -name <pattern>` - Find files  
- `awk '{print $1}' file.txt` - Process text  
- `sed 's/old/new/g' file.txt` - Replace text  
