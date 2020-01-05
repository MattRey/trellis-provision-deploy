# trellis-provision-deploy
Step-by-step for initial Trellis provision and deploy

Set up Git repository with master and staging branch:

In local project folder (with trellis and site folders):

git init

git add .

git commit -m "First Commit"

git push -u origin master


git branch staging

git checkout staging

git add .

git commit -m "First Staging Commit"

git push -u origin staging



1. Copy wordpress sites from trellis/group_vars/development/wordpress_sites.yml to trellis/group_vars/staging/wordpress_sites.yml and trellis/group_vars/production/wordpress_sites.yml
