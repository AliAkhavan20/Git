# Delete directory with gitlab cli
## To delete a directory in a GitLab repository using Git Bash, you can follow these steps:

### Clone the Repository:
#### First, you need to clone the GitLab repository to your local machine if you haven't already. Replace <repository_url> with the actual URL of your GitLab repository:
```
git clone <repository_url>
```
### Navigate to the Local Repository:
#### Change your current working directory to the cloned repository:
```
cd <repository_directory>
```
### Delete the Directory:
#### To delete a directory, you can use the rm (remove) command with the -r (recursive) option. Replace <directory_name> with the name of the directory you want to delete:
```
rm -r <directory_name>
```
> For example, if you want to delete a directory named "my_directory," you would use:
```
rm -r my_directory
```
### Commit the Deletion:
#### After deleting the directory locally, you need to commit the change to your Git repository:
```
git add .
```
```
git commit -m "Delete <directory_name>"
```
> Replace <directory_name> with the name of the directory you deleted.

### Push the Changes to GitLab:
#### Finally, push the commit to your GitLab repository:
```
git push origin <branch_name>
```
> Replace <branch_name> with the name of the branch where you want to push the changes, typically master or another branch you are working on.

> After completing these steps, the directory will be deleted from your GitLab repository. Please exercise caution when deleting files and directories, as this action cannot be undone. Make sure you have a backup or confirm that you want to proceed with the deletion.




