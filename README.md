# Send_WhatsApp_Msg_from_ESP32

This article is about sending WhatsApp messages from ESP32 to my Android smartphone using a third party API.

https://www.callmebot.com/blog/free-api-whatsapp-messages/

Follow the instructions in this page, get an API key

Embed this into the ESP32 code, 

Upload this to the ESP32

The API will send the message from ESP32 to your smart phone.

The code / Arduino Sketch is from 

https://randomnerdtutorials.com/esp32-send-messages-whatsapp/#:~:text=After%20inserting%20your%20network%20credentials,Go%20to%20your%20WhatsApp%20account.

```

/*The above copyright notice and this permission notice shall be included in all
  copies or substantial portions of the Software.
*/

#include <WiFi.h>    
#include <HTTPClient.h>
#include <UrlEncode.h>

const char* ssid = "YOUR WiFi Router SSID";
const char* password = "Your WiFi Router Password";

// +international_country_code + phone number
// Portugal +351, example: +351912345678
String phoneNumber = "+919898989898"; //  <===========  Add Destination phone number here.
String apiKey = "XXXXXX";  // <=============== Add API Here

void sendMessage(String message){

  // Data to send with HTTP POST
  String url = "https://api.callmebot.com/whatsapp.php?phone=" + phoneNumber + "&apikey=" + apiKey + "&text=" + urlEncode(message);    
  HTTPClient http;
  http.begin(url);

  // Specify content-type header
  http.addHeader("Content-Type", "application/x-www-form-urlencoded");
  
  // Send HTTP POST request
  int httpResponseCode = http.POST(url);
  if (httpResponseCode == 200){
    Serial.print("Message sent successfully");
  }
  else{
    Serial.println("Error sending the message");
    Serial.print("HTTP response code: ");
    Serial.println(httpResponseCode);
  }

  // Free resources
  http.end();
}

void setup() {
  Serial.begin(115200);

  WiFi.begin(ssid, password);
  Serial.println("Connecting");
  while(WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.print("Connected to WiFi network with IP Address: ");
  Serial.println(WiFi.localIP());

  // Send Message to WhatsAPP
  sendMessage("Hello Siddartha, should appear on your phone.");
}

void loop() {
  
}



![image](https://github.com/kiranshashiny/send_WhatsApp_Msg_from_ESP32/assets/14288989/898c82e1-a4e3-4197-a903-37de01a896ae)

