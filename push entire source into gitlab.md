# Push entire source into gitlab from your local machine
## go to main project directory 
```
git init
```
add all directory 
```
git add .
```
commit the changes
```
git commit -m "Push existing project to GitLab"
```
add remote repo
```
git remote add origin http://192.168.74.130/root/nopcommerce
```
push to repo in git-lab
```
git push -u origin master
```