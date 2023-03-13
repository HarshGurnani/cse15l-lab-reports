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
        System.out.println("Server Started! Visit http://localhost:" + port + 
                            " to visit.");
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
            System.out.println("Missing port number! Try any number between " 
                                + " 1024 to 49151");
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
                    String toAdd = "(ID: " + i + ", Name: " + 
                                     users.get(i).getName() + 
                                     ") \n";
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
                    return "Added user with name " + parameters[1] + 
                            " to database.";
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
                            return "(ID: " + i + ", Name: " + 
                                    users.get(i).getName() + ")";
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

The UserServer class contains the main method through which the server is started. Below that is the Handler class, which is what actually processes the URL that the user enters. The class contains a HashMap, a data structure that stores key-value pairs. In our case, the HashMap stores all the data with an Integer as the key (the User ID is used for the key) and a User object as the value. Essentially, this is a dummy database storing User objects.

The home page displays all the user objects that are currently stored in the 'database.' From the home page, someone running the server has three possible routes: Post, Get, and Patch. 

### POST Route

The POST route is used to add a new User object to the database. We can call this route using a query like this:

```
/POST?u=<name>
```

In the place of `<name>`, we would include the name of the object we are trying to create. The integer ID, declared at the top of the class, is used as the user's ID. Then, ID is incremented by a random amount between 1 and 45 so that a new ID can be used for the next user. If a name is not passed in, an error message saying "Insufficient Arguments" is printed to the screen.

<img width="319" alt="LabReport5_1" src="https://user-images.githubusercontent.com/68934498/224521796-92234bd3-5108-4964-8aa3-ec57ef2cdad8.png">

> Server started at port 2692. There are currently no users in the database

<img width="367" alt="LabReport5_2" src="https://user-images.githubusercontent.com/68934498/224521855-a360532d-37ee-432b-96f8-eab51e32cba4.png">

> Added user with name "Harsh" to database.

<img width="276" alt="LabReport5_3" src="https://user-images.githubusercontent.com/68934498/224521867-b03d2216-9d5e-4282-b06f-d9f93d719991.png">

> Home page now displays a new user

### GET Route

The GET route is used to search for a User object. It can be accessed with a query as follows:

```
/GET?u=<name>
```

We search the user objects with a name, rather than an ID. This is analogous to a real database search where most people would not know individual user's ID's, but instead would want to search by name. If the user is not found, an error message is printed to the screen. If a name is not passed in, an error message saying "Insufficient Arguments" is printed to the screen.

 <img width="373" alt="LabReport5_4" src="https://user-images.githubusercontent.com/68934498/224521937-25c494f5-de7d-40f6-b462-63e4f621f000.png">

> When we search for a name, that user's String representation is returned, provided that the user is not found.

<img width="363" alt="LabReport5_5" src="https://user-images.githubusercontent.com/68934498/224521962-feb3aff9-f979-4dfd-8dce-dd7fbb727bbb.png">

> User not found in database

### PATCH Route

Finally, the last route we can explore is the PATCH route. This is used to update an existing user in the database. Since the ID is the key used to map an object in the HashMap, it would be foolish to try and edit that. Instead, we are able to edit the user's name by accessing the `setName()` method provided in the User class.

The PATCH route uses the following query:

```
/PATCH?u=<name_to_search>=<name_to_replace>
```

Unconventially, the route uses two equal signs, in order to separate the name we are searching for and the name we want to replace it with. This solution works as a URL, but in an actual application we could implement it differently so it shows up nicely in a graphical user interface. If two names are not passed in, an error message saying "Insufficient Arguments" is printed to the screen.

<img width="284" alt="LabReport5_6" src="https://user-images.githubusercontent.com/68934498/224522070-3d051bf0-6ff0-4aec-9ee2-93c9ab6ca9a2.png">

> Server before updating any names

<img width="445" alt="LabReport5_7" src="https://user-images.githubusercontent.com/68934498/224522089-7565ee97-d480-434d-a960-824a83ab08fc.png">

> Updating the user with name "Vinayak" to "Shray"

<img width="284" alt="LabReport5_8" src="https://user-images.githubusercontent.com/68934498/224522105-d357a059-a64e-4570-b0a7-f455b5435d24.png">

> The updated database. As visible, where we previously saw the name "Vinayak", we now see the name "Shray"

## Part 4: Reflection

The "database" implemented above is very primitive, but is actually analogous to many real life applications. Since Lab Report 2, I've learned a lot more about full stack development and wanted to implement some of the things I learned using the server. Importantly, many applications use GET, POST, and PATCH routes to connect their frontends to the backend. For example, we may use GET to receive a user's name and age from the backend and then display it on the frontend. Alternatively, PATCH may be used to send an updated message from the UI to the database if the user decides to update their personal pronouns. 

In addition, I was able to utilize some Java tools that I picked up. Firstly, I used a HashMap for the database, as shown in the code 

```
HashMap<Integer, User> users = new HashMap<>();
```

A HashMap is a data structure that uses hashing to map a key to a value. It is an extremely useful tool, especially since insertion and retrival occur in O(1) time, meaning that it is very efficient. Furthermore, I updated the ID with the `Random()` package in java.

```
ID += new Random().nextInt(45);
```

This way, we would not just end up with consecutive ID numbers between students. 

Overall, the server implementation was one of my favorite tasks in CSE 15L, and I wanted to implement it in a different way than we did before in order to show how many potential applications it has. While I used much of the same code as the first time around, I really enjoyed creating the different routes that matched what backend routes are like in real world development. 
