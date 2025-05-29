// Load Wi-Fi library
#include <WiFi.h>

// Replace with your network credentials
const char* ssid = "Your_WiFi_SSID";
const char* password = "Your_WiFi_Password";

// Set web server port number to 80
WiFiServer server(80);

// Variable to store the HTTP request
String header;

// variables
volatile int total = 0;
const int result = 0;

// Current time
unsigned long currentTime = millis();
// Previous time
unsigned long previousTime = 0; 
// Define timeout time in milliseconds (example: 2000ms = 2s)
const long timeoutTime = 2000;

void setup() {
  Serial.begin(115200);

  // Connect to Wi-Fi network with SSID and password
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  // Print local IP address and start web server
  Serial.println("");
  Serial.println("WiFi connected.");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
  server.begin();
}

void loop(){
  WiFiClient client = server.available();   // Listen for incoming clients

  if (client) {                             // If a new client connects,
    currentTime = millis();
    previousTime = currentTime;
    Serial.println("New Client.");          // print a message out in the serial port
    String currentLine = "";                // make a String to hold incoming data from the client
    while (client.connected() && currentTime - previousTime <= timeoutTime) {  // loop while the client's connected
      currentTime = millis();
      if (client.available()) {             // if there's bytes to read from the client,
        char c = client.read();             // read a byte, then
        Serial.write(c);                    // print it out the serial monitor
        header += c;
        if (c == '\n') {                    // if the byte is a newline character
          // if the current line is blank, you got two newline characters in a row.
          // that's the end of the client HTTP request, so send a response:
          if (currentLine.length() == 0) {
            // HTTP headers always start with a response code (e.g. HTTP/1.1 200 OK)
            // and a content-type so the client knows what's coming, then a blank line:
            client.println("HTTP/1.1 200 OK");
            client.println("Content-type:text/html");
            client.println("Connection: close");
            client.println();
            
            // Display the HTML web page
            client.println("<!DOCTYPE html><html>");
            client.println("<head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
            client.println("<link rel=\"icon\" href=\"data:,\">");
            // CSS to style the on/off buttons 
            // Feel free to change the background-color and font-size attributes to fit your preferences
            client.println("<style>html { font-family: Helvetica; display: inline-block; margin: 0px auto; text-align: center;}");
            client.println(".button { background-color: #4CAF50; border: none; color: white; padding: 16px 40px;");
            client.println("text-decoration: none; font-size: 30px; margin: 2px; cursor: pointer;}");
            client.println(".button2 {background-color: #555555;}</style></head>");
            
            // Web Page Heading
            client.println("<body><h1>ESP32 Web Server</h1>");
          // Functional buttons for user inputs for the Calculator
          //  client.println("<p>Total is: " + String(total) + "</p>");      
          //  client.println("<a href=\"/one/pressed\"><button class=\"button\">1</button></a>");
          //  client.println("<a href=\"/two/pressed\"><button class=\"button\">2</button></a>");
            client.println("<input type='text' id='display' value='' readonly><br>");
            client.println("<input type='button' value='1' onclick='add(\"1\")'>");
            client.println("<input type='button' value='2' onclick='add(\"2\")'>");
            client.println("<input type='button' value='3' onclick='add(\"3\")'><br>");
            client.println("<input type='button' value='4' onclick='add(\"4\")'>");
            client.println("<input type='button' value='5' onclick='add(\"5\")'>");
            client.println("<input type='button' value='6' onclick='add(\"6\")'><br>");
            client.println("<input type='button' value='7' onclick='add(\"7\")'>");
            client.println("<input type='button' value='8' onclick='add(\"8\")'>");
            client.println("<input type='button' value='9' onclick='add(\"9\")'><br>");
            client.println("<input type='button' value='0' onclick='add(\"0\")'>");
            client.println("<input type='button' value='+' onclick='add(\"+\")'>");
            client.println("<input type='button' value='C' onclick='clearDisplay()'><br>"); //Connected to the clearDisplay() function
            client.println("<input type='button' value='=' onclick='calculate()'>"); //Connected to the calculate() function
          
          //functions

          client.println("<script>");
          client.println("let resultShown = false;");  // Global flag for a displayed value
          client.println("let oldResult = '';"); // Save old results if needed, for this instance, we are not using this variable.          
          client.println("function add(val) {");
          client.println("  let display = document.getElementById('display');"); // Get the displayed text Id
          client.println("  if (val === '+') {resultShown = false;};"); // If the input was a '+', do not clear display
          client.println("  if (resultShown && val !== '+') {"); // Else clear display and take new inputs. Previous calculation is not taken into consideration.
          client.println("    display.value = '';");  // Clear previous result
          client.println("    resultShown = false;"); // Clear resultShown flag i.e. no value is currently being displayed.
          client.println("  }");
          client.println("  if (display.value === '' && val === '+') return;");  // Append '+' and first input and check if display is empty
          client.println("  display.value += val;");                     
          client.println("}");
          client.println("function clearDisplay() {");
          client.println("  document.getElementById('display').value = '';");  // Sets display text to be of id '' or empty.
          client.println("  resultShown = false;"); // Clears resultShown flag
          client.println("}");
          client.println("function calculate() {");
          client.println("  let input = document.getElementById('display').value;"); // Set input to displayed text's respective values.
          client.println("  if (/^[0-9+]+$/.test(input)) {"); // Tests the input for validity. [0-9, '+'] are the only valid inputs. Returns boolean value.
          client.println("    let result = input.split('+').reduce((a,b) => Number(a) + Number(b),0);"); //splits inputs with respect to '+' and adds the units together based on the splitting.
          client.println("    document.getElementById('display').value = result;"); // Display result
          client.println("    resultShown = true;"); // resultShown is set. Numbers are displayed in the calculator's text box.
          client.println("  } else {"); // Handles invalid input
          client.println("    document.getElementById('display').value = 'Error';"); // Display 'Error' in the text box.
          client.println("    resultShown = true;"); //  resultShown is set. Text is being displayed in the calculator's text box.
          client.println("  }");
          client.println("}");
          client.println("</script>");


            client.println("</body></html>");
            
            // The HTTP response ends with another blank line
            client.println();
            // Break out of the while loop
            break;
          } else { // if you got a newline, then clear currentLine
            currentLine = "";
          }
        } else if (c != '\r') {  // if you got anything else but a carriage return character,
          currentLine += c;      // add it to the end of the currentLine
        }
      }
    }
    // Clear the header variable
    header = "";
    // Close the connection and generate a log output.
    client.stop();
    Serial.println("Client disconnected.");
    Serial.println("");
  }
}
