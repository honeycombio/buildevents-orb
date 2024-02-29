# buildevents-orb

[![OSS Lifecycle](https://img.shields.io/osslifecycle/honeycombio/buildevents-orb?color=success)](https://github.com/honeycombio/home/blob/main/honeycomb-oss-lifecycle-and-practices.md)

Orb for CircleCI to instrument builds with Honeycomb

## Overview

The Buildevents Orb is a convenient package for using Honeycomb's `buildevents` to instrument your CircleCI build. For the full documentation see the [`buildevents` repo](https://github.com/honeycombio/buildevents) itself.

The orb requires some environment variables to be set. At minimum:
* `BUILDEVENT_APIKEY` - your Honeycomb API key, available on the [accounts](https://ui.honeycomb.io/account) page
* `BUILDEVENT_CIRCLE_API_TOKEN` - a Personal CircleCI API Token, available from the [api tokens section](https://circleci.com/account/api) of your CircleCI account page

Additional optional environment variables can be found in the [`buildevents`](https://github.com/honeycombio/buildevents) documentation.

The Buildevents Orb is hosted by Honeycomb; to enable your CircleCI config to use 3rd party orbs you must enable third party orbs in the Organization Security settings within CirlceCI. For more information, please visit the [CircleCI documentation on orb use](https://circleci.com/docs/2.0/orbs-faq/#using-uncertified-orbs).

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

## Gotchas

### Pipeline Parameters
Pipeline parameters are not available for use inside orbs, which means they cannot be used in steps that are wrapped with build events. A common error in CircleCI includes text like `unknown variable`.
To use pipeline parameters, pass them through as command parameters.

```yaml
commands:
  command-using-pipeline-parameter:
    description: This command uses a pipeline parameter.
    parameters:
      working_directory:
        type: string
    steps:
      - run: echo "The working directory is named '<< parameters.working_directory >>'"

test:
  steps:
    - buildevents/with_job_span:
        steps:
          - command-using-pipeline-parameter:
              working_directory: << pipeline.parameters.working_directory >>
```

### Shell Escaping in `add_context`

Like most orbs, this one interpolates your provides `parameters` into shell scripts embedded in config YAML. This is extremely powerful but can lead to some tricky shell escaping situations, especially when using `add_context`. `add_context` is implemented as a command that writes key-value pairs to a file, which are later added to the active span when it's sent. 

```bash
    echo << parameters.field_name >>=\"<< parameters.field_value >>\" >> context_fields.lgfmt
```

The above construction allows for dynamic usecases, such as using command substitution to provide values:

```yaml
- buildevents/add_context:
    field_name: arch
    field_value: $(uname -m)
```

However, it also means that you need to be careful about the values provided, as multiline strings or quotes can break this `echo` command.

```yaml
- buildevents/add_context:
    field_name: broken
    field_value: will break\"
- buildevents/add_context:
    field_name: evenmorebroken
    field_value: |-
        this will echo\"
        rm -rf / # oh no now we're executing commands
        echo hacked="yourself" >> /tmp/buildevents/extra_fields.lgfmt
```
         
**Take extra caution if using dynamic values to ensure the fields are written as you expect.** Providing a parameter of `verbose: true` will echo the field name and value. You can also add an extra command to `cat /tmp/buildevents/extra_fields.lgfmt` to see the exact contents of the fields file that is generated. Remember, your CI pipeline is a bunch of arbitrary shell commands—please make sure they are safe.
