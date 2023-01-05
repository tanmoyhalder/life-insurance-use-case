Author: Tanmoy Halder
Date: 05/01/2023
Updated: 05/01/2023

This markdown shows the steps performed in this project. If someone foloows these steps the whole project can be reproduced. Let's begin.

### Pre-requisites

- Python (From anaconda preferrably) - https://www.anaconda.com/
- Docker (I am using Windows 10) - https://docs.docker.com/desktop/install/windows-install/
- Docker Compose - This comes with Docker for Windows. FOr Linux this needs to be installed separately. For Linux based set up, please follow the link mentioned below
- Git - https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
- Sign Up for a GitHub account - https://github.com/join?source=login

### Step 1 - Create a project folder

I have used my local machine (Windows 10) for this project. Someone can choose a Linux based remote machine as well. The set up for a remote machine is a bit complex and requires installation of a few software like docker, python, vscode etc. Since I already have all these in my local machine, I preferred not to spend time on going thrugh the setup process again. 

Windows/MacOS/Linux - Anything is fine as long as you have the required softwares installed. But if someone wants to use a AWS EC2 Linux remote machine, the steps can be found in this video https://www.youtube.com/watch?v=IXSiYkP23zo&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK&index=3

### Step 2 - Create a virtual environment

I have used pipenv package which is a nice python package manager tool. To install, go to your project folder (in a terminal or directly in VS Code open a terminal). Run the command `pip install pipenv`

Next: Navigate to the project root folder and run `pipenv shell` to create a virtual environment. This will also cerate a Pipfile in your root directory. To check the virtual environment run `pipenv --venv`. This is like any other virtual environment.

#### Step 3 - Install libraries

Run `pipenv install pandas numpy scikit-learn matplotlib seaborn mlflow xgboost prefect hyperopt feature-engine joblib jupyter prefect`. This will also create a Pipfile.lock file with more details about the packages and their versions

Next: run `piipenv install pytest --dev` for installing pytest only for development purpose

#### Step 4 - Initialize a empty git repository in your root folder and make a first push

Run `git init` in your root folder. This will create an empty git repository.
Next: create a `.gitignore` file and mention the files you don't want to upload to the GitHub remore repository
Next: Run `git add .` to stage files
Next: Run `git status` to check status
Next: Run `git commit -m <write your message here>` for the first time commit

Next: Create a public repo in your GitHub account
Next: Run `git remote add origin <url for your git repository>` This will set the connection between your repo to the GitHub reomte repo
Next: Run `git push --set-upstream origin main` This will work as a continuous connection between your local repo and reomte repo and will be helpful for further push to the remote repo
Next: When you run `git push origin main` you will get a permission error. The way around is to set up permission for a git push
Next: Run `git remote set-url origin <add your username before github in the repo url https://username@github.com/....>` Once you run this you should get a propmt for password. You can generate the password/personal access token from developer settings in GitHub account. Make sure to note it somewhere
Next: Now you can run `git push origin main` to send your first commit to remote repo

**Note**: Make sure that you are mentioning the files in gitignore, which you want to push (eg. personal acces tokens, any other secrets)


