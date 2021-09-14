# Set up SSH Keys

So let's consider your two GitHub accounts are named githubPersonal and githubWork, respectively.

### Create two SSH keys, saving each to a separate file:

Execute the below mentioned commands in gitbash

```
cd ~/.ssh
ssh-keygen -t rsa -C "your_email_id_associated_with_githubPersonal_account
```

save it as id_rsa_personal when prompted (you can rename personal)

```
Enter passphrase (empty for no passphrase): skip this by pressing enter
Enter same passphrase again: skip this by pressing enter
ssh-keygen -t rsa -C "your_email_id_associated_with_githubWork_account"

save it as id_rsa_work when prompted (you can rename personal)

Enter passphrase (empty for no passphrase): skip this by pressing enter
Enter same passphrase again: skip this by pressing enter
```

The above commands setup the following files:

- id_rsa_personal
- id_rsa_personal.pub
- id_rsa_work
- id_rsa_work.pub

Add the keys to your Github accounts:

Copy the key to your clipboard by opening the file in vs code:

```
code . id_rsa_personal.pub
```

### Add the key to your account:

Go to your GitHub Account Settings
Click “SSH Keys” then “Add SSH key”
Paste your key into the “Key” field and add a relevant title
Click “Add key” then enter your GitHub password to confirm.

Repeat the process for your githubWork account.

Create a configuration file

Create a configuration file to manage the separate keys in ~/.ssh/ using

```
touch config
```

Edit the file using the text editor of your choice. I used vs code -

```
code . config
```

```
\# githubPersonal

Host personal
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_personal

\# githubWork

Host work
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_work
```

Update stored identities

initialize the ssh-agent (use powershell)

```
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
```

Clear currently stored identities:

```
ssh-add -D
```

Add new keys:

```
ssh-add id_rsa_personal
ssh-add id_rsa_work
```

Test to make sure new keys are stored:

```
ssh-add -l
```

Test to make sure Github recognizes the keys:

```
ssh -T personal
Hi githubPersonal! You've successfully authenticated, but GitHub does not provide shell access.


ssh -T work
Hi githubWork! You've successfully authenticated, but GitHub does not provide shell access.
```

### Test PUSH

On Github, create a new repo in your personal account, githubPersonal, called test-personal.
Back on your local machine, create a test directory:

```
cd ~/documents
mkdir test-personal
cd test-personal
```

Add a blank “readme.md” file and PUSH to Github:

```
touch readme.md
git init
git add .
git commit -am "first commit"
git remote add origin git@personal:githubPersonal/test-personal.git
git push origin master
```

Notice how we’re using the custom account, git@personal, instead of git@github.com.

Repeat the process for your githubWork account.

# Changing git user.email

```
[user]
    name = Arnaud Rinquin
    email = rinquin.arnaud@gmail.com

...

[alias]
    setpromail = "config user.email 'arnaud.rinquin@wopata.com'"

```

# useful commands

To add remote

```
git remote add origin git@personal:mrpumpkjng/multipleAccount.git
```

To clone repo

```
git clone git@work:antbou/test.git
```

To use the work user email

```
git setpromail
git config user.email
```

# Ref :

- https://stackoverflow.com/questions/4220416/can-i-specify-multiple-users-for-myself-in-gitconfig
- https://dev.to/raven404/managing-multiple-github-account-using-git-in-windows-2m0h
