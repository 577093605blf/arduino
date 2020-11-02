#define BLINKER_WIFI
#include <Blinker.h>
#include <WiFi.h>
#include <WiFiUdp.h>
//引用各类库
 
char auth[] = "auth";
char ssid[] = "ssid";
char pswd[] = "key";
//Blinker常规操作
#define BUTTON_1 "WOL"//定义按键
BlinkerButton Button1(BUTTON_1);
 
void button1_callback(const String & state)//回调函数
{
  digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
  BLINKER_LOG("get button state: ", state);
 
  if(state == BLINKER_CMD_ON) {
    BLINKER_LOG("Toggle on!");
    wol();
    Button1.print("on");
    Blinker.delay(2000);
    Button1.print("off");//返回关闭状态，方便下一次使用。
  }
 
}
void dataRead(const String & data)
{
    BLINKER_LOG("Blinker readString: ", data);
 
    Blinker.vibrate();
    
    uint32_t BlinkerTime = millis();
    
    Blinker.print("millis", BlinkerTime);
}
 
const char * udpAddress = "192.168.0.255";//udp接收地址，我这里是使用的广播
const int udpPort = 2333;//udp端口
byte MAC[6] = {0x00, 0x10, 0x12, 0xD0, 0x9C, 0x6D};//MAC号
byte package[102];
 
//The udp library class
WiFiUDP udp;
 
void setup() {
   //WiFi.begin(ssid,pswd);
  Blinker.begin(auth,ssid,pswd);
 
  Blinker.attachData(dataRead);
  Button1.attach(button1_callback);//注册回调函数
}
void loop() {
  Blinker.run();
}
 
void wol()//开机代码部分
{
  int i, k, j = 0;
  udp.beginPacket(udpAddress, udpPort);//准备发送，设置地址和端口
  //下面构建magic packet
  for (i = 0; i < 6; i++, j++) 
  {
    package[j] = 0xFF;
  }
  for (i = 0; i < 16; i++) {
    for ( k = 0; k < 6; k++, j++)
      package[j] = MAC[k];
  }
  udp.write((byte*)package, 102);//将数组package放入缓冲区，准备发送
  udp.endPacket();//发送
 
  Blinker.delay(1000);
}
