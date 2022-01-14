# trivy-library-example
Use trivy as a library

## Installation
```
go get github.com/aquasecurity/trivy/pkg/commands/option
go get github.com/aquasecurity/trivy-db/pkg/types
go get github.com/klustair/trivy/pkg/commands/artifact@lib

```

## Usage
```go
	opt := artifact.Option{
        ...
    }
```