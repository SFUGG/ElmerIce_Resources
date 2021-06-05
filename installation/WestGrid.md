---
layout   : default
title    : West Grid
parent   : Installation Instructions
nav_order: 4
---

# Using `Elmer/Ice` on Compute Canada Resources

You will need a Compute Canada account to be able to run these commands.
If you do not already have an account have a look [here](https://www.computecanada.ca/research-portal/account-management/apply-for-an-account/) on how to get one.

Luckily `Elmer` (with `Elmer/Ice`) is already installed on Compute Canada systems.
Therefore, all that needs to be done is load the "module".
Pressuring you've signed onto one of the clusters (e.g. Cedar), run:

```bash
module load gcc/9.3.0
module load elmerfem/9.0
```

and you will be all set to run `Elmer/Ice`. For more information about version of `Elmer` that are available, run:

```bash
module spider elmerfem
```
or see [here](https://docs.computecanada.ca/wiki/Available_software).
