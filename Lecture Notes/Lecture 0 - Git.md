# Levture 0 - Git

## Git
- Keep track of changes to code 
- Synchronise code between different people
- Test new features of your program and merge with the original
- Revert to old versions of code

### git clone
- Clone a repo from GitHub

### git add
- Add the file you want to track the next time you save

### git commit
- Take a snapshot of the current repo
- `git commit -m "commit message"`

### git status
- See current status of your repo

### git push 
- Push code from local machine to remote server (GitHub)

### git pull
- Download lates changes from the remote server

### git log
- Show a history of your commits

### git reset
- `git reset --hard <commit>`
- `git reset --hard origin/master`

### merge conflicts
- Code on remote server and local machine is different
- Make the changes and commit

## Using Github with VS Code

### Create new repo from GitHub
1. Create a new repo (no README or LICENSE)
2. Copy repo url (address bar)
3. Ctr + Shift + p to open commande palette 
4. Type `git clone`
5. Paste the repo url
6. Set a folder where you want it in your local machine
7. Open repo on local machine
8. Add README and LICENSE 

### Push local git repo to GitHub
1. Cd to local repo and run `git init` command
2. Click on source control on the left navbar and commit the changes
3. Create a new repo (no README or LICENSE)
4. Copy link of github repo (git)
5. Run the following commands: 
   - `git remote add origin <link to GitHub Repo>`
   - `git push -u origin master`
