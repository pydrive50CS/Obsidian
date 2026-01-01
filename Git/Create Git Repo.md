``` shell
#Create a new Repo
echo "# Obsidian" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/username/repo.git
git push -u origin main

#Upload existing repo
git remote add origin https://github.com/username/repo.git
git branch -M main
git push -u origin main
```