# File System Management in Linux

Understanding and managing the Linux file system is crucial for effective system administration and cybersecurity work. Linux uses a hierarchical file system structure, which is organized in a tree-like pattern of directories and files.

## Linux File System Hierarchy

1. **/** (Root Directory): The top-level directory of the entire file system hierarchy.
2. **/home**: Contains user home directories.
3. **/etc**: System configuration files.
4. **/var**: Variable data like logs and temporary files.
5. **/usr**: User binaries and program data.
6. **/bin**: Essential command binaries.
7. **/sbin**: System binaries.
8. **/tmp**: Temporary files.
9. **/boot**: Boot loader files.
10. **/dev**: Device files.

## File and Directory Operations

1. **Creating Files**:
   - `touch filename.txt`: Creates an empty file.
   - `echo "content" > filename.txt`: Creates a file with content.

2. **Viewing File Content**:
   - `cat filename.txt`: Displays entire file content.
   - `less filename.txt`: Views file content page by page.
   - `head -n 10 filename.txt`: Shows first 10 lines.
   - `tail -n 10 filename.txt`: Shows last 10 lines.

3. **Editing Files**:
   - `nano filename.txt`: Opens the nano text editor.
   - `vim filename.txt`: Opens the vim text editor.

4. **File Permissions**:
   - `chmod`: Changes file permissions (read, write, execute).
   - `chown`: Changes file ownership.
   - Example: `chmod 644 filename.txt`

5. **Finding Files**:
   - `find /path -name "filename"`: Searches for files by name.
   - `locate filename`: Quickly finds files (requires updated database).

6. **Disk Usage**:
   - `du -sh /path`: Shows disk usage of a directory.
   - `df -h`: Displays free disk space.

7. **File Compression**:
   - `tar -cvzf archive.tar.gz /path/to/directory`: Creates a compressed archive.
   - `tar -xvzf archive.tar.gz`: Extracts a compressed archive.

8. **Symbolic Links**:
   - `ln -s /path/to/file linkname`: Creates a symbolic link.

## File System Security Best Practices

1. Use appropriate file permissions.
2. Regularly audit file system for suspicious changes.
3. Use encryption for sensitive data.
4. Implement access controls and file system quotas.
5. Regularly backup important data.

Understanding file system management is essential for maintaining system integrity and security in Linux environments.
