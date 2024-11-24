# `awk` Command Cheat Sheet

## Print Specific Columns

- **Print the first column from a file**:
  ```bash
  awk '{print $1}' file.txt
  ```

- **Print the second and third columns from a file:**:
  ```bash
  awk '{print $2, $3}' file.txt

  ```
- **Print the first and last column from a file:**:
  ```bash
  awk '{print $1, $NF}' file.txt
  ```
    
## Print Lines Based on Conditions

- **Print lines where the first column is greater than 50:**:
  ```bash
  awk '$1 > 50' file.txt
  ```
  
- **Print lines where the second column equals "apple":**:
  ```bash
  awk '$2 == "apple"' file.txt

  ```

- **Print lines where the third column contains "error":**:
  ```bash
  awk '$3 ~ /error/' file.txt
  ```


## Field Separator
   
- **Use a custom delimiter (e.g., comma) as the field separator**:
  ```bash
    awk -F ',' '{print $1, $2}' file.txt
  ```
  

- **Change field separator to space**
  ```bash
  awk -F ' ' '{print $1, $2}' file.txt
  ```


## Pattern Matching

- **Print lines where "error" is found anywhere in the line:**
  ```bash
  awk '/error/' file.txt
  ```
    

- **Print lines where "error" is found in the first column:**
  ```bash
  awk '$1 ~ /error/' file.txt
  ```


- **Print lines where "error" is found in the second column:**
  ```bash
  awk '$2 ~ /error/' file.txt
  ```


## Perform Operations

- **Sum values in the first column:**
  ```bash
  awk '{sum += $1} END {print sum}' file.txt

  ```

- **Find the average of values in the first column:**
  ```bash
  awk '{sum += $1; count++} END {print sum/count}' file.txt
  ```


- **Count lines in a file:**
  ```bash
  awk 'END {print NR}' file.txt
  ```

## Print Line Numbers
 
 - **Print lines along with their line number:**
  ```bash
    awk '{print NR, $0}' file.txt 
  ```

    
- **Print line number and the first column:**
  ```bash
  awk '{print NR, $1}' file.txt
  ```

## Multiple Actions

- **Print the first column and add a string after it:**
  ```bash
  awk '{print $1, "is the first column"}' file.txt
  ```
    

- **Print the first and second columns with specific formatting:**
  ```bash
  awk '{printf "First: %-10s Second: %-10s\n", $1, $2}' file.txt
  ```


- **Filter out lines starting with a semicolon (;), including those with leading spaces or tabs, and then remove leading spaces from the remaining lines**
  ```bash
  awk '!/^[[:space:]]*;/ {print NR, $0}' /var/backups/app.ini.bak
  ```

- **Find lines from a text file that have between m and n characters:**
  ```bash
  awk 'length($0) >= m && length($0) <= n && $0 !~ /[[:space:]]/' file.txt
  ```

## Redirect Output

- **Redirect output to another file:**
  ```bash
  awk '{print $1}' file.txt > output.txt
  ```
    

- **Append output to an existing file:**
  ```bash
  awk '{print $1}' file.txt >> output.txt
  ```

##  Notes

    -F: Defines the field separator.
    NR: Line number of the current record.
    $1, $2, ...: Column values (1st, 2nd, etc.).
    $NF: The last column in the current line.
    ~: Matches a regular expression.
    END: Executes after reading all lines.
    sum: A variable to store the sum of values.
    count: A variable to count the number of records. """



    Would you like me to suggest more specific additions or help convert this into a downloadable .md file for your convenience?