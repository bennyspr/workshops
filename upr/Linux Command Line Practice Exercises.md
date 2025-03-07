# Linux Command Line Practice Exercises

## Prerequisites

- A Kali Linux machine (physical or virtual).
- Basic knowledge of how to open the terminal in Kali Linux.
- An internet connection (for some optional exercises).

## 1. Basic Navigation & Directory Structure

**Open a Terminal**  

Click on the Kali Linux icon or use the keyboard shortcut <kbd>Ctrl + Alt + T</kbd>.

**Print Working Directory**

```bash
pwd
```
    
This command shows your current location in the filesystem (e.g., `/home/kali`).

**List Files and Directories**

```bash
ls
```

```bash
ls -la
```

The `ls` lists the files/directories in your current folder. The `-la` shows detailed information (permissions, owner, size, etc.) and includes hidden files and directories in the listing.

**Change Directories**

```bash
cd /tmp
```
    
Moves you into the `/tmp` directory. Try check your new location using `pwd`.

**Go Back to Home Directory**

```bash
cd ~
```

The `~` represents your home directory. Verify by running `pwd` again.

## 2. File Creation & Manipulation

**Create a New Directory**
   

```bash
mkdir workshop
```
    
Creates a directory named `workshop` in your current location.
       
**Navigate into the Directory**
    
```bash
cd workshop
```
    
**Create Empty Files**
   
```bash
touch file1.txt file2.txtcd workshop
```
    
The `touch` creates empty files if they don’t exist.
   
**List Files**
        
```bash
ls -l
```
    
**Rename a File**
    
```bash
mv file1.txt renamed_file1.txt
```
    
The `mv` is used both for moving and renaming.
    
**Create a Subdirectory and Move Files**

```bash
mkdir subfolder
```

```bash
mv file2.txt subfolder/
```

Moves `file2.txt` into `subfolder`.
   
**Copy a File**

```bash
cp renamed_file1.txt copy_file1.txt
```
    
Creates a copy of `renamed_file1.txt` named `copy_file1.txt`.

**Remove Files and Directories**

```bash
rm copy_file1.txt
```

```bash
rm -r subfolder
``` 
    
The `rm` removes files, and `-r` removes directories recursively.

## 3. Text Searching & Filtering

**Create a File with Content**

```bash
echo "apple\nbanana\ncherry\napple pie" > fruits.txt
```
    
This creates a file named `fruits.txt` containing four lines of text.
    
**View File Contents**

```bash
cat fruits.txt
```

Displays the contents of `fruits.txt`.
   
**Search for a Keyword**

```bash
grep "apple" fruits.txt
```

The `grep` searches for lines containing `"apple"`.
    
**Search Case-Insensitive**
    
```bash
grep -i "APPLE" fruits.txt
```

The `-i` ignores case during the search.
    
**Count Lines Containing a Pattern**
    
```bash
grep -c "apple" fruits.txt
```

The `-c` returns the count of matching lines.
    
**Filter Long Output**
    
```bash
ls -l /usr/bin | less
```

The `less` allows you to scroll through large outputs. Press `q` to quit.

## 4. Permissions & Ownership

![enter image description here](https://linuxopsys.com/wp-content/uploads/2021/12/linux-permissions-2021-12-1004.png)

![enter image description here](https://tech.auct.eu/explain-linux-file-permission-like-im-five/linux_file_permissions.jpg)

**Check Permissions**
   
```bash
ls -l fruits.txt
```
    
The first 10 characters show file type and permissions (e.g., `-rw-r--r--`).
   
**Modify Permissions**

```bash
chmod 644 fruits.txt
```

Sets `fruits.txt` permissions to `rw-r--r--` (owner has read/write, others read-only).
    
**Change Permissions to Executable**

```bash
chmod +x fruits.txt
```

```bash
ls -l fruits.txt
```
 
Now the file is executable (`-rwxr--r--`), though it may not run as a script unless it has valid script content.
    
**Change Ownership** (Optional)
    
```bash
sudo chown root:root fruits.txt
```

Changes the owner to `root` and the group to `root`. Requires `sudo` (superuser) privileges.
   
**Revert Ownership**
    
```bash
sudo chown kali:kali fruits.txt
```
    
Replace `kali` with your actual username.

## 5. Processes & Job Control

**List Running Processes**
    
```bash
ps aux
```   
Shows currently running processes along with their owners and IDs.
    
**Interactive Process Viewer**
    
```bash
top
```   
    
Press `q` to exit.
     
**Run a Program in the Background**
    
```bash
ping google.com > ping_output.txt &
```   

The `&` symbol runs the command in the background. Output is redirected to `ping_output.txt`.
   
**Show Background Jobs**
    
```bash
jobs
```  

Shows any background tasks.

**Kill a Background Job**
    
First, find the Job ID (e.g. `[1]` for the first job) from `jobs`.
    
```bash
kill %1
```  
  
This terminates the background job with ID `%1`.
    
**Kill by Process ID**
    
Find the PID of a process using `ps` or `top`.
   
```bash
kill <PID>
``` 

## 6. Cleanup

**Remove the Workshop Directory**
    
If you are still inside the `workshop` folder, go up one level:
        
```bash
cd ..
``` 
        
Then remove the folder:
      
```bash
rm -r workshop
``` 
        
**Verify Removal**
    
```bash
ls
``` 

This ensures the `workshop` folder no longer appears in the list.
