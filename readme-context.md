
# -- C O N T E X T  --

> Context use to Control your GoLang functionalities,

01. How to import
    ```go
    import {
        "context"
    }
    ```
02. Types...
    > context.Background()
        - For the long running tasks.
    
    > Context need to be end with TimeOuts
        - context.WithDeadLine(ctx, time)
            - ctx: parent context
            - time: specific time you need to end the process
        
        - context.WithCancel(ctx, diuration)
            - ctx: parent context
            - diuration: time in second (12s, 100s etc...)

    > When you need to Cancel Context Manually or Using specific condition..
        - context.WithCancel(ctx)
        ```go

        ctx, cancelCtx := context.WithCancel(ctx);
        defer cancelCtx() 
        // You can call the cancelCtx as you need!, Remember, somehow this must be called...
        ```
    
    > How to pass values for the Contexts...
        (SET)
        - context.WithValue(ctx, key, value)
            ctx: Preant ctx
            key: Key of the value
            value: The data you need to store

        ```go
            ctx := context.WithValue(ctx, "name", "jenny join")
        ```
        
        (READ)
        - ctx.Value(key)
        ```go
            name := ctx.Value("name");
            fmt.Println(name);
        ```
