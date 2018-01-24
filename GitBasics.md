#Git Basics
--

###1. First time Setup
Set your name and email that associated with your git.

	$ git config --global user.name "John Doe"
	$ git config --global user.email johndoe@example.com
	
To check you settings:

	$ git config --list
	$ git config user.name
	
###2. Get help
If you ever need help while using Git, there are two equivalent ways to get the comprehensive manual page (manpage) help for any of the Git commands:

	$ git help <verb>
	$ man git-<verb>
	
In addition, if you don’t need the full-blown manpage help, but just need a quick refresher on the available options for a Git command, you can ask for the more concise “help” output with the -h or --help options, as in:

	$ git add -h
	usage: git add [<options>] [--] <pathspec>...

    -n, --dry-run         dry run
    -v, --verbose         be verbose
	
###3. Create and clone a new repository
As I want to use Github, I just create a new repository on Github and clone it locally. Just select "Clone with HTTPS" on Github, copy the URL. 

On your laptop, direct to the folder you like in terminal, then use the command:

	$ git clone https://github.com/Tian-Sky/EffectiveJava.git

That creates a directory named *EffectiveJava*, initializes a .git directory inside it, pulls down all the data for that repository, and checks out a working copy of the latest version. If you go into the new *EffectiveJava* directory that was just created, you’ll see the project files in there, ready to be worked on or used.

If you want to clone the repository into a directory named something other than *EffectiveJava*, you can specify that as the next command-line option:

	$ git clone https://github.com/Tian-Sky/EffectiveJava.git OtherName

That command does the same thing as the previous one, but the target directory is called *OtherName*.

###4. Recording changes
At this step, you can start to make changes and commit them.

Remember that each file in your working directory can be in one of two states: tracked or untracked. Tracked files are files that were in the last snapshot; they can be unmodified, modified, or staged. In short, tracked files are files that Git knows about.

Untracked files are everything else — any files in your working directory that were not in your last snapshot and are not in your staging area. When you first clone a repository, all of your files will be tracked and unmodified because Git just checked them out and you haven’t edited anything.

As you edit files, Git sees them as modified, because you’ve changed them since your last commit. As you work, you selectively stage these modified files and then commit all those staged changes, and the cycle repeats.

####Checking status of your file

	$ git status
	On branch master
	Your branch is up-to-date with 'origin/master'.
	nothing to commit, working directory clean

This means you have a clean working directory — in other words, none of your tracked files are modified. 

####Add new file
Let’s say you add a new file to your project, a simple README file. If the file didn’t exist before, and you run git status, you see your untracked file like so:

	$ echo 'My Project' > README
	$ git status
	On branch master
	Your branch is up-to-date with 'origin/master'.
	Untracked files:
  	  (use "git add <file>..." to include in what will be committed)

      README

	nothing added to commit but untracked files present (use "git add" to track)

To track your new file, just use:

	$ git add README
	
Now check your states and you will see:

	$ git status
	On branch master
	Your branch is up-to-date with 'origin/master'.
	Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

    new file:   README

####Staging Modified Files
If you change a file, and use ``git add``, then make changes to it again, you will see something like this:

	$ vim CONTRIBUTING.md
	$ git status
	On branch master
	Your branch is up-to-date with 'origin/master'.
	Changes to be committed:
 	 	(use "git reset HEAD <file>..." to unstage)

   	 	new file:   README
    	modified:   CONTRIBUTING.md

	Changes not staged for commit:
  		(use "git add <file>..." to update what will be committed)
  		(use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md

Now CONTRIBUTING.md is listed as **both staged and unstaged**. How is that possible? It turns out that Git stages a file exactly as it is when you run the git add command. If you commit now, the version of CONTRIBUTING.md as it was when you last ran the git add command is how it will go into the commit, not the version of the file as it looks in your working directory when you run git commit. If you modify a file after you run git add, you have to run git add again to stage the latest version of the file.

####Short Status
While the git status output is pretty comprehensive, it’s also quite wordy. Git also has a short status flag so you can see your changes in a more compact way. If you run git status -s or git status --short you get a far more simplified output from the command:

	$ git status -s
	 M README
	MM Rakefile
	A  lib/git.rb
	M  lib/simplegit.rb
	?? LICENSE.txt

New files that aren’t tracked have a ?? next to them, new files that have been added to the staging area have an A, modified files have an M and so on. There are two columns to the output - the left-hand column indicates the status of the staging area and the right-hand column indicates the status of the working tree. So for example in that output, the README file is modified in the working directory but not yet staged, while the lib/simplegit.rb file is modified and staged. The Rakefile was modified, staged and then modified again, so there are changes to it that are both staged and unstaged.

###5. Ignoring chnages
To ignore changes, use .gitignore file. Here is an example .gitignore file:

	$ cat .gitignore
	*.[oa]
	*~
	
The first line tells Git to ignore any files ending in “.o” or “.a” — object and archive files that may be the product of building your code. The second line tells Git to ignore all files whose names end with a tilde (~), which is used by many text editors such as Emacs to mark temporary files. You may also include a log, tmp, or pid directory; automatically generated documentation; and so on. Setting up a .gitignore file for your new repository before you get going is generally a good idea so you don’t accidentally commit files that you really don’t want in your Git repository.

The rules for the patterns you can put in the .gitignore file are as follows:

* Blank lines or lines starting with # are ignored.
* Standard glob patterns work, and will be applied recursively throughout the entire working tree.
* You can start patterns with a forward slash (/) to avoid recursivity.
* You can end patterns with a forward slash (/) to specify a directory.
* You can negate a pattern by starting it with an exclamation point (!).

Glob patterns are like simplified regular expressions that shells use. An asterisk (\*) matches zero or more characters; [abc] matches any character inside the brackets (in this case a, b, or c); a question mark (?) matches a single character; and brackets enclosing characters separated by a hyphen ([0-9]) matches any character between them (in this case 0 through 9). You can also use two asterisks to match nested directories; a/**/z would match a/z, a/b/z, a/b/c/z, and so on.
Here is another example .gitignore file:

	# ignore all .a files
	*.a

	# but do track lib.a, even though you're ignoring .a files above
	!lib.a

	# only ignore the TODO file in the current directory, not subdir/TODO
	/TODO

	# ignore all files in the build/ directory
	build/

	# ignore doc/notes.txt, but not doc/server/arch.txt
	doc/*.txt

	# ignore all .pdf files in the doc/ directory and any of its subdirectories
	doc/**/*.pdf

Tips: GitHub maintains a fairly comprehensive list of good .gitignore file examples for dozens of projects and languages at [https://github.com/github/gitignore](https://github.com/github/gitignore) if you want a starting point for your project.

###6. Viewing Your Staged and Unstaged Changes
To see what you’ve changed but not yet staged, type git diff with no other arguments:

	$ git diff
	
If you want to see what you’ve staged that will go into your next commit, you can use git diff --staged. This command compares your staged changes to your last commit:

	$ git diff --staged
	diff --git a/README b/README
	new file mode 100644
	index 0000000..03902a1
	--- /dev/null
	+++ b/README
	@@ -0,0 +1 @@
	+My Project

###7. Commiting your changes

	$ git commit



--
Reference: [https://git-scm.com/book/en/v2/Getting-Started-Getting-Help](https://git-scm.com/book/en/v2/Getting-Started-Getting-Help)