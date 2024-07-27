The `grep` command is an incredibly powerful tool in Unix-like operating systems for searching through text using patterns, known as regular expressions (regex). In this blog post, we'll explore how to use `grep` with regex to find specific types of data in files, such as MAC addresses, IP addresses, API keys, passwords, and other useful patterns.

## Basics of grep

The `grep` command searches for patterns within files and outputs the matching lines. The basic syntax of `grep` is:

```bash
grep [options] pattern [file...]
command1 output | grep [options] pattern | grep [options] pattern
```

### Basic grep Options
- `-i`: Ignore case
- `-r` or `-R`: Recursive search
- `-l`: List file names with matches
- `-n`: Show line numbers with matches
- `-c`: Show the count of matches
- `-A [num]`: Show [num] lines after match
- `-B [num]`: Show [num] lines before match
- `-C [num]`: Show [num] lines around match

## Finding MAC Addresses

A MAC address are in  all sorts of of files and logs. super helpful in tracking down devices on switchport or AP in the wild. The standard format is six groups of two hexadecimal digits, separated by colons or hyphens. Cisco formats them wrong and I will have to add that, but we aran't happy with TACS right now so that can wait. 


### Using `grep` to Find MAC Addresses

To search for MAC addresses in a file, you can use the following:

```bash
grep -E "([0-9A-Fa-f]{2}[:\-]){5}([0-9A-Fa-f]{2})" network_log.txt
```

## Finding IP Addresses

Bread and butter of log analys and troubleshooting. Also very helpful to figure out if you have to sanitize any data out of a log before sending (encrypted, of course..) to a vendor. 


### Finding IPv4 Addresses

To search for IPv4 addresses in a file, you can use the following  command:

```bash
grep -E "\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b" server_log.txt
```

### Finding IPv6 Addresses
``` bash
(9_9)
```
Yeah I haven't found or been able to come up with a good one  yet.

## Finding API Keys

API keys are used to authenticate requests associated with your project for usage and billing purposes. They are typically long alphanumeric strings, and they should be on your systems unvaulted. Basic they can be just just or powerful if not more then passwords because they normally do have 2 factor auth.  

###  Finding API Keys

To search for API keys in a file, you can use the following `grep` command:

```bash
grep -E "\b[a-zA-Z0-9]{32}\b" config_file.txt
```



## Finding Passwords

Passwords can take many forms and are often found in configuration files, logs, or databases. A common format for passwords includes combinations of letters, numbers, and special characters. While the exact regex pattern may vary, here's a general pattern to match typical passwords (assuming they are at least 8 characters long and include letters, numbers, and special characters):

### Regex for Passwords

The regex pattern to match typical passwords is:

```regex
\b[a-zA-Z0-9!@#$%^&*()_+{}\[\]:;"'<>?,./]{8,}\b
```

### Using `grep` to Find Passwords

you can try to search for passwords in a file using the following  command:

```bash
grep -E "\b[a-zA-Z0-9!@#$%^&*()_+{}\[\]:;\"'<>?,./]{8,}\b" filename
```


### Finding Password Definitions

To search for lines where passwords are defined in a file, look for common patterns such as "password=", "password:", "passwd", etc. Here's a regex pattern to find such lines:

### Using `grep` to Find Password Definitions

To search for password definitions in a file, you can use the following command:


```bash
grep -E "\b(password|passwd|pwd|pass)\s*[:=]\s*[^ ]+\b" config_file.txt
```



## Finding Email Addresses

Email addresses are commonly used identifiers in various applications. They typically follow the format `user@domain.com`. 



To search for email addresses in a file, you can use the following `grep` command:

```bash
 grep -E "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b" filename
```



## Finding Credit Card Numbers

Credit card numbers usually follow a pattern of 13 to 19 digits. For simplicity, we'll assume a common format of 16 digits grouped in four sets of four digits.




```bash
grep -E "\b(?:\d[ -]*?){13,16}\b" transactions.txt
```

This command will search for credit card numbers in `transactions.txt`.

## Finding Social Security Numbers (SSNs)

In the United States, Social Security Numbers (SSNs) are often formatted as `XXX-XX-XXXX`.





```bash
grep -E "\b\d{3}-\d{2}-\d{4}\b" employee_records.txt
```



