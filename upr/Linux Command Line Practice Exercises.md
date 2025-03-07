# Linux Command Line Practice Exercises

## Prerequisites

- A Kali Linux machine (physical or virtual).
- Basic knowledge of how to open the terminal in Kali Linux.
- An internet connection (for some optional exercises).

## 1. Basic Navigation & Directory Structure

**Open a Terminal**  

Click on the Kali Linux icon or use the keyboard shortcut <kbd>Ctrl + Alt + T</kbd>.

**Print Working Directory**
   
`pwd`
    
This command shows your current location in the filesystem (e.g., `/home/kali`).

**List Files and Directories**

`ls`
`ls -la`

The `ls` lists the files/directories in your current folder. The `-la` shows detailed information (permissions, owner, size, etc.) and includes hidden files and directories in the listing.

**Change Directories**

`cd /tmp`
    
Moves you into the `/tmp` directory. Try check your new location using `pwd`.

**Go Back to Home Directory**

`cd ~`

The `~` represents your home directory. Verify by running `pwd` again.

## 2. File Creation & Manipulation

**Create a New Directory**
   
`mkdir workshop` 
    
Creates a directory named `workshop` in your current location.
       
**Navigate into the Directory**
    
`cd workshop` 
    
**Create Empty Files**
   
`touch file1.txt file2.txt` 
    
The `touch` creates empty files if they don’t exist.
   
**List Files**
        
`ls -l` 
    
**Rename a File**
    
`mv file1.txt renamed_file1.txt` 
    
The `mv` is used both for moving and renaming.
    
**Create a Subdirectory and Move Files**

`mkdir subfolder`
`mv file2.txt subfolder/`
    
Moves `file2.txt` into `subfolder`.
   
**Copy a File**

`cp renamed_file1.txt copy_file1.txt`
    
Creates a copy of `renamed_file1.txt` named `copy_file1.txt`.

**Remove Files and Directories**
    
`rm copy_file1.txt`
`rm -r subfolder` 
    
The `rm` removes files, and `-r` removes directories recursively.

## 3. Text Searching & Filtering

**Create a File with Content**
    
`echo "apple\nbanana\ncherry\napple pie" > fruits.txt` 
    
This creates a file named `fruits.txt` containing four lines of text.
    
**View File Contents**

`cat fruits.txt` 
    
Displays the contents of `fruits.txt`.
   
**Search for a Keyword**

`grep "apple" fruits.txt` 
    
The `grep` searches for lines containing `"apple"`.
    
**Search Case-Insensitive**
    
`grep -i "APPLE" fruits.txt` 
    
The `-i` ignores case during the search.
    
**Count Lines Containing a Pattern**
    
`grep -c "apple" fruits.txt` 
    
The `-c` returns the count of matching lines.
    
**Filter Long Output**
    
`ls -l /usr/bin | less` 
    
The `less` allows you to scroll through large outputs. Press `q` to quit.

## 4. Permissions & Ownership

![enter image description here](https://linuxopsys.com/wp-content/uploads/2021/12/linux-permissions-2021-12-1004.png)

![enter image description here](https://tech.auct.eu/explain-linux-file-permission-like-im-five/linux_file_permissions.jpg)

**Check Permissions**
   
`ls -l fruits.txt` 
    
The first 10 characters show file type and permissions (e.g., `-rw-r--r--`).
   
**Modify Permissions**

`chmod 644 fruits.txt` 
    
Sets `fruits.txt` permissions to `rw-r--r--` (owner has read/write, others read-only).
    
**Change Permissions to Executable**
    
`chmod +x fruits.txt`
`ls -l fruits.txt` 
    
Now the file is executable (`-rwxr--r--`), though it may not run as a script unless it has valid script content.
    
**Change Ownership** (Optional)
    
`sudo chown root:root fruits.txt` 
    
Changes the owner to `root` and the group to `root`. Requires `sudo` (superuser) privileges.
   
**Revert Ownership**
    
`sudo chown kali:kali fruits.txt` 
    
Replace `kali` with your actual username.

## 5. Processes & Job Control

**List Running Processes**
    
`ps aux` 
    
Shows currently running processes along with their owners and IDs.
    
**Interactive Process Viewer**
    
`top` 
    
Press `q` to exit.
     
**Run a Program in the Background**
    
`ping google.com > ping_output.txt &` 
    
The `&` symbol runs the command in the background. Output is redirected to `ping_output.txt`.
   
**Show Background Jobs**
    
`jobs` 
    
Shows any background tasks.

**Kill a Background Job**
    
First, find the Job ID (e.g. `[1]` for the first job) from `jobs`.
    
`kill %1` 
    
This terminates the background job with ID `%1`.
    
**Kill by Process ID**
    
Find the PID of a process using `ps` or `top`.
   
`kill <PID>`   

## 6. Cleanup


**Remove the Workshop Directory**
    
If you are still inside the `workshop` folder, go up one level:
        
`cd ..` 
        
Then remove the folder:
      
`rm -r workshop` 
        
**Verify Removal**
    
`ls` 
    
This ensures the `workshop` folder no longer appears in the list.
