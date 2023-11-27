# add files and folders into source 

### First clone the app you want to add folder to it.
```
git clone http://192.168.74.130/nop/nopcommerce.git
```
### change directory into the project.
```
cd nopcommerce
```
### Copy files or folders you want into project directory.
```
cp -r /path/to/local/folders/* copy-after-publish/
```
### add the dir to project
```
git add copy-after-publish/
```
### Commit the new changes and add a comment
```
git commit -m "Add folders to copy-after-publish"
```
### Push the project you cloned back into origin location
```
git push origin main
```