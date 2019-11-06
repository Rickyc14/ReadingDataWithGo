# ReadingDataWithGo
Reading data from a CSV file in Go.


```go
package main

import (
  "encoding/csv"
  "flag"
  "fmt"
  "os"
)
/* go build, then dataproj -csv="china.csv" if using cmd prompt */
func main() {
  csvFilename := flag.String("csv", "china.csv", "China's GDP per capita.")
  flag.Parse()

  file, err := os.Open(*csvFilename)
  if err != nil {
    exit(fmt.Sprintf("Failed to open the CSV file: %s\n", *csvFilename))
  }
  r := csv.NewReader(file)
  lines, err := r.ReadAll()
  if err != nil {
    exit("Failed to parse the provided CSV file.")
  }
  china := parseLines(lines)

  for _, c := range china {
    fmt.Printf("%s | %s \n", c.year, c.gdp)
  }
}

func parseLines(lines [][]string) []China {
  ret := make([]China, len(lines))
  for i, line := range lines {
    ret[i] = China{
      gdp: line[3],
      year: line[4],
    }
  }
  return ret
}

type China struct {
  gdp string
  year string
}

func exit(msg string) {
  fmt.Println(msg)
  os.Exit(1)
}
```
