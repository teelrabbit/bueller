#### What is a symlink
- a symlink stands for symbolic link
- symbolic links are for linux but a variation of the same technilogy exists on windows
- on windows a symbolic link is refered to as an "mk-link"
- they can be used if you want to move a file or directory but you still want it to point to the same loction (so it is symboliclly linked)
- this can be done using the following command
```bash
ln -s <directiory of the file you want> <the name of the link file>
```
- the "-s" stands for symbolic link
- the new link file will be made in the directory you are currenly in or specify
### Hardlinks vs Softlinks
- every file inside you filesystem has a corresponding inode
- inodes are databases of file, it contains information about file
- an inode contains file_size, permissoons, file_type, number of links
##### Soft Link
- softlinks are like shortcuts (i.e a symbolic link)
- softlinks are pointers to the original file 
- these also have diffrent inode numbers
#### Hard Link
- a hard link is a diffrent name for the same file
- it will have the same file size as the orginal file
- they also have the same inode number
- they are like a copy
- if the orginal file is deleted, it will not effect the hard link
- to creat a hardlink use the following command 
```bash
ln <source of the original file> <name of the second hardlink file>
```
- a hardlink will have the same size as its orginal file because it is pointing to the same spot on the harddrive
- any changes to the hardlinked file will also effect the orginal file
