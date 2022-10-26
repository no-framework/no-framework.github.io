+++
title = "Modules"
weight = 10
+++

Modularity brings some benefits. You can modify, remove or replace a software
component without spiraling changes across the entire project. You can re-use
components in ways that were never before considered.

For example, if you define a module for a Postgres database connection, your project
should be able to connect to 50 different databases using the same code.
Secondly, your project should be able to also add a MongoDB component which can
also let your application connect to hundreds of databases at the same time.

This is easier said than done and requires decoupling from other components in
the software and only letting dependencies (eg, `import` statements) be formed
when there is a concrete logical need (instead of a convenience short-cut when
writing code).

You should be able to select the folder for a software component and run a `git
subtree-split` to extract it into its own stand-alone software package (with
minor work to add a new dependency file and adjust import names).


It means you have a tree structure like this:

```
    modular_provider_architecture
    ├── __init__.py
    ├── logic_module
    │   ├── __init__.py
    │   ├── logic.py
    │   └── provider.py
    └── module_runtime
        ├── __init__.py
        ├── provider.py
        ├── run.py
        └── runtime.py
```

Every top-level folder is a "module" in this architecture. This language may be
confusing in python so feel free to call it a component instead if that's less
confusing for your team.

The concept of a "submodule" is a violation of this architecture as it forces
heavily coupling between components. Keep it all on one folder level, and pick
good names. If you have too many folders it is a good indication that your
project has grown too large, and development would be easier if you created two
projects with 1 common package made from common software components (remember,
you can use `git subtree-split` to move code around to a new home and keep the
history)

Your software dependencies should look one-way:

```
 ┌────────────┐
 │ module     │
 │  _runtime  │
 └─────┬──────┘
       │
       │Depends on
       │ ("imports from")
       │
       │
       ▼
       ▼
 ┌─────▼──────┐
 │ logic      │
 │  _module   │
 └────────────┘
```

If you have any "cycles" in your architecture it is likely that you need to
make a simple package in-between to keep the common information (often this is
just an interface or some similar data)

In this architecture there are two major types of object instances:

1. "Infrastructure", it moves data around and processes it
2. "Data Transfer Objects" which represent state.

You can only create infrastructure inside a "Provider". We don't create new
classes "on-the-fly" inside DTOs or other infrastructure.
