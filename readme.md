## Terminology(From ChatGPT)

### Working Directory

**Definition**: The working directory is the directory on your filesystem where you create, modify, and delete files. It's where you do your actual work.

- When you edit a file, those changes are made in the working directory.


### Staging Area (Index)

**Definition**: The staging area (also called the index) is an intermediate area where changes are gathered before they are committed to the repository.

- When you use the `git add` command, you move changes from the working directory to the staging area.


## Git Restore
Mainly used to undo changes in the **uncommitted** files .
### unstaged files
```bash
  git restore <file_name>
```
Restores files in working directory to its state at the last commit. Also it does not delete or change newly created files.


### staged files
```bash
  git restore --staged <file_name>
```
Unstages the files but does not undo the changes in the files. Also does not delete or change newly created files.

To undo changes, use `git restore` after unstaging the files. 

```bash
  git restore --staged <file_name>
  git restore <file_name>
```

### --worktree flag
`git restore --worktree <file_name>` works same as `git restore <file_name>`.

But when combined with the `--staged` flag, the command both unstages and undo changes from the files. It even deletes newly created files.
```bash
  git restore --staged --worktree <file_name>
```
This commad will make the full work directory similar to the last commit.

## Git Clean
`git clean` cleans working directory by removing untracked files. These untracked files can include files that are not part of the version control (i.e., files not staged or committed) and ignored files .

#### **`-f`  flag** :
This is required for actually removing all untracked files. Git will not delete anything unless the `-f` flag is used.

 ```bash
  git clean -f 
 ```   
### **`-d`** flag:

This option removes untracked directories in addition to untracked files.

   ```bash
    git clean -fd
   ```
   >note : **-f** is mandatory for removing anything. 
   >
 ### **`-x`** flag:
Normally, `git clean` will not remove ignored files (files listed in `.gitignore`). The `-x` option forces Git to also remove those ignored files.
    
  ```bash
 git clean -f -x
 ```       
 ###  **`-X`** flag:
    
Similar to [`-x`](#-x-flag),  but only removes ignored files, not untracked files. 
```bash
git clean -f -X
```
  
## Git Revert
Creates a new commit that undoes the changes of a specific commit. So, reverting last commit, we can undo changes in the already committed files. It also deletes newly created files.
```bash
  git revert <commit-hash>
```
### --no-commit flag
`--no-commit` flag prevents Git from automatically committing the revert. It will only delete the newly created files and undo the chages. all these changes will be in **staged condition** ( verify using `git status` ).
```bash
  git revert --no-commit <commit-hash>
```
Now `git commit` can be used to complete the revert.

## Git Reset
It is a powerful command to undo changes by moving current **HEAD** to a spefic state/commit. While `git revert` creates a new commit on top of all previous commits, `git reset` changes the commit history.

### --soft flag
```bash
  git reset --soft <commit-hash>
```
Moves the **HEAD** pointer to the specified commit but leaves the **working directory** and the **staging area** unchanged. so, it will only remove the spesific commit but the files will remain as staged and all the changes will remain visible in files. 

To undo changes from files, further [unstaging and restoring](#staged-files) is needed. 

### --mixed flag
```bash
  git reset --mixed <commit-hash>
```
Similarly, moves the **HEAD** pointer but the files become unstaged also. But the changes are still visible on the files. 

Further [restoring](#unstaged-files) is needed to undo changes from files.

### --hard flag
```bash
  git reset --hard <commit-hash>
```
Moves the **HEAD** pointer, **unstages** the files and also **undoes** all the changes.

## Rollback Git Reset
`git reset --hard` can be used to restore the commit just reset. As, commit history is changed, **commit hash** can be found from `git reflog`.

Get the commit-hash -
```bash
  git reflog
``` 
Reset hard to the restore the commit -
```bash
  git reset --hard <reset-commit-hash>
``` 
