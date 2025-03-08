# simple-golang-server
# Golang HTTP Server

This is a simple HTTP server written in Go that serves static files and handles form submissions.

## Features
- Serves static files from the `STATIC` directory
- Handles form submissions via `POST` requests
- Provides a simple `/hello` endpoint

## Installation and Usage
### Prerequisites
Ensure you have Go installed. You can download it from [golang.org](https://golang.org/dl/).

### Running the Server
1. Clone the repository:
   ```sh
   git clone <repository_url>
   cd <repository_name>
   ```
2. Create a `STATIC` directory and place your static files (e.g., `index.html`).
3. Run the server:
   ```sh
   go run main.go
   ```
4. Open `http://localhost:8080` in your browser.

## Endpoints
### 1. Serving Static Files
- The server serves files from the `STATIC` directory.
- Access static files via `http://localhost:8080/filename`.

### 2. `/form` Endpoint (POST Request)
Handles form submissions.
#### Example Form:
```html
<form action="/form" method="post">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name">
    <br>
    <label for="address">Address:</label>
    <input type="text" id="address" name="address">
    <br>
    <button type="submit">Submit</button>
</form>
```
#### Response Example:
```
POST REQUEST SUCCESSFUL
Name = John Doe
Address = 123 Street, City
```

### 3. `/hello` Endpoint (GET Request)
- Returns a simple `hello` message.
- Access it via `http://localhost:8080/hello`.

## Code Explanation
```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

// Handles form submission
func formhandler(w http.ResponseWriter, r *http.Request) {
    if r.Method != http.MethodPost {
        http.Error(w, "method not supported", http.StatusNotFound)
        return
    }
    if err := r.ParseForm(); err != nil {
        fmt.Fprintf(w, "parse form err %v", err)
        return
    }
    fmt.Fprintf(w, "POST REQUEST SUCCESSFUL \n")
    fmt.Fprintf(w, "Name = %s\n", r.FormValue("name"))
    fmt.Fprintf(w, "Address = %s\n", r.FormValue("address"))
}

// Handles /hello endpoint
func hellohandler(w http.ResponseWriter, r *http.Request) {
    if r.URL.Path != "/hello" {
        http.Error(w, "404 not found", http.StatusNotFound)
        return
    }
    if r.Method != http.MethodGet {
        http.Error(w, "method is not supported", http.StatusNotFound)
        return
    }
    fmt.Fprintf(w, "hello")
}

// Main function
func main() {
    fmt.Println("MAIN SERVER")
    fileServer := http.FileServer(http.Dir("./STATIC"))
    http.Handle("/", fileServer)
    http.HandleFunc("/form", formhandler)
    http.HandleFunc("/hello", hellohandler)
    fmt.Println("Server started at http://localhost:8080")
    if err := http.ListenAndServe(":8080", nil); err != nil {
        log.Fatal(err)
    }
}
```

## License
This project is open-source and available under the MIT License.

