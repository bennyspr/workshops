# Linux Command Line Practice Exercises

## Prerequisites

- A Kali Linux machine (physical or virtual).
- Basic knowledge of how to open the terminal in Kali Linux.
- An internet connection (for some optional exercises).

## 1. Basic Navigation & Directory Structure

### Objective:
Familiarize yourself with common navigation commands (`ls`, `pwd`, `cd`) and directory structure.

### Steps:

1. **Open a Terminal**  

   Click on the Kali Linux icon or use the keyboard shortcut <kbd>Ctrl + Alt + T</kbd>.

3. **Print Working Directory**
   
    `pwd`
    
   This command shows your current location in the filesystem (e.g., `/home/kali`).

3. **List Files and Directories**

    `ls`
    `ls -la`

   The `ls` lists the files/directories in your current folder. The `-la` shows detailed information (permissions, owner, size, etc.) and includes hidden files and directories in the listing.

4. **Change Directories**

    `cd /tmp`
    
   Moves you into the `/tmp` directory. Try check your new location using `pwd`.

5. **Go Back to Home Directory**

    `cd ~`

   The `~` represents your home directory. Verify by running `pwd` again.

## 2. File Creation & Manipulation

### Objective:

Learn how to create, move, copy, rename, and remove files/directories using the command line.

### Steps:

1.  **Create a New Directory**
   
    `mkdir workshop` 
    
    Creates a directory named `workshop` in your current location.
       
2.  **Navigate into the Directory**
    
    `cd workshop` 
    
3.  **Create Empty Files**
   
    `touch file1.txt file2.txt` 
    
    The `touch` creates empty files if they don’t exist.
   
4.  **List Files**
        
    `ls -l` 
    
5.  **Rename a File**
    
    `mv file1.txt renamed_file1.txt` 
    
    The `mv` is used both for moving and renaming.
    
6.  **Create a Subdirectory and Move Files**

    `mkdir subfolder`
   `mv file2.txt subfolder/`
    
   Moves `file2.txt` into `subfolder`.
   
7.  **Copy a File**

   `cp renamed_file1.txt copy_file1.txt`
    
   Creates a copy of `renamed_file1.txt` named `copy_file1.txt`.

8.  **Remove Files and Directories**
    
    `rm copy_file1.txt`
   `rm -r subfolder` 
    
    The `rm` removes files, and `-r` removes directories recursively.

## 3. Text Searching & Filtering

### Objective:

Use `cat`, `grep`, and other related tools to search and filter text within files.

### Steps:

1.  **Create a File with Content**
    
    `echo "apple\nbanana\ncherry\napple pie" > fruits.txt` 
    
    This creates a file named `fruits.txt` containing four lines of text.
    
2.  **View File Contents**

    `cat fruits.txt` 
    
    Displays the contents of `fruits.txt`.
   
3.  **Search for a Keyword**

    `grep "apple" fruits.txt` 
    
    The `grep` searches for lines containing `"apple"`.
    
4.  **Search Case-Insensitive**
    
    `grep -i "APPLE" fruits.txt` 
    
    The `-i` ignores case during the search.
    
5.  **Count Lines Containing a Pattern**
    
    `grep -c "apple" fruits.txt` 
    
    The `-c` returns the count of matching lines.
    
6.  **Filter Long Output**
    
    `ls -l /usr/bin | less` 
    
    The `less` allows you to scroll through large outputs. Press `q` to quit.

## 4. Permissions & Ownership

### Objective:

Understand how to view and modify file/directory permissions and ownership using `chmod` and `chown`.

![enter image description here](https://linuxopsys.com/wp-content/uploads/2021/12/linux-permissions-2021-12-1004.png)

![enter image description here](https://tech.auct.eu/explain-linux-file-permission-like-im-five/linux_file_permissions.jpg)

### Steps:

1.  **Check Permissions**
   
    `ls -l fruits.txt` 
    
    The first 10 characters show file type and permissions (e.g., `-rw-r--r--`).
   
2.  **Modify Permissions**

    `chmod 644 fruits.txt` 
    
    Sets `fruits.txt` permissions to `rw-r--r--` (owner has read/write, others read-only).
    
3.  **Change Permissions to Executable**
    
    `chmod +x fruits.txt`
    `ls -l fruits.txt` 
    
    Now the file is executable (`-rwxr--r--`), though it may not run as a script unless it has valid script content.
    
4.  **Change Ownership** (Optional)
    
    `sudo chown root:root fruits.txt` 
    
    Changes the owner to `root` and the group to `root`. Requires `sudo` (superuser) privileges.
   
5.  **Revert Ownership**
    
    `sudo chown kali:kali fruits.txt` 
    
    Replace `kali` with your actual username.

## 5. Processes & Job Control

### Objective:

Learn to manage processes using `ps`, `top`, `kill`, background/foreground job control, etc.

### Steps:

1.  **List Running Processes**
    
    `ps aux` 
    
    Shows currently running processes along with their owners and IDs.
    
2.  **Interactive Process Viewer**
    
    `top` 
    
    Press `q` to exit.
     
3.  **Run a Program in the Background**
    
    `ping google.com > ping_output.txt &` 
    
    The `&` symbol runs the command in the background. Output is redirected to `ping_output.txt`.
   
4.  **Show Background Jobs**
    
    `jobs` 
    
    Shows any background tasks.

5.  **Kill a Background Job**
    
    First, find the Job ID (e.g. `[1]` for the first job) from `jobs`.
    
    `kill %1` 
    
    This terminates the background job with ID `%1`.
    
6.  **Kill by Process ID**
    
    Find the PID of a process using `ps` or `top`.
   
    `kill <PID>`   

## 6. Networking Basics

### Objective:

Use basic network tools like `ifconfig`, `ping`, and `netstat`/`ss` to understand network configuration and connectivity.

### Steps:

1.  **Check Network Configuration**
    
    `ifconfig` 
    
    Displays the network interfaces and their IP addresses. On some Linux systems, you may need `ip a` instead.
    
2.  **Ping a Website**
    
    `ping -c 4 google.com` 
    
    Sends 4 ICMP echo requests to check connectivity with `google.com`.
    
3.  **Check Open Ports and Connections**
    
    `sudo netstat -tulpn` 
    
    Shows active connections and listening ports. Alternatively, use:
       
    `ss -tulpn` 
        
    The `-tulpn` stands for `TCP`, `UDP`, `Listening`, `Processes`, `numeric`.
    
4.  **Traceroute** (Optional)
    
    `traceroute google.com` 
    
    Shows the path that packets take to reach `google.com`.

## 7. Cleanup

### Objective:

Remove or clean up the files and directories created during the exercises.

### Steps:

1.  **Remove the Workshop Directory**
    
    If you are still inside the `workshop` folder, go up one level:
        
    `cd ..` 
        
    Then remove the folder:
      
    `rm -r workshop` 
        
2.  **Verify Removal**
    
    `ls` 
    
    This ensures the `workshop` folder no longer appears in the list.
