# Get_Next_Line Project

## Overview

**Get_Next_Line** is a C function that efficiently reads lines from a file descriptor one line at a time. This project demonstrates advanced file I/O handling, memory management, and the use of static variables in C programming.

> 🏫 42 School  
> 💻 Language: C

---

## Key Features

- Reads lines from any file descriptor (files, stdin, pipes, etc.)
- Handles buffer sizes from 1 byte to extremely large values
- Efficient memory management with no leaks
- Includes both mandatory and bonus implementations
- Configurable `BUFFER_SIZE` at compile time
- Bonus version handles multiple file descriptors simultaneously

---

## Functions

### Core Functions

#### `char *get_next_line(int fd)`

- Reads next line from file descriptor
- Returns line including `\n` (except at EOF)
- Returns `NULL` at EOF or on error

#### `char *ft_readfile(int fd, char *str)`

- Reads file into buffer using `read()` syscall
- Appends to static buffer until `\n` or EOF
- Returns updated static buffer

#### `char *ft_readline(char *fullstr)`

- Extracts next line from static buffer
- Returns allocated string containing the line

#### `char *ft_movestr(char *str)`

- Removes processed line from static buffer
- Returns remaining content for next read

### Utility Functions

- `ft_strlen`: Safe string length check
- `ft_strchr`: Finds character in string
- `ft_strjoin`: Safely concatenates strings
- `ft_substr`: Creates substring (unused in final version)

---

## How It Works

- **Static Buffer**: Preserves unprocessed content between function calls  
- **Buffered Reading**: Reads in chunks defined by `BUFFER_SIZE`  
- **Line Extraction**: Processes buffer until `\n` is found  
- **Memory Efficiency**: Only reads what's needed for next line  
- **Error Handling**: Safely handles file errors and memory issues

### Example Implementation

```c
char *get_next_line(int fd)
{
    char        *tmp;
    static char *str;
    
    if (fd < 0 || BUFFER_SIZE <= 0)
        return (NULL);
    str = ft_readfile(fd, str);
    if (!str)
        return (NULL);
    tmp = ft_readline(str);
    str = ft_movestr(str);
    if (!*tmp)
    {
        free(tmp);
        return (NULL);
    }
    return (tmp);
}
```

---

## Compilation

```bash
# Mandatory version
cc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line.c get_next_line_utils.c main.c

# Bonus version (handles multiple FDs)
cc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line_bonus.c get_next_line_utils_bonus.c main.c
```

---

## Usage Example

```c
#include "get_next_line.h"
#include <fcntl.h>
#include <stdio.h>

int main()
{
    int fd = open("file.txt", O_RDONLY);
    char *line;
    
    while ((line = get_next_line(fd)))
    {
        printf("%s", line);
        free(line);
    }
    close(fd);
    return 0;
}
```

---

## Learning Outcomes

- **Static Variables**: Learned to preserve state between function calls
- **File I/O**: Mastered low-level file reading with `read()`
- **Memory Management**: Implemented safe memory allocation and freeing
- **Edge Cases**: Handled special cases (empty files, no newline at EOF)
- **System Programming**: Gained experience with file descriptors
- **Optimization**: Learned to minimize read operations and memory usage

---

## Key Insights

- **Buffer Size Flexibility**: Works efficiently with any `BUFFER_SIZE`
- **Minimal Reads**: Reads only what's necessary for next line
- **Memory Safety**: Properly frees all allocated memory
- **No Leaks**: Passes rigorous memory leak checks
- **Binary Safety**: Avoids processing binary files (undefined behavior per spec)

---

## Bonus Implementation

The bonus version enhances the function to:

- Use only one static variable (array of strings)
- Handle multiple file descriptors simultaneously
- Maintain separate buffers for each FD
- Process interleaved reads from different FDs

### Bonus Example

```c
// Bonus version handles multiple file descriptors
char *get_next_line(int fd)
{
    static char *str[1024];
    // ... same logic using str[fd] ...
}
```

---

## Best Practices

- Test with various `BUFFER_SIZE` values (1, 42, 4096, 10000000)
- Validate with different file types (text, empty, large, no newline at EOF)
- Check memory leaks with Valgrind
- Test with standard input (`STDIN_FILENO`)
- Verify interleaved FD handling in bonus version

---

## Project Structure

```
get_next_line/
├── get_next_line.c             # Main mandatory implementation
├── get_next_line.h             # Header file (mandatory)
├── get_next_line_utils.c       # Helper functions (mandatory)
├── get_next_line_bonus.c       # Bonus implementation
├── get_next_line_bonus.h       # Header for bonus
└── get_next_line_utils_bonus.c # Helper functions for bonus
```

---

## Final Thoughts

This project demonstrates professional-grade C programming techniques suitable for:

- Systems programming  
- File processing utilities  
- Embedded systems development
