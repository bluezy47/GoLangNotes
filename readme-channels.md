# - C O N C U R R E N C Y - 

    -   Work with Concurrecny in 
        -   Goroutines
        -   Channels
        -   Select
        -   Wait Groups

## Goroutines
- To run a task ( function ) in a seperate thead. This thread is known as `goroutine`
- To use goroutine we need to use
```go

    func helloWorld() {
        fmt.Println("Do something ... ");
        // some logic here ...
    }

    go func() {
        helloWorld();
    }();

    or 

    go helloWorld();
```

## Channels 
- We use channels to comminiuticate between `Goroutines` Because we can't return any value from a Goroutine.
- Channels are two types
    - Non Buffer channels.
    - Buffered channels.

```go

    // Non Buffer channels.
    // This channel will able to hold only one string value. After you write to this, you must read it.
    // If not you will not able write to this channel again...
    chan01 := make(chan string);  

    // Buffered Channels.
    // This channel will able to hold 5 string values, without reading.. After 5 filled, then you must need to read it...
    chan02 := make(chan string, 5); 


    // How to read from a channel ?

    // read
    chan01Value := <- chan01; 

    // write
    chan02 <- "Hello World"
    chan02 <- "Good Morning!"
```

- When you passing a channel to a function, You need to tell for wht you use that for
- Are you using it for `Read` or `Write` 

- How to define function parm, When you need to `Read`
```go
    func helloRead(ch <- chan string) (string, error) {
        // do something ....
        var chanVal string = <- ch; // read from the channel...
    }
```

- How to define function param, When you need to `Write`
```go 
    func helloWrite(ch chan <- string) (bool, error) {
        // do something ...
        ch <- "Hello World";
    }
```

## Close a Channel. 
- When you done with the channel, you have to close it. 
```go
    chan01 := make(chan string);
    // do something...

    close(chan01); // closed the channel;
    // When you read from a closed channel, It will always return the `0` (Zero)
```

- How to identify closed or non closed channel.
    - For this we are using `ok idom`
```go

    chan02 := make(chan string);
    close(chan02) // closed the channel.

    value, ok := <- chan02;
    if !ok {
        // channel is closed!
    } else {
        // channel is open!
    }
    fmt.println(value);
```


## For-Range Loop with Channels.
- We use `for-rage` loop to listern on channels...
```go
    for v := range channel01 {
        fmt.println(v);
    }

    // This for loop will continued until channel get closed or a brake or retun going to happen.
    // These for when you using this `for-range` make sure, there is at leat one way to kill this `for-range` loop
```
---
## N o t e s
-> Unbuffed Open Channel
    - Read: push until something is written
    - Write: push until something is read
    - Close: works 

-> Unbuffed Closed Channel
    - Read: Return Zero ( use `ok idom`)
    - Write: Panic
    - Close: Panic 

-> Buffed Open Channel
    - Read: push if buffer is empty.
    - Write: push if buffer is full
    - Close: works read values will be still there. So you can read even it closed. (use `ok idom`)


-> Buffed Closed Channel
    - Read: If buffer have values already those will return. ( use `ok idom`)
    - Write: Panic
    - Close: Panic 

-> Nil Channel
    - Read: Hang forever
    - Write: Hang forever
    - Close: Panic 
---


## S E L E C T
- To identify go concurrecny model use can you `select`
- Also `select` help to avalid go's `Stravation` and to avoid `deadlocks` too...

```go 
    select {
        case v := <- chann01:
            // do something ...
        case v := <- chann02:
            // do something ...
        case chan03 <- "Hello World"
            // do something ...
    }
```

## FOR-SELECT 
- When we have multiple channels to process and read..
- Here you need to make sure, in somepoint that will going to exit from the for-loop...
```go
    for {
        select {
            case v := <- chann01:
                // do something ...
            case v := <- chann02:
                // do something ...
            case chan03 <- "Hello World"
                // do something ...
        }
    }
```
---

## Always Clean Up your Goroutines....
- When we lunch a goroutine, we must make sure in some case it will exit. If not it will keep forever,
- That will be root case for `Goroutine Leak` 
- 
- As for this we use `Done` channel pattren.
```go
    // make a done chan
    done := make(chan struct{});

    go func() {
        select {
            case ...
            case ...
            case ...
            case <- done
                exit
        }
    }();

    // When you done with something...
    close(done); // this will triiger the `<- done` in select and goroutine will exit....
```


## Wait Groups ... 
- Wait till all the goroutines are done!
- 
```go 
    // make one...
    var wg Sync.WaitGroups;
    //
    wg.add() // -> Add a task to wait.
    wg.done() // -> Return a wait task.
    wg.wait() // -> Wait all to finished.
```

