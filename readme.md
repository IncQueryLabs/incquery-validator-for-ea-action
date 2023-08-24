# GitHub Action to deploy and run IncQuery Validator for Enterprise Architect

[![IncQuery Validator for Enterprise Architect](https://github.com/IncQueryLabs/incquery-validator-for-ea-action/actions/workflows/IncQueryValidatorForEAExample.yml/badge.svg)](https://github.com/IncQueryLabs/incquery-validator-for-ea-action/actions/workflows/IncQueryValidatorForEAExample.yml)

This repository contains a GitHub Action for deploying and running *IncQuery Validator for Enterprise Architect*. The repository also contains a workflow example demonstrating how to use the action.

## What does this action do?

This action downloads and installs *IncQuery Validator for Enterprise Architect* (if not already installed on the GitHub Runner), then executes a model analysis with the specified analysis suite on the specified Enterprise Architect model. The result of the analysis will be available in the artifacts of the workflow. If the workflow is triggered on a branch which has a pull request, then a comment is posted about the summary of the analysis. A basic pass-fail criteria is also configurable.

## About IncQuery Validator

<p align="center">
  <img height=250 src="./images/IncQueryValidator.png">
</p>

IncQuery Validator is a powerful tool that provides Automated Quality Gates as an integral part of your next-generation MBSE workflow. Running either as a stand-alone desktop application, or as a server-side automated pipeline, it can generate rich validation reports including a high-level Quality Score, as well as detailed rule violation breakdowns, that give information on the general context, severity, and impact of each quality issue.

For more information visit [https://incquery.io/validator](https://incquery.io/validator).

# Usage

### Requirements

In case of GitHub hosted runner:
- windows-2022 (or newer) runner
- Valid license
- Credentials to access the *IncQuery Validator for Enterprise Architect* release

In case of self hosted runner:
- Runner hosted on Windows 10 x64 1803 April 2018 Update or newer
- Valid license
- Credentials to access the *IncQuery Validator for Enterprise Architect* release or the Validator being pre-installed on the runner

### Required secrets

The following secrets need to be configured in the repository:
- If the Validator is not pre-installed on the runnerincquery_username, incquery_password to access the Validator releases
- incquery_ea_validator_license : contents of the license file as-is

### Example workflow

```yaml
- name: Run validation
  uses: IncQueryLabs/incquery-validator-for-ea-action@v1
  with:
    model_file_path:   example.qeax
    analysis_suite:    "SAIC Digital Engineering Validation"
    incquery_username: "${{ secrets.incquery_username }}"
    incquery_password: "${{ secrets.incquery_password }}"
    license:           "${{ secrets.incquery_ea_validator_license }}"
    comment_on_pr:     true
    fail_on:           warning
```

See [IncQueryValidatorForEAExample.yml](.github/workflows/IncQueryValidatorForEAExample.yml) for an example of a full workflow.

### Input parameters

#### model_file_path
Path to the Enterprise Architect project. EAP, EAPX, QEA, QEAX files are supported. Shortcuts (and connection strings) to remote servers are also supported, but in this case make sure that the runner can reach the server.

This parameter is mandatory.

#### analysis_suite
Name of the analysis suite, same as what appears in the addin's *Validate model* menu.

This parameter is mandatory.

#### incquery_username, incquery_password

Username and password to *artifacts.incquery.io*. If the runner does not have the Validator already installed, then these parameters are required.

#### license

Contents of the license file as-is. The ValidatorEA and ValidatorEACI license features are required.

This parameter is mandatory.

#### comment_on_pr

If set to true and there is a pull request for the branch then the action will create a comment with the summary of the analysis. By default this is disabled.

#### fail_on

This parameter sets the pass-fail quality gate.

The following table lists the possible values.

| Value | Description |
| ----- | ----------- |
| none  | The action will not trigger a build failure, regardless of the analysis result. |
| debug | The action will fail if there is any kind of issues detected. |
| info  | The action will fail if there are *info* or more severe issues detected. |
| warning | The action will fail if there are *warnings* or more severe issues detected. |
| error | The action will fail if there are *error* or more severe issues detected. |
| fatal | The action only fails if there are *fatal errors* detected. |
