# AWS Restore Role Buildkite Plugin

A [Buildkite plugin](https://buildkite.com/docs/agent/plugins) to unset IAM Role environment variables after running the build command.

This is essentially the inverse of the [AWS Assume Role Plugin](https://github.com/cultureamp/aws-assume-role-buildkite-plugin), resetting the environment back to the default role for the build agent.

This plugin should come early in your plugin list due to [Buildkite's decision to run pre-command hooks and post-command hooks in the same order](https://github.com/buildkite/agent/issues/1646). Remember that assuming a role happens before the command and restoring a role happens _after_ the command.

## Example

```yml
steps:
  - command: bin/ci-aws-thing
    plugins:
      - franklin-ross/aws-restore-role#HEAD: ~
      # Without aws-restore-role, this plugin would use the default agent role
      # in the pre-command hook and the example-role in the post-command hook.
      - plugin-that-uses-aws-roles: ~
      - cultureamp/aws-assume-role#v0.1.0:
          role: "arn:aws:iam::123456789012:role/example-role"
```
