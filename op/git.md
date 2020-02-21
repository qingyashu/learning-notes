# Git Hooks
[Atlassian Git HooksTutorial](https://www.atlassian.com/git/tutorials/git-hooks)
- Hooks resides in the `.git/hooks` directory. To install a hook, simply remove `.sample` extension, and add `+x` permissions for the file. 
- To use a different language for a hook script, just change the first line interpreter. 

- 6 most useful local hooks:
  - `pre-commit`
  - `prepare-commit-msg`
  - `commit-msg`
  - `post-commit`
  - `post-checkout`
  - `pre-rebase`
All of the `pre-` hooks allow alter the action that's about the take place, while `post-` hooks are used only for notifications. 

- 3 most useful server-side hooks:
  - `pre-receive`
  - `update`
  - `post-receive`
All of the hooks let react to different stages of the `git push` process. 