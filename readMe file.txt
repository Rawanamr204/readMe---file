(Git Config)
(Description)
You can query/set/replace/unset options with this command. The name is actually the section and the key separated by a dot, and the value will be escaped.
Multiple lines can be added to an option by using the --add option. If you want to update or unset an option which can occur on multiple lines, a value-pattern (which is an extended regular expression, unless the --fixed-value option is given) needs to be given. Only the existing values that match the pattern are updated or unset. If you want to handle the lines that do not match the pattern, just prepend a single exclamation mark in front (see also EXAMPLES), but note that this only works when the --fixed-value option is not in use.
The --type=<type> option instructs git config to ensure that incoming and outgoing values are canonicalize-able under the given <type>. If no --type=<type> is given, no canonicalization will be performed. Callers may unset an existing --type specifier with --no-type.When writing, the new value is written to the repository local configuration file by default, and options --system, --global, --worktree, --file <filename> can be used to tell the command to write to that location (you can say --local but that is the default).
(Options)
--replace-all
Default behavior is to replace at most one line. This replaces all lines matching the key (and optionally the value-pattern).

--add
Adds a new line to the option without altering any existing values. This is the same as providing ^$ as the value-pattern in --replace-all.

--get
Get the value for a given key (optionally filtered by a regex matching the value). Returns error code 1 if the key was not found and the last value if multiple key values were found.

--get-all
Like get, but returns all values for a multi-valued key.

--get-regexp
Like --get-all, but interprets the name as a regular expression and writes out the key names. Regular expression matching is currently case-sensitive and done against a canonicalized version of the key in which section and variable names are lowercased, but subsection names are not.
(Git init)
(Description)
This command creates an empty Git repository - basically a .git directory with subdirectories for objects, refs/heads, refs/tags, and template files. An initial branch without any commits will be created (see the --initial-branch option below for its name).

If the $GIT_DIR environment variable is set then it specifies a path to use instead of ./.git for the base of the repository.

If the object storage directory is specified via the $GIT_OBJECT_DIRECTORY environment variable then the sha1 directories are created underneath - otherwise the default $GIT_DIR/objects directory is used.

Running git init in an existing repository is safe. It will not overwrite things that are already there. The primary reason for rerunning git init is to pick up newly added templates (or to move the repository to another place if --separate-git-dir is given).

(OPTIONS)
-q
--quiet
Only print error and warning messages; all other output will be suppressed.

--bare
Create a bare repository. If GIT_DIR environment is not set, it is set to the current working directory.

--object-format=<format>
Specify the given object format (hash algorithm) for the repository. The valid values are sha1 and (if enabled) sha256. sha1 is the default.

Note: At present, there is no interoperability between SHA-256 repositories and SHA-1 repositories.

Historically, we warned that SHA-256 repositories may later need backward incompatible changes when we introduce such interoperability features. Today, we only expect compatible changes. Furthermore, if such changes prove to be necessary, it can be expected that SHA-256 repositories created with today’s Git will be usable by future versions of Git without data loss.

--template=<template-directory>
Specify the directory from which templates will be used. (See the "TEMPLATE DIRECTORY" section below.)

--separate-git-dir=<git-dir>
Instead of initializing the repository as a directory to either $GIT_DIR or ./.git/, create a text file there containing the path to the actual repository. This file acts as filesystem-agnostic Git symbolic link to the repository.

If this is reinitialization, the repository will be moved to the specified path.

-b <branch-name>
--initial-branch=<branch-name>
Use the specified name for the initial branch in the newly created repository. If not specified, fall back to the default name (currently master, but this is subject to change in the future; the name can be customized via the init.defaultBranch configuration variable).

--shared[=(false|true|umask|group|all|world|everybody|<perm>)]
Specify that the Git repository is to be shared amongst several users. This allows users belonging to the same group to push into that repository. When specified, the config variable "core.sharedRepository" is set so that files and directories under $GIT_DIR are created with the requested permissions. When not specified, Git will use permissions reported by umask(2).

(Git Merge)
(Description)
Incorporates changes from the named commits (since the time their histories diverged from the current branch) into the current branch. This command is used by git pull to incorporate changes from another repository and can be used by hand to merge changes from one branch into another.
Assume the following history exists and the current branch is "master":

	  A---B---C topic
	 /
    D---E---F---G master
Then "git merge topic" will replay the changes made on the topic branch since it diverged from master (i.e., E) until its current commit (C) on top of master, and record the result in a new commit along with the nof the two parent commits and a log message from the user describing the changes. Before the operation, ORIG_HEAD is set to the tip of the current branch (C).

	  A---B---C topic
	 /         \
    D---E---F---G---H master
OPTIONS
--commit
--no-commit
Perform the merge and commit the result. This option can be used to override --no-commit.

With --no-commit perform the merge and stop just before creating a merge commit, to give the user a chance to inspect and further tweak the merge result before committing.

Note that fast-forwardupdates do not create a merge commit and therefore there is no way to stop those merges with --no-commit. Thus, if you want to ensure your branch is not changed or updated by the merge command, use --no-ff with --no-commit.

--edit
-e--no-edit
Invoke an editor before committing successful mechanical merge to further edit the auto-generated merge message, so that the user can explain and justify the merge. The --no-edit option can be used to accept the auto-generated message (this is generally discouraged). The --edit (or -e) option is still useful if you are giving a draft message with the -m option from the command line and want to edit it in the edito.
(Git Remote)
DESCRIPTION
Manage the set of repositories ("remotes") whose branches you track.

OPTIONS
-v
--verbose
Be a little more verbose and show remote url after name. For promisor remotes, also show which filter (blob:none etc.) are configured.NOTE: This must be placed between remote and subcommand.

COMMANDS
add
Add a remote named <name> for the repository at <URL>. The command git fetch <name> can then be used to create and update remote-tracking branches <name>/<branch>.

With -f option, git fetch <name> is run immediately after the remote information is set up.

With --tagsoption, git fetch <name> imports every tag from the remote repository.

With --no-tags option, git fetch <name> does not import tags from the remote repository.

By default, only tags on fetched branches are imported (see git-fetch(1).