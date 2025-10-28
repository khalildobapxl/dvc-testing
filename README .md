## How to use DVC

### What is DVC?

DVC (Data Version Control) is a tool that helps you version control datasets and machine learning models alongside your code using Git. It lets you manage large data files efficiently without storing them directly in Git, enabling easy collaboration and reproducibility.

## Getting Started with DVC

### Have a git repo

You should have a git repo to keep the versioning on. 

### Install DVC

Make a virtual environment to install dvc on. `pip install dvc`

### Initialise DVC

in the root dir of the repo: `dvc init`
> Make sure you are in the venv: `source .venv/bin/activate`
### Commit the changes

when initialising dvc, we get 3 files: `.dvc/.gitignore` `.dvc/config` `.dvcignore` we commit those first before starting with versioning

### Track a file or a folder

Now we can finally add the dataset or folder containing datasets with `dvc add test.csv`

> The file mustn't be tracked with git, otherwise, error

This creates 2 files, `.gitignore` to ignore the dataset itself, the other one is the versioning of the file.

### Commit the files

Now we can commit the files with git to track the changes. Since each commit has a hash for it, it's gonna be also the same for the version of the dataset. Let's say I made changes to the dataset and did `dvc add` to it and pushed to the repo, if I want the previous version I just do `git checkout #####` to that previous commit, after that: `dvc checkout` will bring the dataset to its previous state in that commit.


## Using DVC with Google Drive


### Step 1: Install dvc-gdrive

```bash
pip install dvc-gdrive
```
#### Step 2: Google API Console

Sign in to [Google API Console](https://console.developers.google.com/) and create a project. From **APIs & Services Dashboard** click on 
**+ ENABLE APIS AND SERVICES** (on top), Find `Google Drive API` and enable it

#### Step 3: OAuth


**APIs & Services** in the left sidebar. Select **OAuth consent screen**. Click on **Get Started**. Add app name and your email address so people can reach out if there are problems etc. Under **Audience** choose **External**. Add your email after this for google to contact you for changes in app. Create app.


#### Step 4: Create Credentials

You need to create credentials to be able to authenticate:
1. Go back to **APIs & Services** (Top left dropdown menu then API & Services)
2. Go to **Credentials** $\rightarrow$ **+ Create Credentials** $\rightarrow$  **OAuth client ID**
3. Choose **Desktop APP** as app type
4. Choose name
5. Download Credentials

### Step 5: Add Test Users

Under OAuth you see **Audience**, in there you can add users to be able to use the drive you have. Otherwise they get access denied when authenticating. Add the google accounts they will use when authenticating.

### Step 5: Configure DVC with Google Drive

> On Google Drive each folder has a unique ID, when we make a folder and open it we see the URL as follows:`https://drive.google.com/drive/u/0/folders/1IFqX6MRJMHwYh1RYwHdZv98Wz_Ko9Dx7` The ID of the folder is: `1IFqX6MRJMHwYh1RYwHdZv98Wz_Ko9Dx7` We will need it to tell dvc which folder it should push to and pull from

Now we configure dvc with gdrive:
1. `dvc remote add --default dvc-testing gdrive://1IFqX6MRJMHwYh1RYwHdZv98Wz_Ko9Dx7`
   After --default comes the name of the remote, free to choose.
2. `dvc remote modify dvc-testing gdrive_acknowledge_abuse true`
3. `dvc remote modify dvc-testing --local gdrive_client_id 'CLIENT_ID'`
   `dvc remote modify dvc-testing --local gdrive_client_secret 'CLIENT_SECRET'`
   We get those from the json file we downloaded earlier. Use `--local`, otherwise the credentials will reside in config file that is pushed to the repo, which you don't want. (When working in a team, when team member do all of those steps and share the `client_id` and `client_secret` with the rest)



### Step 5: Authenticating and Usage

After we configure dvc, we can `dvc push` or `dvc pull`, which will give a URL in the terminal, we click on it to authenticate with a trusted google account under Test Users.






## Read Also: [The Complete Guide to Data Version Control With DVC | DataCamp](https://www.datacamp.com/tutorial/data-version-control-dvc)




