# trellis-provision-deploy
Step-by-step for initial Trellis provision and deploy

Set up Git repository with master and staging branch:

In local project folder (with trellis and site folders):
```
git init

git add .

git commit -m "First Commit"

git remote add origin 'remote repository url'

git remote -v

git push -u origin master
```

If you need a staging branch too:

```
git branch staging

git checkout staging

git add .

git commit -m "First Staging Commit"

git push -u origin staging
```


1. Copy wordpress sites from trellis/group_vars/development/wordpress_sites.yml to trellis/group_vars/staging/wordpress_sites.yml and trellis/group_vars/production/wordpress_sites.yml - can remove the redirects from staging becuase they are not needed. Add the ssh link to github repo and the two branches created above. Enable ssl from letsencrypt.


2. Update trellis/ansible.cfg to add 'vault_password_file = .vault_pass' to [defaults]

3. Create  .vault_pass file in trellis directory and add a random 64 - character string to it (from e.g. https://www.randomlists.com/) run chmod 600 .vault_pass from the trellis directory to restrict access to it.

4. Set server names for staging and production in trellis/hosts/staging and trellis/hosts/production

5. In group_vars/all/users.yml change admin user name at top and add link to your ssh key on github (https://github.com/MattRey.keys)

6. set   sshd_permit_root_login: false in group_vars/all/security.yml

7. Generate a set of random passwords at random.org and copy one to the vault_users password in trellis/group_vars/staging/vault.yml and trellis/group_vars/production/vault.yml

8. ssh into root at your server and run adduser new_user_name (using the first random password generated in the above step as the users password)  then run usermod -aG sudo new_user_name 
switch user by typing in  su - new_user_name and then test the privileges by typing in sudo ls -la /root (you will be prompted for the password you entered for the new user and a list of root files should then be displayed)

9. Copy the remainig random passwords over to the password fields in the files in step 7. and in trellis/group_vars/all/vault.yml

10. Generate salts at https://roots.io/salts.html and copy/paste in the trellis details for two sets into the staging and production vault.yml files

11. Encrypt files using ansible vault

12. Commit changes to version control 

13. Provision server with ansible-playbook server.yml -e env=<environment> 
  
14. Deploy with ansible-playbook deploy.yml -e "site=staging.mysite.co.uk env=staging" -i hosts/staging and ansible-playbook deploy.yml -e "site=mysite.co.uk env=production" -i hosts/production
