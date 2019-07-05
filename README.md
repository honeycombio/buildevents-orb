# buildevents-orb
Orb for CircleCI to instrument builds with Honeycomb

## Overview

The Buildevents Orb is a convenient package for using Honeycomb's `buildevents` to instrument your CircleCI build. For the full documentation see the [`buildevents` repo](https://github.com/honeycombio/buildevents) itself.

The orb requires some environment variables to be set. At minimum:
* `BUILDEVENT_APIKEY` - your Honeycomb API key, available on the [accounts](https://ui.honeycomb.io/account) page
* `BUILDEVENT_CIRCLE_API_TOKEN` - a Personal CircleCI API Token, available from the [api tokens section](https://circleci.com/account/api) of your CircleCI account page

Additional optional environment variables can be found in the [`buildevents`](https://github.com/honeycombio/buildevents) documentation.

## Use

The expected use of the Buildevents Orb adds two jobs to your workflow; the rest of the use is within existing jobs.

```
     [setup]  ----  [the rest of the build]
        \
          ----  [watch and finish]
```

**setup** Add a job that runs at the very beginning of the workflow that initializes the orb by downloading the `buildevents` binary and recording some starting information. This is the `start_trace` command.

**watch** The watch job should depend on setup and have no jobs depending on it. It will poll the CircleCI API to determine when the build has finished and its status. It will continue to run until all other jobs have finished (or are blocked). This is the `watch_build_and_finish` command.

**the rest of the build** Within each of your existing jobs there three other commands the orb supplies to be used.
* `with_job_span` should be the only command in your job's `steps` section. It starts a span that will record the length of this job. It has its own `steps` command, in which all the things you want this job to do reside.
* `add_context` is a way of recording additional fields you want to include in the job's span. Examples of useful additional context are parameters to the job and the size of artifacts.
* `berun` is a way to create a span for a specific command within a job. Similar to the native `run` command, this takes a name and a command to run. The duration and exit status of the command will be recorded in a span.
* `buildevents` the raw buildevents binary is available in the path as an alternative to the `berun` command. Doing so in a way that will create a span that will join the trace requires specific arguments: `buildevents cmd $CIRCLE_WORKFLOW_ID $BUILDEVENTS_SPAN_ID <name> -- <the command to run>`

If it's impossible to get a Personal CircleCI API Token, you can use the `finish` command instead of `watch_build_and_finish`. You should schedule a job to be run as close to the end of the build as possible. If the job does not run (because a previous job failed) you will not get a root span for the build's trace (though all previous spans will still exist).

## Examples

Several of the Honeycomb repositories use buildevents and you can take a look as working examples:
* [buildevents](https://github.com/honeycombio/buildevents/blob/master/.circleci/config.yml) builds itself using buildevents to instrument its own build.
* [samproxy](https://github.com/honeycombio/samproxy/blob/master/.circleci/config.yml)
* [honeytail](https://github.com/honeycombio/honeytail/blob/master/.circleci/config.yml)

