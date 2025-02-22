# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## v1.1.6

### Fixed
- Quote `Args` value on `AnsibleAdhocCmd`'s `String` method #91
- On default executor, set all parent process environment variables to `cmd.Env` when a custom env vars is defined #94
- Fix parsing of long lines in output #101

### Added
- `ExecutorTimeMeasurement` is a decorator defined on `github.com/apenella/go-ansible/pkg/execute`, that measures the duration of an execution, it receives an `Executor` which is measured the execution time #92
- Add `unreachable` state on task play results struct `AnsiblePlaybookJSONResultsPlayTaskHostsItem` #100

### Chanded
- `MockExecute` uses `github.com/stretchr/testify/mock` #92
- Examples' name are prefixed by `ansibleplaybook` or `ansibleadhoc`

### Removed
- `DefaultExecutor` does not measures the execution duration anymore. Instead of it, `ExecutorTimeMeasurement` must be used #92

## v1.1.5

### Added
- New function `WithEnvVar` on `github.com/apenella/go-ansible/pkg/execute` package that adds environment variables to `DefaultExecutor` command.

### Fixed
- Include missing attributes on `AnsiblePlaybookJSONResultsPlayTaskHostsItem`. Those attributes are `cmd`, `skipped`, `skip_reason`, `failed`, and `failed_when_result`

## v1.1.4

### Added
- New function `ParseJSONResultsStream` on `"github.com/apenella/go-ansible/pkg/stdoutcallback/results"` that allow to parse ansible stdout json output as a stream. That method supports to parse json output when multiple playbooks are executed.

## v1.1.3

### Fixed
- New attribute `ExtraVarsFile` on `AnsiblePlaybookOptions` that allows to use YAML/JSON files to define extra-vars
- New attribute `ExtraVarsFile` on `AnsibleAdhocOptions` that allows to use YAML/JSON files to define extra-vars

## v1.1.2

### Fixed
- Include `stdout` and `stdout_lines` to `AnsiblePlaybookJSONResultsPlayTaskHostsItem`
- Include `stderr` and `stderr_lines` to `AnsiblePlaybookJSONResultsPlayTaskHostsItem`

## v1.1.1

### Changed
- update dependency package github.com/apenella/go-common-utils/error
- update dependency package github.com/apenella/go-common-utils/data

### Fixed
- Fixed(#57) typos and language mistakes on Readme file
- Fixed(#64) update `Msg` type on `AnsiblePlaybookJSONResultsPlayTaskHostsItem` from `string` to `interface{}`

## v1.1.0

### Added
- support for stdin on `DefaultExecute` Execute method

## v1.0.0

### Added
- Included `ansible-playbook` version `2.10.6` options on `AnsiblePlaybookOptions`
- Included `github.com/apenella/go-ansible/pkg/adhoc` package to interact to `ansible` adhoc command
- New function type `ExecuteOptions` to provide options to executor instances
- New `DefaultExecute` constructor `NewDefaultExecute` that accepts a list of `ExecuteOptions`
- New component to customize ansible output lines. That component is named *transformer*
- Include a bunch of transformers that can be already used:
    - Prepend(string): Prepends and string to the output line
    - Append(string): Appends and string to the output line
    - LogFormat(string): Prepends date time to the output line
    - IgnoreMessage([]string): Ignores the output lines based on input strings
- New private method `output` on `results` package to manage how to write the output lines and that can be used by any `StdoutCallbackResultsFunc`

### Changed
- **BREAKING CHANGE**: `ansibler` has been restructured and splitted to multiple packages:
  - Type `AnsiblePlaybookConnectionOptions` is renamed to `AnsibleConnectionOptions` and placed to `github.com/apenella/go-ansible/pkg/options`
  - Type `AnsiblePlaybookPrivilegeEscalationOptions` is renamed to `AnsiblePrivilegeEscalationOptions` and placed to `github.com/apenella/go-ansible/pkg/options`
  - All constants regarding connection options and privileged escalations options has been placed to `github.com/apenella/go-ansible/pkg/options`
  - `AnsiblePlaybookCmd` and `AnsiblePlaybookOptions` has been placed to `github.com/apenella/go-ansible/pkg/playbook`
  - All constants regarding ansible-playbook command interaction has been placed to `github.com/apenella/go-ansible/pkg/playbook`
- **BREAKING CHANGE**: `Playbook` attribute on `AnsiblePlaybookCmd` has been replaced to `Playbooks` attribut which accept multiple playbooks to be run
- **BREAKING CHANGE**: `Executor` interface has been moved from `ansibler` package to `github.com/apenella/go-ansible/pkg/execute` package
- **BREAKING CHANGE**: `Executor` interface is changed to `Execute(ctx context.Context, command []string, resultsFunc stdoutcallback.StdoutCallbackResultsFunc, options ...ExecuteOptions) error`
- **BREAKING CHANGE**: `DefaultExecute` has been updated to use options pattern design, and includes a bunch of `WithXXX` methods to set its attributes
- **BREAKING CHANGE**: `StdoutCallbackResultsFunc` signature has been updated to `func(context.Context, io.Reader, io.Writer, ...results.TransformerFunc) error`. Prefix argument has been removed and a list of transformers could be passed to the function
- `DefaultStdoutCallbackResults` and `JSONStdoutCallbackResults` prepares default transformers for default output an calls `output`, instead of managing the output by its own

### Removed
- **BREAKING CHANGE**: Remove `ExecPrefix` from `AnsiblePlaybookCmd`
- **BREAKING CHANGE**: Remove `CmdRunDir` from `AnsiblePlaybookCmd`
- **BREAKING CHANGE**: Remove `Writer` from `AnsiblePlaybookCmd`
- **BREAKING CHANGE**: Remove `ResultsFunc` from `DefaultExecute`
- **BREAKING CHANGE**: Remove `Prefix` from `DefaultExecute`. Prefix is not manatory any more and could be added using the `Prepend` transformer.
- `skipLine` method has been removed. Replaced by `IgnoreMessage` transformer

## v0.8.0
### Added
- Include attribute CmdRunDir on AnsiblePlaybookCmd which defines the playbook run directory
- Include attribute CmdRunDir on DefaultExecutor

## v0.7.1
### Fixed
- fix to do not use a multireader for stdout and stderr on DefaultExecutor

## v0.7.0
### Added
- Add Binary attribute to AnsiblePlaybookCmd
- Add VaultPasswordFile to AnsiblePlaybookOptions

## v0.6.1
### Changed
- On error, write to output writer either stdout and stderr

### Fixed
- Quote extravars when return command as string

## v0.6.0
### Added
- New method CheckStats on results package that validates AnsiblePlaybookJSONResults stats

### Changed
- __JSONStdoutCallbackResults__ on results package does not manipulates ansible JSON output, writes output as is into a writer
- __JSONParser__ on results package has changed its signature to _JSONParse(data []byte) (*AnsiblePlaybookJSONResults, error)_
- __simple-ansibleplaybook-json__ example has been modified to use a custom executor to manipulate the JSON output.
- Use github.com/apenella/go-common-utils/error to manage errors

## v0.5.1
### Fixed
- [#12](https://github.com/apenella/go-ansible/pull/12): Fix the concurrency issue in the defaultExecute.go

## v0.5.0
### Added
- Changelog based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
- New package to manage `ansible-playbook` output
- Manage Json stdout callback results
- DefaultExecutor includes an error managemnt depending on `ansible-playbook` exit code
- Use go mod to manage dependencies

## v0.4.1
### Added
- start using go mod as dependencies manager

### Fixed
- fix bug ansible always showing error " error: unrecognized arguments" when use private key 

## v0.4.0
### Added
- Include privilege escalation options

## v0.3.0
### Added
- AnsiblePlaybookCmd has a Write attribute, which must be defined by user.

## v0.2.0
### Added
- Use package github.com/apenella/go-common-utils

### Changed
- Change package name to ansibler

