# OpalOPC Scan Action

This action runs a OpalOPC vulnerability scan against a target server. On completion, an HTML and a SARIF report is generated containing information about the vulnerabilities found, where
they were located, additional information about the vulnerability and links to our learning resources with suggestions on how to fix them.

## About OpalOPC

- [OpalOPC](https://opalopc.com/) is a OPC UA security scanner.
- It is designed to be run manually by security testers, or via a CI/CD pipeline by developers
- It checks your OPC UA server for [various security issues](https://opalopc.com/docs/category/plugins) that are likely to interest you during software development, as well as attackers during a cyberattack.

For full documentation on using OpalOPC, please consult the [Documentation](https://opalopc.com/docs/).

## Inputs

## `target-url`

**Required** The full URL (including scheme) of the server to scan.

## `license-key`

**Required** OpalOPC license key.

## `output-base-filename`

**Optional** The base filename used for the scan report. This will be stored in the GITHUB_WORKSPACE (/github/workspace) directory. The resulting reports have suffixes `.html` and `.sarif`.

**Default** `opalopc-report`

## Examples

Below are some examples of how to use the action by running a OpalOPC scan against our very own [Practice target](https://opalopc.com/docs/get-started/test-drive) site. This is a deliberately
vulnerable OPC UA server setup for testing vulnerability scanners.

### Basic Usage

The basic usage is to scan and make the results available for download:

```yaml
steps:
  - name: Run OpalOPC Action Step
    uses: Molemmat-Oy/opalopc-scan-action@main
    with:
      target-url: 'opc.tcp://scanme.opalopc.com:53530'
      license-key: ${{ secrets.OPALOPC_LICENSE_KEY }}

  - name: Store scan reports
    uses: actions/upload-artifact@v4
    with:
      name: opalopc-reports
      path: opalopc-report.*
```

## Integrate with Code Scanning

You can integrate OpalOPC with GitHub Code Scanning by uploading SARIF scan reports to it. This lets you display the results in GitHub, or configure webhooks that listen to code scanning activity in your repository.

See [Uploading a SARIF file to GitHub](https://docs.github.com/en/code-security/code-scanning/integrating-with-code-scanning/uploading-a-sarif-file-to-github#uploading-a-code-scanning-analysis-with-github-actions).
