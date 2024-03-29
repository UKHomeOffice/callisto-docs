# Mono repo vs poly repo

## Summary

### Issue
Our project involves developing two major categories of software:

  * Front-end GUIs
  * Back-end servers

When we develop, our source code management (SCM) version control system (VCS) is git.

We need to choose how we use git to organize our code.

The top-level choice is to organize as a "monorepo" or "polyrepo" or "hybrid":

  * Monorepo means we put all pieces into one big repo
  * Polyrepo means we put each piece in its own repo
  * Hybrid means some mix of monorepo and polyrepo

For more please see https://github.com/joelparkerhenderson/monorepo-vs-polyrepo


### Decision
DONE

### Status

## Details

### Assumptions

### Constraints

### Options
* Polyrepo
* Monorepo
* Hybridrepo

### Selected Option
Polyrepo

### Implications

## Related

### Related decisions

### Related requirements

## Notes
Monorepo when an organization/team/project is relatively small, and rapid iteration is higher priority than sustaining stability.

Polyrepo when an organization/team/project is relatively large, and sustaining stability is higher priority than rapid iteration.