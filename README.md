[![GitHub
release](https://img.shields.io/github/release/docwhat/chronic.svg)](https://github.com/docwhat/chronic/releases)
[![Build
Status](https://travis-ci.org/docwhat/chronic.svg?branch=master)](https://travis-ci.org/docwhat/chronic)

chronic
=======

This is a [go](https://golang.org/) based version of the classic `cronic` or
`chronic` tool. The original [`chronic`](http://habilis.net/cronic/) was
written by [Chuck Houpt](http://habilis.net/chuck/).

If you've ever used `cron` to run jobs, you know that it sends an email for any
output generated by the command. This is why you see `crontabs` that look like
this:

    MAILTO=admin@example.com
    PATH=/usr/bin:/bin
    0 4 0 0 0 my-noisy-command -vF >/dev/null 2>&1

This is an anti-pattern:

-   If you failed to get the command line correct, then you'll end up getting
    an email.
-   If anything goes wrong, you've lost the output.
-   If you log to a file, then you have to rotate that log, even if 99% of the
    contents are useless.

With `chronic` you don't have to worry about that anymore!

    MAILTO=admin@example.com
    PATH=/usr/bin:/bin
    0 4 0 0 0 chronic my-noisy-command -vF

Now, you'll get no emails unless `my-noisy-command` returns a non-zero exit
code. If it does return a non-zero exit code, then you'll get an email that
looks like this:

    To: admin@example.com
    Subject: [CRON] chronic my-noisy-command -vF

    **** command ****
    [`bash` `-c` `echo "boo"; echo "emergency" 1>&2; exit 10`]

    **** stdout ****
    stdout: Normal output from the noisy command.
    stdout: Starting messages
    stdout: Normal stuff

    **** stderr ****
    stderr: Standard error output from the noisy command.
    stderr:
    stderr: [FATAL] Something went wrong!

    Exited with 10

Installation
------------

### Download binaries

The latest release is available on
[github.com/docwhat/chronic/releases](https://github.com/docwhat/chronic/releases).
You can download the binary for your architecture and OS there.

### Compile it yourself

You'll need to install a recent version of [Go](https://golang.org/) and set it
up. You can check the `.travis.yml` file to see what version of go I'm using.

\`\`~\~~ .sh \$ go get -u docwhat.org/chronic ~\~~

Developers
----------

While this project has a `Rakefile` it isn't needed for normal development; it
is only used for CI builds.

You can use the normal `go` tools to build and install it.
