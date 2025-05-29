# ESP32-Web-server---Basic-Calculator
This project was made on the ESP-WROOM-32 as a basic functioning web server and a calculator. The main purpose is to demonstrate the capabilities of the WiFi module of the ESP32 to function as a local web server and to show that data can be sent/received on WiFi.

<h2>Setting the web server up</h2>
For the web server to work you will need to follow a few steps in order to ensure that the project works as intended. To start off, you need to have an ESP32 microcontroller with a usb port (serial communication capability). The one used in this project is ESP-WROOM-32 by ESPRESSIF (https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32e_esp32-wroom-32ue_datasheet_en.pdf) on the Arduino IDE (https://www.arduino.cc/en/software/) in the language of C++, however, HTML was also used to create the functioning web page. In addition to the application, it is pivotal to ensure that ESP32 is able to communicate with the host. In the case that it is unable to communicate, be sure to download the required drivers on the host for your respective board.

<h2>Required Libraries</h2>
There are plenty of useful libraries for the ESP32 web server project but the ones that were specifically chosen were: <br>
- AsyncEspFsWebserver by Tolentino <br>
- ESPAsyncWebServer by lacamera <br>
- esp32 by Espressif Systems (Board Library) <br>
- Arduino ESP32 Boards by Arduino <br>

<h2>Running the code</h2>
After installing all required libraries, all that is left is putting the code and running (flashing) it on the ESP32. When connecting the usb adapter, remember to check for the correct COM(1-5) port and ensure that you are running on the correct board type. For ESP-WROOM-32, you would need to run it on "DOIT ESP32 DEVKIT V1".

<h2>Code disection</h2>
The code contains specific comments that explain more about the functionalities and meaning behind each of the segments of the code. In the beginning of any microcontroller related project, you need to first import the required libraries. Even if you install the libraries, the IDE will not be able to use the ready-made functions of the controller if you do not include the header file of the library.

<h3>WiFi Library</h3>
During setup, the "WiFi.begin(ssid, password);" function allows ESP32 to attempt to connect to a router using the WiFi library's begin() function. ssid and password will have to be replaced by your actual router ssid and router's password. <br>
while(WiFi.status() != WL_CONNECTED) will check if the board has successfully connected to the router, otherwise, it will stay idle while waiting for an ACK from the router. WiFi.localIP() will print out the local IP address of your ESP32 which you need to be able to connect to the web page from a host in the <b>SAME</b> network. After connection, we can initialize the server to wait for incoming connection requests using client as an object of class WiFiClient whenever the server is available. This is a boolean value that changes depending on the connectivity of the client to the web page. While the client is connected and is available to receive bytes from the server, a string header prints out the web page content using client.println() function where the printed out strings are HTML-based content which will generate the web page for the client that is connected.

<h3>Scripts (HTML)</h3>
Within the Arduino IDE, you may notice that we used HTML scripts. This is because we want to make the web page function as our intended calculator project. There are 3 important scripts/functions we added in the project: <br>
1) add(val)  <br>
<i>Takes the button input ids from the textbox and converts them to numbers which is then added to each other within the textbox</i> <br>
2) clearDisplay() <br>
<i>Removes all text in the calculator's textbox</i> <br>
3) calculate() <br>
<i>Main function, calculates the added values and checks for validity of the inputs (Digits and '+' are the only whitlisted inputs)</i> <br>

<h2>Contribution and Source</h2>
Sourced code by Rui Santos. (https://randomnerdtutorials.com/esp32-web-server-arduino-ide/)  <br>
Contributions: Lameist03
