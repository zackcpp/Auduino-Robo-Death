##Robot

Robot with keypad, lcd, and ultrasonic


Our code:

```Cpp

#include <SPI.h>
#include <Ethernet.h>

// Your Arduino Ethernet's MAC address.
byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
// Your Arduino Ethernet's IP address (192.168.1.177). This is the URL that you type into the browser URL bar.
IPAddress ip(192,168,1,177);
// Tell your Ethernet server to listen on port 80 (the standard port for HTTP requests)
EthernetServer server(80);

int laser = 4;

void setup() {
  pinMode(laser, OUTPUT); //Setting my laser pin as an output 
  Serial.begin(9600);
  // Wait until serial is loaded.
  while (!Serial) { }
   
  // start the Ethernet connection and the server.
  Ethernet.begin(mac, ip);
  server.begin();
  Serial.print("Server is at ");
  Serial.println(Ethernet.localIP());
  pinMode(8, OUTPUT);
  
}

void loop() {
  // listen for incoming clients
  EthernetClient client = server.available();
  // If there is an incoming client
  if (client) {
    Serial.println("new client");
    // an http request ends with a blank line
    boolean currentLineIsBlank = true;
    
    // While the client is connected and waiting for data
    while (client.connected()) {
      // Check for HTTP timeout
      if (client.available()) {
        char c = client.read();
        Serial.write(c);
        // if you've gotten to the end of the line (received a newline
        // character) and the line is blank, the http request has ended,
        // so you can send a reply
        if (c == '\n' && currentLineIsBlank) {
          
          // The standard HTTP response header for a successful request.
          client.println("HTTP/1.1 200 OK");
          client.println("Content-Type: text/html");
          client.println("Connection: close");
	        client.println("Refresh: 2");
          client.println();
          client.println("<!DOCTYPE HTML>");
          
          // ************************************************
          // ************************************************
          // BELOW IS WHERE YOU PRINT OUT YOUR HTML PAGE. ***
          // ************************************************
          // ************************************************
          client.println("<html>");
            client.println("<head>");
              client.println("<title>Arduino Ethernet</title>");
              client.println("<script src=\"http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js\"></script>");
              client.println("<link href=\"http://maxcdn.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css\" rel=\"stylesheet\">");
              client.println("<script src=\"http://maxcdn.bootstrapcdn.com/bootstrap/3.1.1/js/bootstrap.min.js\"></script>");
            client.println("</head>");
            
            client.println("<body>");
            
            int van = random(3);
            if (van == 1) {
              client.println("<h1>Sharks With Laser Beams Attached to their heads!!!</h1>");
            }
            else if (van == 2) {
              client.println("<h1>Nothing</h1>");
            }
            else {
              client.println("<h1>Hey cutie, let me cool you off...</h1>");
              digitalWrite(8, HIGH);
              delay(2000);
              digitalWrite(8, LOW);
            }
    
            if (analogRead(A0) < 500) {
              client.println("<p>Thank you... Thatt feelss goood...</p>");
            }
            Serial.println(analogRead(A0));
            
            client.println("</body>");
          
          client.println("</html>");
          break;
        }
        if (c == '\n') {
          // you're starting a new line
          currentLineIsBlank = true;
        } 
        else if (c != '\r') {
          // you've gotten a character on the current line
          currentLineIsBlank = false;
        }
      }
    }
    // give the web browser time to receive the data
    delay(1);
    // close the connection:
    client.stop();
    Serial.println("client disonnected");
  }
}


```

Code for speaker beep at 1KHz


```cpp
for (int i=0; i<500; i++) {  // generate a 1KHz tone for 1/2 second
  digitalWrite(SPKR, HIGH);
  delayMicroseconds(500);
  digitalWrite(SPKR, LOW);
  delayMicroseconds(500);
}
```
