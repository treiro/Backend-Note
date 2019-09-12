merge is used to bring two (or more) branches together.

a little example:

# on branch A:
# create new branch B
$ git checkout -b B
# hack hack
$ git commit -am "commit on branch B"

# create new branch C from A
$ git checkout -b C A
# hack hack
$ git commit -am "commit on branch C"

# go back to branch A
$ git checkout A
# hack hack
$ git commit -am "commit on branch A"
so now there are three separate branches (namely A B and C) with different heads

to get the changes from B and C back to A, checkout A (already done in this example) and then use the merge command:

# create an octopus merge
$ git merge B C
your history will then look something like this:

…-o-o-x-------A
      |\     /|
      | B---/ |
       \     /
        C---/
