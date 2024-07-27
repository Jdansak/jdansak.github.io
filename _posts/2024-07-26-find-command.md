I love the `find` command. Unfortunately, I can never remember any of the sytax when I'm troubleshooting on a WebEX bridgeline and everyone is laughing at my poor command line kun-fu, which is why I put together these notes for myself and I am putting it on my website just because hell why not. I'm not gonna reference any other blogs or claim any of this content to be my own because it is a combination of what I have found over the past years ripped out of my obsidian notebook. If somebody's reading this and thinking, hey, some of the stuff looks like it was ran through ChatGPT, it was. I really don't care because this is for me and not you.

### Basic `find` Commands

Here are some fundamental `find` commands to get you started:

```bash
find . -name foo.txt                     # search under the current dir
find . -name "foo.*"                     # wildcard for extensions 
find . -name "*.txt"                     # wildcard for names
find . -type f                           # regular files
find . -type d    -name foo*             # directories
find /users/al -name Cookbook -type d    # search '/users/al' for dir named..
find . -type l, f   -name foo            # symbolic links and files
find /dev -type c                        # char devices, think consoles and serial
find /dev -type b                        # block devices , storage
find . -type p                           # named pipes
find . -type s                           # sockets

```


- **find / -name foo.txt**: Omitting the `-type f` flag makes `find` look for files by default.
- **find . -name foo.txt**: Searches for `foo.txt` in the current directory (`.`).
- **find . -name "foo.*"**: Uses a wildcard to search for any file starting with `foo.` in the current directory.
- **find . -name "*.txt"**: Uses a wildcard to search for all `.txt` files in the current directory.
- **find /users/al -name Cookbook -type d**: Searches for a directory named `Cookbook` in `/users/al`.

### Search Multiple Directories

You can search in multiple directories simultaneously:

```bash
find /opt /usr /var -name foo.scala -type f     # search multiple dirs
```

This command searches for `foo.scala` in `/opt`, `/usr`, and `/var` directories.

### Case-Insensitive Searching

To perform a case-insensitive search, use the `-iname` flag:

```bash
find . -iname foo                  # find foo, Foo, FOo, FOO, etc.
find . -iname foo -type d          # same thing, but only dirs
find . -iname foo -type f          # same thing, but only files
```

- **find . -iname foo**: Searches for `foo` in a case-insensitive manner.
- **find . -iname foo -type d**: Searches for directories named `foo` in a case-insensitive manner.
- **find . -iname foo -type f**: Searches for files named `foo` in a case-insensitive manner.

### Find Files with Different Extensions

You can search for files with different extensions using the `-name` flag combined with `-o` (logical OR):

```bash 
find . -type f \( -name "*.c" -o -name "*.sh" \)                       # *.c and *.sh files
find . -type f \( -name "*cache" -o -name "*xml" -o -name "*html" \)   # three patterns
```

- **find . -type f \( -name "*.c" -o -name "*.sh" \)**: Searches for files with `.c` or `.sh` extensions.
- **find . -type f \( -name "*cache" -o -name "*xml" -o -name "*html" \)**: Searches for files with extensions matching `cache`, `xml`, or `html`.

### Combining `find` with `grep`

The `grep` command is a powerful utility for searching text using patterns. You can combine `find` and `grep` to search for specific content within files:

```bash
find . -type f -name "*.txt" -exec grep "search_term" {} \;       # search_term in all .txt files
find /var/log -type f -name "*.log" -exec grep -i "error" {} \;   # case-insensitive search for "error" in all .log files
```

- **find . -type f -name "*.txt" -exec grep "search_term" {} \;**: Searches for `search_term` within all `.txt` files in the current directory. The `{}` is a placeholder for each file found, and `\;` ends the `-exec` command.
- **find /var/log -type f -name "*.log" -exec grep -i "error" {} \;**: Performs a case-insensitive search for the word `error` within all `.log` files in `/var/log`.

#### Advanced `grep` Usage with `find`

To refine your searches even further, you can use additional `grep` options:

```bash
find . -type f -name "*.conf" -exec grep -l "parameter" {} \;  # list files containing "parameter"
find /etc -type f -name "*.conf" -exec grep -H "parameter" {} \;  # show filenames with matches
find /var/log -type f -name "*.log" -exec grep -A 3 "ERROR" {} \;  # show 3 lines after match
find /home/user -type f -name "*.txt" -exec grep -B 2 "TODO" {} \;  # show 2 lines before match
find /var/log -type f -name "*.log" -exec grep -C 2 "WARNING" {} \;  # show 2 lines around match
```

- **find . -type f -name "*.conf" -exec grep -l "parameter" {} \;**: Lists files containing the term `parameter`.
- **find /etc -type f -name "*.conf" -exec grep -H "parameter" {} \;**: Shows filenames with matches for `parameter`.
- **find /var/log -type f -name "*.log" -exec grep -A 3 "ERROR" {} \;**: Displays 3 lines after each match of `ERROR`.
- **find /home/user -type f -name "*.txt" -exec grep -B 2 "TODO" {} \;**: Displays 2 lines before each match of `TODO`.
- **find /var/log -type f -name "*.log" -exec grep -C 2 "WARNING" {} \;**: Displays 2 lines around each match of `WARNING`.

### Finding Recently Modified Files

You can use `find` to locate files modified within a certain time frame:

```bash
find . -type f -mtime -1       # files modified in the last 24 hours
find . -type f -mtime -7       # files modified in the last 7 days
find . -type f -mmin -60       # files modified in the last 60 minutes
```

- **find . -type f -mtime -1**: Finds files modified in the last 24 hours.
- **find . -type f -mtime -7**: Finds files modified in the last 7 days.
- **find . -type f -mmin -60**: Finds files modified in the last 60 minutes.

### Finding Large Files

To locate large files, use the `-size` flag:

``` bash
find / -type f -size +100M     # files larger than 100MB
find . -type f -size +1G       # files larger than 1GB
```

- **find / -type f -size +100M**: Finds files larger than 100MB starting from the root directory.
- **find . -type f -size +1G**: Finds files larger than 1GB in the current directory.

  ### TODO:
  Need to add some more complex commands examples to chain together find with other tools like grep to find stuff like passwords and API keys other little goodies
