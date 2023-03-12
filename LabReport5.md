# Lab Report 5

Name: Harsh Gurnani

PID: A17342880

Email: hgurnani@ucsd.edu

---

One of my favorite tasks from CSE 15L was creating an Http Server in Java. In this lab report, I'm going to be modifying the server we created in Lab Report 2. In that server, titled StringServer, we kept a running String value and added to it everytime the user put in a request. Instead of having a String Server, I now used the server to create a dummy database that stores users. 

## Part 1: User object

The User object is defined as follows:

```
public class User {
    private int ID;
    private String name;

    public User(int ID, String name) {
        this.ID = ID;
        this.name = name;
    }

    public String getName() {
        return this.name;
    }

    public void setName(String newName) {
        this.name = newName;
    }

    public int getID() {
        return this.ID;
    }
}

```

Each user has a name and an ID. Both have 'getter' methods, but only the name is modifiable using the `getName()` method. 


## Part 2: Server code

This code supports the main Handler code (shown ahead in Part 3) by actually creating the server that the user can go to. 

```
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

public class Server {
    public static void start(int port, URLHandler handler) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        server.createContext("/", new ServerHttpHandler(handler));

        server.start();
        System.out.println("Server Started! Visit http://localhost:" + port + " to visit.");
    }
}

```

## Part 3: User Server

The following code defines the behavior of the server, including what the user can query and what should be printed out.

```
import java.io.IOException;
import java.net.URI;
import java.util.*;

public class UserServer {
    public static void main(String[] args) throws IOException {
        if (args.length == 0) {
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}

class Handler implements URLHandler {
    HashMap<Integer, User> users = new HashMap<>();
    int ID = 10247892;

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            if (users.size() == 0) {
                return "No users in database yet.";
            } else {
                String representation = "";
                for (Integer i: users.keySet()) {
                    String toAdd = "(ID: " + i + ", Name: " + users.get(i).getName() + ") \n";
                    representation += toAdd;
                }
                return representation;
            }
        } else if (url.getPath().contains("/POST")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters.length > 1) {
                if (parameters[0].equals("u")) {
                    User newUser = new User(ID, parameters[1]);
                    users.put(ID, newUser);
                    ID += new Random().nextInt(45);
                    return "Added user with name " + parameters[1] + " to database.";
                } else {
                    return "Invalid query";
                }
            }
            return "Insufficient Arguments";
        } else if (url.getPath().contains("/GET")){
            String[] parameters = url.getQuery().split("=");
            if (parameters.length > 1) {
                if (parameters[0].equals("u")) {
                    for (Integer i: users.keySet()) {
                        if (users.get(i).getName().equals(parameters[1])) {
                            return "(ID: " + i + ", Name: " + users.get(i).getName() + ")";
                        }
                    }
                    return "User not found";
                } else {
                    return "Invalid query";
                }
            }
            return "Insufficient Arguments";
        } else if (url.getPath().contains("/PATCH")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters.length > 2) {
                if (parameters[0].equals("u")) {
                    for (Integer i: users.keySet()) {
                        if (users.get(i).getName().equals(parameters[1])) {
                            users.get(i).setName(parameters[2]);
                            return "User's name has been updated.";
                        }
                    }
                    return "User not found";
                } else {
                    return "Invalid query";
                }
            }
            return "Insufficient Arguments";
        } else {
            return "404 Not Found!";
        }
    }
}
```

