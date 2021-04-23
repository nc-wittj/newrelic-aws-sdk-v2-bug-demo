# NewRelic AWS SDK V2 Integration Incompatibility
Reported in [GitHub Issue #288](https://github.com/newrelic/go-agent/issues/288).

## To Reproduce:
1. Use Go 1.16 (it should also happen with other Go versions)
2. Clone this repo
3. Run `go get github.com/newrelic/go-agent/v3/integrations/nrawssdk-v2`
4. See this error:
   ```
   # github.com/newrelic/go-agent/v3/integrations/nrawssdk-v2
   ../../go/pkg/mod/github.com/newrelic/go-agent/v3/integrations/nrawssdk-v2@v1.0.0/nrawssdk.go:12:24: undefined: aws.Request
   ../../go/pkg/mod/github.com/newrelic/go-agent/v3/integrations/nrawssdk-v2@v1.0.0/nrawssdk.go:23:22: undefined: aws.Request
   ../../go/pkg/mod/github.com/newrelic/go-agent/v3/integrations/nrawssdk-v2@v1.0.0/nrawssdk.go:76:35: undefined: aws.Handlers
   ../../go/pkg/mod/github.com/newrelic/go-agent/v3/integrations/nrawssdk-v2@v1.0.0/nrawssdk.go:77:30: undefined: aws.NamedHandler
   ../../go/pkg/mod/github.com/newrelic/go-agent/v3/integrations/nrawssdk-v2@v1.0.0/nrawssdk.go:81:29: undefined: aws.NamedHandler
   ```

## Why is this happening?
It looks like the current version of `v3/integrations/nrawssdk-v2` [depends on](https://github.com/newrelic/go-agent/blob/198c033a21ef66200032d444e1be06b354175516/v3/integrations/nrawssdk-v2/go.mod#L10) a prerelease version of `github.com/aws/aws-sdk-go-v2`.
There was a breaking API change to get to `github.com/aws/aws-sdk-go-v2@v1.0.0`.
Thus, `v3/integrations/nrawssdk-v2` cannot be used by any projects that use a `v1.0.0`+ release of the AWS V2 SDK.
