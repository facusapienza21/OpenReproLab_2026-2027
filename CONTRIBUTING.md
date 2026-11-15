# Contributing to OpenReproLab_2026-2027

> [!IMPORTANT]
> For organizational simplicity, we currently do not accept contributions from people not belonging to OpenReproLab.

## Architecture of the CI

As described in [the syllabus](./README.md), OpenReproLab students work on labs, submit their work to an online Git-hosting platform (namely Github or Gricad's Gitlab). This platform then checks the students' work using CI scripts (CI = continuous integration).

The CI code for a given lab (say lab01) should be packaged in a python script named `ciscripts/lab01.py`. Running `python ciscripts/lab01.py` from anywhere in the repository should run the lab's verifications, print (to `stdout`) the textual output meant for the student, and return 0 if all the lab's checks were passed successfully, and a non-zero integer otherwise. Note that the script `ciscripts/lab01.py` can call custom Python modules but these custom modules should be named ``ciscripts/lab01*.py``.

These CI scripts:

 - Can be run locally to test one's current work before committing.

 - Will be run on Github or Gricad's Gitlab when students push work on their branch.

> [!TIP]
> As an OpenReproLab developer, create a branch named `user/${your_user_name}` to test the CI on the platform if needed.

## Procedure to add a new OpenReproLab student

 - Retrieve the login name of the student on Github or on Gricad's Gitlab.

 - Add the student as a contributor to the corresponding repository. On Gitlab, add the student as developer and not as a maintainer. Developers have sufficient permissions to complete all labs.

 - Create a branch named `user/${login_name}` for that user.

 - In the repository's settings, add a rule to prevent anyone else but said student to modify their branch. This is possible on Github but more limited on Gricad.

## Pushing to both Github and Gitlab

Do make sure to always push your changes on the main branch to both Github and Gitlab.

For that, we recommend setting both remotes as follows:

```sh
git remote add github git@github.com:ige-PING/OpenReproLab_2026-2027.git
git remote add gricad git@gricad-gitlab.univ-grenoble-alpes.fr:ping/openreprolab_2026-2027.git
```

and to use the following convenience shell function when pushing:

```sh
function git-push-current-branch-to-gricad-and-github
{
    local branch=$(git branch | grep -E "^\*" | cut -w -f 2)
    echo "Local branch is \"$branch\""
    for remote in github gricad; do
        if [ $(git remote | grep -cE "^$remote\$") -eq 1 ]; then
            echo "Remote \"$remote\" exists... pushing..."
            git push $remote $branch
        else
            echo "Remote \"$remote\" does not exist!"
        fi
    done
}
```
