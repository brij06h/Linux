# File Permissions in Linux

In Linux, every file has permissions that control who can read, write, or execute it.

4 = read

6 = read + write

7 = full control

# chmod permission

chmod 444 file -> read only for everyone

chmod 644 file -> normal safe sertting (recommended)


# to check perissions

ls -l -> check permission

-rm-r--r--

 rw- : owner can read/write
 
 r-- : group can read
 
 r-- : others can read

# what 444 means

4 = read only (r)

2 = write (w)

1 = execute (x)


# While pushing code to GitHub, I got a divergent branch / merge conflict issue.

This happened because:

My local branch had 1 commit
GitHub (remote repo) also had 1 different commit

So Git could not automatically combine them.

in short Local and GitHub (origin/main) had different commits.

# commands i've used 

git pull --rebase origin main

git add .

git commit -m "fix merge conflict"

git push origin main
