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

In Lab 3, we saw a lot of programs, such as with ArrayLists, LinkedLists, and files, that each had various bugs. An example of this buggy code can be found in the reverseInPlace metehod in the ArrayExamples.java file. The original code, taken from the lab3 repo on GitHub by the CSE15L tutors, is as follows:

```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

The method is clearly buggy, as the replacing line `arr[i] = arr[arr.length - i - 1];` does not work properly. A good JUnit test reveals this issue. For example, we can test the method on an array consisting of integers from 1 to 5, in order. 

```
@Test
    public void testReverseInPlace() {
    int[] input = {1, 2, 3, 4, 5};
    ArrayExamples.reverseInPlace(input);
    assertArrayEquals(new int[]{5, 4, 3, 2, 1}, input);
  }
```

<img width="413" alt="Screenshot 2023-01-27 at 11 22 45 AM" src="https://user-images.githubusercontent.com/68934498/215179027-47b4502f-9fb3-4c84-a990-042f3fd2c197.png">

> Failed test


Still, some tests can pass even with the buggy code, such as when we have just one element in the array. The reason is that even with the buggy code, the reversed version is what it should be (just 5 in this case). In fact, for any array that is a palindrome, the method would work fine (such as {3, 2, 1, 2, 3}).

```
@Test 
    public void testReverseInPlace() {
    int[] input = {5};
    ArrayExamples.reverseInPlace(input);
    assertArrayEquals(new int[]{5}, input);
	}
```

<img width="394" alt="Screenshot 2023-01-27 at 11 24 50 AM" src="https://user-images.githubusercontent.com/68934498/215179352-881fdc45-c042-4461-9c1f-c83d38535a7a.png">

> Test passed even though codee is buggy.

In the first test, we can see that the third element is expected to be a 2, but the tester found a 4. This is because the line `arr[i] = arr[arr.length - i - 1];` works until half of the array, but doesn't change the value of the remaining indices. Let's analyze this with the array {1, 2, 3, 4, 5}

* From index 0 to 2 (inclusive), the method sets the *i'th* index to the value of the *5-i'th* number. So, we end up with {5, 4, 3, 4, 5}
* For the remaining two elements, it continues to do the same thing. However, since the first two elements are now 5 and 4, we end up with no change.
* The resulting array is {5, 4, 3, 4, 5} rather than {5, 4, 3, 2, 1}.

To fix this code, we can write it as follows:

```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length/2; i += 1) {
      int temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = temp;
    }
  }
```

Both tests pass following the change. Now, we only traverse through half the array, and for each element we pass through, we switch it with the elements in the back. 

* We start with the array {1, 2, 3, 4, 5}
* In the first pass through, temp is set to 1. The first element then becomes 5, and finally the last element becomes temp, or 1. The array is {5, 2, 3, 4, 1}
* Something similar happens in the next pass, and the array at this point is {5, 4, 3, 2, 1}
* Since we have now passed through half the length, we stop traversing, giving us the right answer.

## Part 3: Something I Learned

One of the most useful things I learned during lab in weeks 2 and 3 was how to debug code efficiently. Prior to the lab, I had almost no experience with JUnit, apart from a little bit of testing I did for my programming assignment in CSE 12. The lab in week 3 taught me how to use the JUnit framework properly, and helped me track down bugs in my code. For example, for the LinkedListExample, I created new tests for all the methods that showed me bugs in the append() and first() functions, and I was able to fix them by moving the line `n.next = new Node(value, null);` outside of the while loop, and throwing an Exception if the root was null, respectively. In addition, by going through so many examples, I even got better at reading through code and figuring out which parts will not function as expected. 
