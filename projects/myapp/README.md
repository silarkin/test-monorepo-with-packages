myapp
=====

Overview
--------

```myapp``` is a python application that uses the ```mypkg``` Python package to print "Hello, world!". It is intended as a demonstration of python dependency management in a single repository.

- The symbolic link ```myapp/mypkg``` with value ```../mypkg``` makes ```mypkg``` visible to myapp.
- By default, the imported package will be on the same git branch as myapp, so that a single pull request can modify code in both ```myapp``` and ```mypkg```.
- It is possible to use and modify a different revision (or branch) of ```mypkg``` by updating the symbolic link. Here's an example:

    cd ~/src
    git clone ~/src/test-monorepo-with-python-and-packages mypkg-repo
    cd mypkg-repo/projects/mypkg
    git checkout -b mypkg-branch
    sed -i '' 's/Hello,/Hello again,/g' greeting.py
    git diff

    cd ~/src
    git clone ~/src/test-monorepo-with-python-and-packages myapp-repo
    cd myapp-repo/projects/myapp
    git checkout -b myapp-branch
    python myapp.py    
    ln -sfn ../../../mypkg-repo/projects/mypkg mypkg
    python myapp.py


- (not working...yet) Symbolic links to packages are .gitignore'd to avoid unintended commits of modified symbolic links.

Unanswered questions
--------------------
- .gitignore of the mypkg symbolic link doesn't work
- Will this work on Windows, macOS and Linux? Thatis, is git's handling of symbolic links cross-platform-compatible?