# How to delete a file with too long of file extension (Windows)

When you want to completely delete a directory and it has file with long names inside it, robocopy does a VERY good job:

```
mkdir empty_dir
robocopy empty_dir the_dir_to_delete /s /mir
rmdir empty_dir
rmdir the_dir_to_delete
```
