+++
title = "Runtime"
weight = 10
+++

Runtimes are minimal. They should be the absolute smallest amount of code to
plug in your main application to some kind of runtime environment.

Your runtime might be a command-line, http-daemon, queue consumer, background
cron job runner, or anything else that "starts" your program.

You should always define a `run.py` in the root level of a runtime component
and expect that this is where your software will be started.


