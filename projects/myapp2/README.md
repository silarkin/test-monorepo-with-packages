Myapp2
=======

Overview
--------

```myapp2``` is a python application that uses the ```mypkg``` Python package to print "Hello, world!". It is intended as a demonstration of python dependency management in a single repository.

- The symbolic link ```myapp2/mypkg``` with value ```../mypkg``` makes ```mypkg``` visible to myapp2.
- By default, the imported package will be on the same git branch as myapp, so that a single pull request can modify code in both ```myapp2``` and ```mypkg``` (and other applications that use mypkg, if making a non-backward-compatible change).
- It is possible to use and modify a different revision (or branch) of ```mypkg``` by checking out another copy of the repository on a different branch/commit/tag, and updating the ```myapp2/mypkg``` symbolic link. Here's an example:

    # some shell variables for an easier-to-read demo
    export WKDIR=`pwd`
    export ORIGIN=https://github.com/silarkin/test-monorepo-with-packages.git

    # Check out and run myapp2 unmodified with
    # latest (master) application & package code
    #
    cd $WKDIR && git clone $ORIGIN myapp2-repo
    cd myapp2-repo/projects/myapp2
    pwd && git branch | grep '*' && python main.py    

    # create branch of myapp2, modify its behavior
    git checkout -b myapp2-branch
    sed -i '' 's/world/worldlings/g' main.py
    pwd && git branch | grep '*' && git diff
    pwd && git branch | grep '*' && python main.py    

    # Check out separate repo for mypkg development
    # create branch, modify package behavior
    #
    cd $WKDIR && git clone $ORIGIN mypkg-repo
    cd mypkg-repo/projects/shared-libraries/mypkg
    git checkout -b mypkg-branch
    sed -i '' 's/Hello/Hello again/g' greeting.py
    pwd && git branch | grep '*' && git diff
    python ../myapp2/main.py    

    # Go back to application library, run in-development myapp2 with
    # in-development modified libary
    cd $WKDIR/myapp2-repo/projects/myapp2
    ln -sfn $WKDIR/mypkg-repo/projects/shared-library/mypkg mypkg
    pwd && git branch | grep '*' && git diff
    pwd && git branch | grep '*' && python main.py    


- (not working...yet) Symbolic links to packages are .gitignore'd to avoid unintended commits of modified symbolic links.

Unanswered questions
--------------------
- .gitignore of the mypkg symbolic link doesn't work
- Will this work on Windows, macOS and Linux? Thatis, is git's handling of symbolic links cross-platform-compatible?