---
title: "Go Webassembly"
tags:
  - golang
  - webassembly
date: 2023-02-26T18:19:24+08:00
---

<style>
.lntable{
    max-height: 99em;
}
</style>

[程式碼](https://github.com/adrian-lin-1-0-0/go-webassembly-demo)

## Server and Client


1. 直接寫code吧

    當使用`js/wasm`架構時,`syscall/js`可以訪問`WebAssembly`環境.它的API基於JavaScript.<br>

    因為在瀏覽器上不能直接使用`TCP`或`UDP`,所以這裡使用`websocket`.<br>


    client/client.go
    ```go
    package main

    import (
        "syscall/js"
    )

    func main() {

        js.Global().Get("WebSocket").New("ws://localhost:8080/echo")

    }
    ```

    server 端就直接使用`TCP`了(大家都是tcp,不要分那麼細)

    server/server.go
    ```go
    package main

    import (
        "net"
    )

    func main() {
        println("Starting TCP server...")
        listener, err := net.Listen("tcp", ":8080")
        if err != nil {
            println("Error starting TCP server:", err)
            return
        }
        defer listener.Close()

        for {
            conn, err := listener.Accept()
            if err != nil {
                println("Error accepting connection:", err)
                continue
            }
            go handleConnection(conn)
        }
    }

    func handleConnection(conn net.Conn) {
        defer conn.Close()

        for {
            buffer := make([]byte, 1024)
            length, err := conn.Read(buffer)
            if err != nil {
                println("Error reading:", err)
                return
            }
            message := string(buffer[:length])
            println("Received message:", message)
        }
    }
    ```

2. 編譯Client成`WebAssembly`
    ```
    GOOS=js GOARCH=wasm go build -o static/client.wasm client/client.go
    ```


3. 要使用`.wasm`還需要`wasm_exec.js`
    ```sh
     cp "$(go env GOROOT)/misc/wasm/wasm_exec.js" static
    ```

4. 編寫 html

    html.index

    ```html
    <html>
    <script src="static/wasm_exec.js"></script>
    <script>
        const go = new Go();
        WebAssembly.instantiateStreaming(fetch("static/client.wasm"), go.importObject)
            .then((result) => go.run(result.instance));
    </script>

    </html>
    ```

5. 看看結果

    啟動`TCP` Server

    ```go
    cd server
    go run server.go
    ```

    使用`python3`的`http.server`

    ```
    python3 -m http.server 8000
    ```

    接著在瀏覽器上開啟`http://localhost:8000` 
    > 這時候 Client 會試圖跟 TCP Server 建立 websocket connection
    
    <br>

    在`TCP` Server的Terminal上可以看到`Protocol upgrade mechanism`
    ```
    Received message: GET /echo HTTP/1.1
    Host: localhost:8080
    Connection: Upgrade
    Pragma: no-cache
    Cache-Control: no-cache
    User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.0.0 Safari/537.36
    Upgrade: websocket
    Origin: http://0.0.0.0:8000
    Sec-WebSocket-Version: 13
    Accept-Encoding: gzip, deflate, br
    Accept-Language: zh-TW,zh;q=0.9,en-US;q=0.8,en;q=0.7
    Sec-WebSocket-Key: KJPuTDiTFeddEzYXqus2Bg==
    Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits
    ```
    
    