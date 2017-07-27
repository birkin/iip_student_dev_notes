### 2017-07-27-Thursday

- unicode & python info...

    - first, unicode-strings and byte-strings...

        I think of a unicode representation of a letter as some sort of abstract perfect theoretical representation of that letter.

        How that letter gets 'written' to a file on the disk -- how it is 'encoded' -- can happen different ways.

    - python2

            >>>
            >>> unicode_string = u'föo'
            >>>
            >>> type( unicode_string )
            <type 'unicode'>
            >>>
            >>> unicode_string.encode( 'utf-8' )
            'f\xc3\xb6o'
            >>>
            >>> unicode_string.encode( 'latin-1' )
            'f\xf6o'
            >>>

            >>>
            >>> byte_string = unicode_string.encode( 'utf-8')
            >>>
            >>> type( byte_string )
            <type 'str'>
            >>>
            >>> ### _always_ use 'utf-8' to encode ###
            >>>

    - python3

            >>>
            >>> unicode_string = 'föo'  # no need for the preceeding 'u' -- in python3 all strings are unicode by default
            >>>
            >>> type( unicode_string )
            <class 'str'>
            >>>
            >>> # the above can be _confusing_: 'str' in python3 means it's a unicode string; 'str' in python2 means it's a byte-string
            >>>
            >>> unicode_string.encode( 'utf-8' )
            b'f\xc3\xb6o'
            >>>
            >>> # that preceeding 'b' is the indicator that the string is a byte-string
            >>>
            >>> unicode_string.encode( 'latin-1' )
            b'f\xf6o'
            >>>

            >>>
            >>> byte_string = unicode_string.encode( 'utf-8')
            >>>
            >>> type( byte_string )
            <class 'bytes'>
            >>>
            >>> ### again, _always_ use 'utf-8' to encode ###
            >>>

    - main rule:
        - whenever you're working in code: _always_ make sure the string you're working with is a unicode string
            - sometimes this will happen automatically, sometimes not

    - practicality of the main rule:
        - data in a file will be in some type of encoding (meaning it's in byte-string format)
            - if you have _any_ control over the production of that data, make _sure_ it's encoded in utf-8
        - when you 'read' the data into your code, make __sure__ that __as soon as possible__ in your code, that the data has been converted from a byte-string to a unicode-string
        - keep it in unicode-format until you're ready to write it out

- laptop permissions fix

    - step a: update a gitconfig entry

            cd /path/to/iip_summer_dev_stuff/iip_smr_web_project
            git config core.sharedRepository group

    - step b: update project-directory permissions

            sudo chmod u=rwx,g=rwx,o=rx /path/to/iip_summer_dev_stuff/iip_smr_web_project
            cd /path/to/iip_summer_dev_stuff/iip_smr_web_project
            sudo find ./ -type d | xargs chmod u=rwx,g=rwx,o=rx
            sudo find ./ -type f | xargs sudo chmod u=rw,g=rw,o=r

- goals:

    - go over update script
        - cover concept of 'static files', which they don't have to think about on their own laptops, but do re the server

    - go throuch checking out master

    - git commands...
        - show all branches: `git branch -a`
        - switch to new branch: `git checkout name-of-branch`
        - delete branch: `git branch -d name-of-branch`
        - update server's knowledge of github current branches: `git fetch -p`

    - followup on past xml/xslt conversation, showing them [transformer url](https://library.brown.edu/xsl_transformer/v1/shib/?xml_url=https://library.brown.edu/bjd/transform_test/people.xml&xsl_url=https://library.brown.edu/bjd/transform_test/people_to_web.xsl&auth_key=shib
)
    - issues: have S. add an issue re the bib problem

    - github tips for referring to code
        - note ability to reference individual lines, or multiple lines, in github urls
        - note how to reference unchanging version of code

---


### 2017-07-26-Wednesday

- showed them the [working site](http://dcdscit.services.brown.edu/iip_smr_dev/search/) on the server
    - requires VPN

- covered automatic log-rotataion
    - mac: `cd /etc/newsyslog.d/`
    - showed sample format for file:

            $
            $ cat ./project_log.conf

            # idea: <http://randomerrata.com/post/64121939537/newsyslog-d>
            # docs: <http://www.freebsd.org/cgi/man.cgi?newsyslog.conf(5)>

            # logfilename                  [owner:group]  mode   count  size(KB)  when  flags [/pid_file] [sig_num]
            /path/to/project_logs/*.log    _www:staff     664    5      250       *     G

            $

- goal: master always works, even if not complete

- reminded them to check with G. re her upcoming presentation dates -- if they want to check out a branch on their server

- showed github [releases](https://github.com/Brown-University-Library/iip_smr_web_project/releases)
    - noted one approach: to make releases reflect versions of working code put into production
    - noted benefit: easy to find working version to revert to if things go south

- troubleshot template-error w/S.
    - briefly reviewed concept of reverse url-lookup
    - got the django 1.11 reverse url-lookup syntax working

- got student-update-script working; emailed S. & W. to try it; troubleshot S. problem report w/J.M. -- think we got it working

---


### undated stuff covered over about 3 meetings

- helped them install the updated version of C.R.'s code, [now using python3 & django-1.11](https://github.com/Brown-University-Library/iip_smr_web_project) onto their laptops

- made sure each could make a commit to the github repo

- noted virtual-environment 'gotcha' that deactivating a venv, or reloading, or loading a new one does _not_ eliminate environmental-variables from the environment

- gave them overview of indexing process:
    - inscription-editor makes changes to an inscription
    - editor commits change to github iip-texts repo
    - github fires off a 'webhook' notification to a web-app 'listener' I created
    - the web-app-listener:
        - initiates a git-pull for the iip-texts directory
        - determines which files have been added/updated, and which have been deleted
        - creates a solr-doc and updates solr for each addition/update
        - deletes from solr any needed deletions

- gave them overview of xml & stylesheets

- discussed approaches to 'Stories' feature S. would like to implement
    - showed them architecture of how basic markdown drives the '[welcome](http://dcdscit.services.brown.edu/iip_smr_dev/info/welcome/)', 'about', etc. pages in site
    - showed them how they can use that model to experiment with 'Stories' if they desire

- looked into S.'s report of a problem with bibliographies

---


### 2017-06-27-Tuesday

- goal: deploy to 'production' regularly
    - production being your dev server
    - meaning you should be able to email E.M., or Prof, a url every day of a new feature, or of added functionality

- related to the above, the utility of using a script to update the server code from github
    - the script does a git-pull, and...
    - 'touches' the file that makes the new code active
    - optional: the script can also update permissions on the code

- keep out of git:
    - passwords
    - server-paths
    - db and db-table names
    - strong suggestion: do not count on .gitignore to keep out settings
