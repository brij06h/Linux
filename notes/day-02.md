# Day 2 - SSH setup and Github Connection

## What i learned today
- Today i understood something important -github is not just a wensite i use to upload files. i can actually clone my repository into my local system (Ubuntu VM) and work completely from there.
- Earlier, i thought github was something i had to manually open in the browser every time to update files. but now i understand that git works locally, and github is just the remote storge.

---

## Main learning

- I can clone a github repository into my system using Git
- I can work completely from my local machine without opening github UI
- Git kkeeps track of my changes locally
- I only need Github when i want to push my changes

---

#commands i understood better

1. to reload SSH key:
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519

2. to clone git repo:
   git clone  git@github.com:brij06h/Linux.git

3. to sync VM with Github:
   git pull origin main
   
4. to stage the file (or everything):
   git add day-02.md or git add .

5. to commit he file:
   git commit -m "day 2 : added learning notes"

6. to push to Github:
   git push origin main (if the branch is master use master.)
