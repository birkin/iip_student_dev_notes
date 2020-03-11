### 2020-Mar-11-Wednesday

##### solr info

- great tutorial: <https://github.com/hectorcorrea/solr-for-newbies/blob/master/tutorial.md>

- simple facet-query: <https://library.brown.edu/cds/projects/iip/api/?start=0&rows=0&indent=on&fl=type&q=*:*&facet=on&facet.field=type>
    - shows start and end rows as 0, since this query is just to get facet info
    - nothing is being filtered -- `q=*:*`
    - shows only the counts for the field `type`

- another facet-query: <https://library.brown.edu/cds/projects/iip/api/?start=0&rows=0&indent=on&fl=type,language&q=*:*&facet=on&facet.field=type&facet.field=language>
    - same as above, but showing counts for `language`, in addition to `type`

- mixing results and facets: <https://library.brown.edu/cds/projects/iip/api/?start=0&rows=6000&indent=on&fl=inscription_id,city_pleiades&q=(language:he)AND(religion:jewish)&facet=on&facet.field=type&facet.field=language&sort=inscription_id%20asc>
    - I'm specifying different fields for the objects returned (`fl=inscription_id,city_pleiades`) -- than for the query (`q=(language:he)AND(religion:jewish)`)
    - For easy checking on results, I added sorting of the returned objects (`sort=inscription_id%20asc`)


---


### 2020-Feb-24-Monday

##### see sent email locally

- issue: sometimes your webapp may want to send you email -- how to see that, without installing/activating email software:

- solution:
    - open up another terminal window/tab
    - enter (no `$` prompt): `$ python3 -m smtpd -n -c DebuggingServer localhost:1026`
    - any email your webapp sends (assuming the host/part is set to `localhost/1026`) will display in that terminal window

---


### 2020-Feb-19-Wednesday

##### review django flow...

- concept: `client -> apache -> django -> client`
- more accurate: `...django -> urls.py (router) -> views.py -> client`
- more accurate: `...django -> urls.py (router) -> views.py -> (produces key-value data) -> template (html) -> client`
- true (ideally): `...django -> urls.py (router) -> views.py -> some_library.py (produces key-value data for views.py) -> template (html) -> client`

##### one-time iip local setup...

- fork iip webapp repository ([link](https://github.com/Brown-University-Library/iip-production)) into own account
- set up local iip_webapp_stuff dir
- set up env_settings, db, and logs directories in iip_webapp_stuff
- clone student iip-webapp into iip_webapp_stuff
- start getting it to work (there's an sqlite db and a settings file I'll give you)

##### workflow...

- note: master-branch is the in-production branch (generally)
- make a branch, for your work, off master or dev (we'll figure that out) -- and make all your changes to that
- when you think an improvement is working, make a pull-request from your branch to `Brown-University-Library/iip-production (dev-branch)`
- i will:
    - look over your changes
    - merge your changes in to the dev branch
    - make them live on our dev server
    - i'll let you and E know that the changes are live, so E can review them
    - once i get the ok, i'll merge the Brown University dev branch into master and update production

##### github good practices...

- keep out of git:
    - passwords
    - server-paths
    - db and db-table names
    - strong suggestion: do not count on .gitignore to keep out settings. Instead, use an env_settings.sh file located _outside_ of the git-code directory.
- goal: deploy code regularly, meaning that (ideally) at the end of each day, you could email E and let her know of a new feature or functionality, even if small.

---
