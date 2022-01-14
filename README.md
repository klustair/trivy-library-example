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
package main

import (
	"context"
	"fmt"
	"os"
	"time"

	dbTypes "github.com/aquasecurity/trivy-db/pkg/types"
	"github.com/aquasecurity/trivy/pkg/commands/option"
	"github.com/klustair/trivy/pkg/commands/artifact"
)

func main() {
	fmt.Println("Use trivy as a library")

	// Create a context with a timeout of 15 seconds
	ctx, cancel := context.WithTimeout(context.Background(), 15*time.Second)
	defer cancel()

	// Define a file where to store the results of the scan
	//file, _ := os.Create("/tmp/trivy.txt")

	// Define the options for the scan
	opt := artifact.Option{
		GlobalOption: option.GlobalOption{
			Context:    nil,
			Logger:     nil,
			AppVersion: "1",
			Quiet:      false,
			Debug:      true,
			CacheDir:   "/tmp/trivy",
		},
		ArtifactOption: option.ArtifactOption{
			Input:      "",
			Timeout:    time.Duration(15 * time.Second),
			ClearCache: false,

			SkipDirs:  []string{},
			SkipFiles: []string{},

			Target: "node:latest",
		},
		DBOption: option.DBOption{
			Reset:          false,
			DownloadDBOnly: false,
			SkipDBUpdate:   false,
			Light:          false,
			NoProgress:     false,
		},
		ImageOption: option.ImageOption{
			ScanRemovedPkgs: false,
			ListAllPkgs:     false,
		},
		ReportOption: option.ReportOption{
			Format:   "table",
			Template: "",

			IgnoreFile:    "",
			IgnoreUnfixed: false,
			ExitCode:      0,
			IgnorePolicy:  "",

			VulnType: []string{
				"os",
				"library",
			},
			SecurityChecks: []string{
				"vuln",
				//"config",
			},
			Output: os.Stdout, //nil, //os.Stdout, //file
			Severities: []dbTypes.Severity{
				0,
				1,
				2,
				3,
				4,
			},
		},
		CacheOption: option.CacheOption{
			CacheBackend: "fs",
		},
		ConfigOption: option.ConfigOption{
			FilePatterns:       nil,
			IncludeNonFailures: false,
			SkipPolicyUpdate:   true,
			Trace:              false,

			PolicyPaths:      []string{},
			DataPaths:        []string{},
			PolicyNamespaces: []string{},
		},
		DisabledAnalyzers: nil,
	}

	// Optional: Change the image
	opt.ArtifactOption.Target = "alpine:latest"

	// Run the scan
	report, err := artifact.ImageRunLib(ctx, opt)

	// Do what ever you want with the report
	fmt.Printf("%+v\n", report)
	if err != nil {
		fmt.Println(err)
	}

}

```