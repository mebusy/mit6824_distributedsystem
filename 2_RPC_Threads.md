
# Lecture 2: RPC and Threads

[RPC packet](https://golang.org/pkg/net/rpc/)


- Race
    - lock
- Coordination
    - channel
        - more general-purpose
    - sync.Cond
        - Condition variables work well to alert goroutines that may (or may not) be waiting for something. 
    - WaitGroup
        - fairly special-purpose; it's only useful when waiting for a bunch of activities to complete
- Deadlock


## Crawled

[Exercise: Web Crawler](https://tour.golang.org/concurrency/10)

[crawler.go](https://pdos.csail.mit.edu/6.824/notes/crawler.go)

- challenge 
    - one of the jobs of the crawler is to remember the set of pages that is already crawled, or already even started a fetching. 
        - use a map
    - sometimes the hardest thing to solve is to know when the crawl is finished. 


- caution: the follow programe has a potential bug:
    - `log.Println(v)`   loop varialbe v captured by func literal

```go
package main

import (
    "log"
    "sync"
    "time"
)

func main() {
    // log.Println("hahaha")
    arr := []string{ "a","b", "c" }
    var done sync.WaitGroup
    for _, v := range(arr) {
        done.Add(1)
        go func() {
            defer done.Done()
            time.Sleep( time.Second )
            log.Println( v )
        }()
    }
    done.Wait()
}
```




