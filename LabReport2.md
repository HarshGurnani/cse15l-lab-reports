# Lab Report 2

Name: Harsh Gurnani

PID: A17342880

Email: hgurnani@ucsd.edu

---

## Part 1: StringServer Web Server

Part 1 consists of creating a web server called StringServer that keeps a running String value, and adds to it everytime the user requests to append a message. When a new message is added, the String adds a new line operator `\n` as well as the new text. The user can request to add to the String by typing in the host url, and adding a query as follows:

```
/add-message?s=<String>
```

Here is the code for the StringServer file:

```
// Sources used: Wavelet code provided by CSE15L tutors in lab session on January 19, 2023

import java.io.IOException;
import java.net.URI;

public class StringServer {
    public static void main(String[] args) throws IOException {
        if (args.length == 0) {
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        ServerNew.start(port, new Handler());
    }
}

class Handler implements URLHandler {
    String myString = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            if (myString.length() == 0) {
                return "Nothing in String yet!";
            }
            return myString;
        } else if (url.getPath().contains("/add-message")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters.length > 1) {
                myString += parameters[1] + "\n";
                return "Added new part to running String";
            }
            return "Insufficient Arguments";
        } else {
            return "404 Not Found!";
        }
    }
}

interface URLHandler {
    String handleRequest(URI url);
}
```

In addition, a supporting file called ServerNew.java is needed to make it run.

```
// Sources used: Wavelet code provided by CSE15L tutors in lab session on January 19, 2023

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.URI;

import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

interface URLHandler {
    String handleRequest(URI url);
}

class ServerHttpHandler implements HttpHandler {
    URLHandler handler;
    ServerHttpHandler(URLHandler handler) {
      this.handler = handler;
    }
    public void handle(final HttpExchange exchange) throws IOException {
        try {
            String ret = handler.handleRequest(exchange.getRequestURI());
            exchange.sendResponseHeaders(200, ret.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(ret.getBytes());
            os.close();
        } catch(Exception e) {
            String response = e.toString();
            exchange.sendResponseHeaders(500, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}

public class ServerNew {
    public static void start(int port, URLHandler handler) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        server.createContext("/", new ServerHttpHandler(handler));

        server.start();
        System.out.println("Server Started! Visit http://localhost:" + port + " to visit.");
    }
}

```

> Note that much of this code was sourced from the starter code provided by CSE15L tutors in the lab session on January 19, 2023. 

Here are also some screenshots of the code in action. The server was started at port 4000. Before any messages were added, the server displays this:

<img width="533" alt="Screenshot 2023-01-26 at 5 51 57 PM" src="https://user-images.githubusercontent.com/68934498/214993544-b638b9e9-906e-4216-bb88-48a3850b5f45.png">

> Default message when nothing has been added. Server is at port 4000.

<img width="457" alt="Screenshot 2023-01-26 at 5 52 08 PM" src="https://user-images.githubusercontent.com/68934498/214993609-bcc49ed8-f8b7-46ba-bfb7-34fb956b1db9.png">

> Adding the word "Hello" to the String

The home page is now updated to show the word. 

<img width="325" alt="Screenshot 2023-01-26 at 6 09 39 PM" src="https://user-images.githubusercontent.com/68934498/214995391-d7e9e473-590f-4505-abde-e7523ce66297.png">

> Updated home page

When the web server is first instantiated, a series of methods are called to actually start it. A lot of these methods are in the ServerNew file defined above. 

When the word hello is called, the handleRequest method in the Handler class is called. The method takes in the current URL as an argument and checks what it contains. If it contains just a `/`, the home page is displayed and no fields get changed because we are not adding anything. If the user uses the add message phrase defined above, the code checks for a word after the `=` and updates the `String myString` field to reflect it. If no word is included, the page displayed a text saying `Insufficient Arguments` Finally, if even an add message is not displayed, the code returns a `404 Not Found` error. 

Even when we add a number, the code works correctly. 

<img width="410" alt="Screenshot 2023-01-26 at 6 24 42 PM" src="https://user-images.githubusercontent.com/68934498/214996990-d6484172-eb6f-4034-bbba-d36f3ebc8909.png">

> Adding 2004

<img width="287" alt="Screenshot 2023-01-26 at 6 24 45 PM" src="https://user-images.githubusercontent.com/68934498/214997008-90916126-f8f3-47d8-bb66-19625ae540b3.png">

> New home page

The program is able to convert an integer to a String, allowing for a wide range of inputs. Only some inputs, like adding a `\`, are not allowed and cause a URL error. 


## Part 2: Bugs
