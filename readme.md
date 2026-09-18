
**1.Configure Git on your machine:**

Set up your global username and email address. Git uses this information to tag every commit you make:


> [!NOTE]
> 
>  In the `git config` commands, you write your **real name** (or nickname) and the **email address tied to your GitHub account**:
> 
> - **Username:** Write your full name or the name you want attached to your code commits (e.g., `"John Doe"` or `"Alex Smith"`). This doesn't strictly have to match your GitHub username, but using your real name or GitHub handle makes it easy to identify your work.
>     
> - **Email:** Write the exact email address you used to register your **GitHub/GitLab account** (e.g., `"john@example.com"`).

Bash

```
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Verify your setup:

  

Bash

```
git config --list
```

**2.Create and initialize a local project:**

Create a new project directory, navigate into it, and initialize an empty Git repository:

  

Bash

```
mkdir my-git-project
cd my-git-project
git init
```

**3.Create project files:**

Create a few initial files to work with in your workspace:

  

Bash

```
echo "# My Git Project" > README.md
echo "console.log('Hello Git');" > app.js
```

**4.Check status and stage files:**

Check which files Git is tracking, then add your new files to the Staging Area:

  

Bash

```
# View untracked files
git status

# Stage a single file
git add README.md

# Stage all remaining files in the directory
git add .
```

**5.Commit changes to local repository:**

Record your staged snapshot into the repository's history with a clear commit message:

  

Bash

```
git commit -m "Initial commit: Add README.md and app.js"
```

**6.View status and commit history log:**

Confirm your working directory is clean and review the project's commit history:

  

Bash

```
# Verify staging area and working directory state
git status

# View history log
git log

# View history log in a condensed one-line format
git log --oneline
```

**7.Connect your local repo to GitHub:**

Create a new repository on [GitHub](https://github.com/) (do not initialize it with a README on the website). Then set your main branch and add the remote GitHub URL:

  

Bash

```
# Rename default branch to 'main'
git branch -M main

# Link your local repository to GitHub
git remote add origin https://github.com/your-username/my-git-project.git

# Verify remote connection
git remote -v
```

**8.Push local repository to GitHub:**

Upload your local commits to the remote GitHub repository:

  

Bash

```
git push -u origin main
```

- **`-u origin main`**: Sets `origin/main` as the default upstream target so future pushes only require running `git push`.
    
      
    

**9.Clone a repository from GitHub:**

Simulate working on a new machine or collaborating by cloning the repository into a separate directory:

  

Bash

```
# Move out of current project folder
cd ..

# Clone project into a new folder named 'cloned-project'
git clone https://github.com/your-username/my-git-project.git cloned-project
cd cloned-project
```

**10.Modify files and push updates back to GitHub:**

Make updates in the cloned repository, commit the changes locally, and sync them back to GitHub:

  

Bash

```
# Modify an existing file
echo "console.log('Updated application');" >> app.js

# Check status of modified file
git status

# Stage and commit changes
git add app.js
git commit -m "Update app.js with new log statement"

# Push updates back to GitHub
git push
```

**11.Pull latest changes into original repository:**

Navigate back to your original folder and fetch the new changes pushed from the cloned repository:

Bash

```
cd ../my-git-project
git pull origin main
```


---
In the Git and GitHub workflow, the **username and password (or Personal Access Token / SSH key)** are requested during **Step 8: Push local repository to GitHub** or **Step 9: Clone a private repository**.

Authentication is only triggered when your local Git client communicates directly over the network with a remote server like GitHub:

- **Operations that require authentication:**
    
    - `git push` (pushing local commits to remote)
        
    - `git clone` (cloning a **private** repository)
        
    - `git pull` / `git fetch` (fetching updates from a **private** repository)

> [!NOTE]
>    The Key Difference: `git fetch` vs. `git pull`
> 
> - **`git fetch`**: Downloads updates silently in the background into remote-tracking branches (e.g., `origin/main`). Your local files and active working branch **do not change at all**.
>     
> - **`git pull`**: Performs a `git fetch` **AND** immediately runs a `git merge` on your current branch. It updates your local files automatically.
> - > **`git pull` = `git fetch` + `git merge`**


- **Operations that do NOT require authentication:**
    
    - `git init`, `git add`, `git commit`, `git status`, `git log`, `git branch`, or `git merge` (these are 100% local to your computer).
        
    - `git clone` / `git pull` on a **public** repository (reading public code requires no credentials).
        

> **Important Security Note for GitHub:** GitHub no longer accepts account passwords for Git operations over HTTPS. When Git prompts for a password in the command line, you must enter a **Personal Access Token (PAT)** or configure an **SSH key** instead.


------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1. **Prepare your repository and main branch:**
Ensure you are on the `main` branch and have pulled the latest updates from your remote repository:

```bash
git checkout main
git pull origin main

```


2. **Create and switch to a new feature branch:**
Create a new branch named `feature-login` and switch your working context to it:

```bash
git checkout -b feature-login

```

* **`-b`**: Tells Git to create the branch first before switching to it.


3. **Make changes and commit on the feature branch:**
Add or modify files on your feature branch to simulate working on a isolated feature:

```bash
echo "function login() { console.log('User logged in'); }" > auth.js
git add auth.js
git commit -m "Add basic login functionality"

```


4. **Simulate a conflicting change on the main branch:**
Switch back to the `main` branch and make a change to the exact same line in `auth.js` to trigger a conflict later:

```bash
# Switch back to main
git checkout main

# Create auth.js with different content on main
echo "function login() { console.log('Login via OAuth'); }" > auth.js
git add auth.js
git commit -m "Update login logic to OAuth on main"

```


5. **Attempt to merge the feature branch into main:**
Try merging `feature-login` into `main`. Git will attempt an automatic merge and raise a merge conflict:

```bash
git merge feature-login

```

Git will output a message stating: `CONFLICT (add/add): Merge conflict in auth.js. Automatic merge failed; fix conflicts and then commit the result.`


6. **Inspect the merge conflict:**
Check which files are in a conflicted state using `git status`:

```bash
git status

```

Open `auth.js` in your editor. Git will have added conflict markers to indicate the divergent changes:

```javascript
<<<<<<< HEAD
function login() { console.log('Login via OAuth'); }
=======
function login() { console.log('User logged in'); }
>>>>>>> feature-login

```

* **`<<<<<<< HEAD`**: Marks the beginning of changes on your current branch (`main`).
* **`=======`**: Divides the two conflicting versions.
* **`>>>>>>> feature-login`**: Marks the end of changes coming from the branch being merged.


7. **Resolve the conflict manually:**
Edit `auth.js` to resolve the conflict. Remove the Git marker lines (`<<<<<<<`, `=======`, `>>>>>>>`) and keep the desired final version (or combine both):

```javascript
function login() { 
  console.log('User logged in via OAuth'); 
}

```


8. **Stage and commit the resolved conflict:**
Mark the file as resolved by staging it, then finalize the merge commit:

```bash
# Stage the resolved file
git add auth.js

# Verify all conflicts are resolved
git status

# Finalize the merge commit
git commit -m "Merge feature-login into main and resolve auth.js conflict"

```


9. **Delete the merged feature branch:**
Once the branch has been successfully merged into `main`, clean up the local feature branch:

```bash
git branch -d feature-login

```

* **`-d`**: Safely deletes the branch only if its changes have already been merged.
