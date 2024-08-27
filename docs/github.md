# Github  
For a more in depth overview [go to the official documentation](https://docs.github.com/en/get-started/using-github/github-flow).  

## 1. Setting Up a Local Git Repository  

1. **Initialize a new Git repository:**

    ```sh
    cd /path/to/your/project
    git init
    ```

    - **`cd /path/to/your/project`**: Change directory to your project folder.
    - **`git init`**: Initialize a new Git repository in the current directory.

2. **Add files to the repository:**

    ```sh
    git add .
    ```

    - **`git add .`**: Stage all files in the current directory for the next commit.

3. **Commit the files:**

    ```sh
    git commit -m "Initial commit"
    ```

    - **`git commit -m "Initial commit"`**: Commit the staged files with a message.

## 2. Authenticating Using SSH Key  

1. **Generate an SSH key pair (if you don't have one):**

    ```sh
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    ```

    - **`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`**: Generate a new SSH key with RSA encryption.

2. **Add the SSH key to your SSH agent:**

    ```sh
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_rsa
    ```

    - **`eval "$(ssh-agent -s)"`**: Start the SSH agent.
    - **`ssh-add ~/.ssh/id_rsa`**: Add your SSH private key to the SSH agent.

3. **Add the SSH key to your GitHub/GitLab/Bitbucket account:**

    - Copy the SSH key to your clipboard:

        ```sh
        cat ~/.ssh/id_rsa.pub
        ```

    - Go to your Git hosting service and add the SSH key to your account settings.

## 3. Pushing the Local Repository to a Remote Repository  

1. **Add the remote repository:**

    ```sh
    git remote add origin git@github.com:username/repo.git
    ```

    - **`git remote add origin git@github.com:username/repo.git`**: Add a remote repository with the alias `origin`.

2. **Push the local repository to the remote repository:**

    ```sh
    git push -u origin main
    ```

    - **`git push -u origin main`**: Push the local `main` branch to the `origin` remote repository and set it as the default upstream branch.

## 4. Making Changes and Syncing Them  

1. **Make changes to your files.**

2. **Stage the changes:**

    ```sh
    git add .
    ```

    - **`git add .`**: Stage all changed files for the next commit.

3. **Commit the changes:**

    ```sh
    git commit -m "Describe your changes"
    ```

    - **`git commit -m "Describe your changes"`**: Commit the staged changes with a descriptive message.

4. **Push the changes to the remote repository:**

    ```sh
    git push
    ```

    - **`git push`**: Push the committed changes to the remote repository.

## 5. Keeping Your Local Repository in Sync with the Remote  

1. **Fetch the latest changes from the remote repository:**

    ```sh
    git fetch origin
    ```

    - **`git fetch origin`**: Fetch the latest changes from the `origin` remote repository.

2. **Merge the fetched changes into your local branch:**

    ```sh
    git merge origin/main
    ```

    - **`git merge origin/main`**: Merge the fetched `main` branch into your local `main` branch.

3. **Pull the latest changes (fetch + merge):**

    ```sh
    git pull
    ```

    - **`git pull`**: Fetch and merge the latest changes from the remote repository into your local branch.

## Full Command Summary  

1. **Initialize Git repository:**

    ```sh
    cd /path/to/your/project
    git init
    git add .
    git commit -m "Initial commit"
    ```

2. **Generate and add SSH key:**

    ```sh
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_rsa
    cat ~/.ssh/id_rsa.pub
    # Add the key to your Git hosting account settings
    ```

3. **Add and push to remote repository:**

    ```sh
    git remote add origin git@github.com:username/repo.git
    git push -u origin main
    ```

4. **Make changes and push:**

    ```sh
    # Make changes to your files
    git add .
    git commit -m "Describe your changes"
    git push
    ```

5. **Sync with remote repository:**

    ```sh
    git pull
    ```

By following these steps and using the commands provided, you can effectively manage your local and remote Git repositories.