### 20120-Feb-12-Wednesday

review django flow

one-time iip local setup...
- fork iip repository (Brown-University-Library/iip-production) into own account
- set up local iip_webapp_stuff dir
- set up env_settings, db, and logs directories in iip_webapp_stuff
- clone student iip-webapp into iip_webapp_stuff
- start getting it to work

workflow...
- note: master-branch is the in-production branch (generally)
- make a branch, for your work, off master or dev (we'll figure that out) -- and make all your changes to that
- when you think an improvement is working, make a pull-request from your branch to `Brown-University-Library/iip-production (dev-branch)`
- i will:
    - merge your changes in to the dev branch
    - make them live on our dev server ()
    - i'll let you and E know that the changes are live, so E can review them
    - once i get the ok, i'll merge the Brown University dev branch into master and update production.

github good practices:
- keep out of git:
    - passwords
    - server-paths
    - db and db-table names
    - strong suggestion: do not count on .gitignore to keep out settings. Instead, use an env_settings.sh file located _outside_ of the git-code directory.
- goal: deploy code regularly, meaning that (ideally) at the end of each day, you could email E and let her know of a new feature or functionality, even if small.

---
