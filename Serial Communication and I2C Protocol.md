# Serial Communication and I2C Protocols
## What is serial communication?
To transfer data from one system to another there are mainly two ways, **serial communication** and **parallel communtication**. As the name suggests, in serial type, the data is sent in a series form whereas in the parallel type the data is sent in a parallel type. What this means is that in parallel type, we would require a lot of wires connected between the systems which although **very fast** but is often **not feasable**. The serial type on the other hand sends the data in a serial order which therefore requires one cable for data transfer and one or two other for earthing etc. Hence we can deduce that this method is quite **slow** as compared to the other way but **is feasible**.<br><br>
### Serial Communication and Arduino
Serial Communication is a process of sending data 1 bit at a time sequentially over a communication channel. This the way the Arduino board communicates to the computer. Serial Communication is used when we send a commond from computer to Arduino board and vice-versa. Thus, uploading a code uses the same. The LEDs (present on Arduino) **RX** and **TX** flash when the arduino receives data and transmits data respectively.<br>
###### A simple Arduino code:
~~~
void setup() {
Serial.begin(9600);
}

void loop() {
Serial.print('1');
delay(200);
}
~~~
Output:<br>
![Serial Communication](https://cdn.instructables.com/FOO/XDSD/J7JMSYT1/FOOXDSDJ7JMSYT1.LARGE.jpg?auto=webp&frame=1&fit=bounds)<br>
The above picture shows the Serial Output for the code.<br>
### Serial Communication using Python
Python can be used to interact with microcontrollers and other serial-port-enabled devices (including those using virtual ports). <br><br>
![Circuit](https://maker.pro/storage/3ZalKvD/3ZalKvDI6JPuLpMCvWxz2oBSmXKVYf7QkYM8gOTB.jpeg)
<br><br>We use PySerial for the purpose. **Installing PySerial:**<br>
On your computer, open a terminal or command prompt and type in “PIP”. If you get an unrecognized error, then your PIP did not install correctly. When installing Python, make sure that the “Add to environmental variables” option is checked. Once PIP is working, run the command shown below to install PySerial:<br><br>
~~~
pip install PySerial
~~~
![Code](https://maker.pro/storage/GUHsXPu/GUHsXPuNUHsokHAOP5zJtPiLlj99w8WUhPWOuhu3.jpeg)<br><br>
You need to configure a few parameters while using PySerial:<br>
* Baud rate – How fast your COM port operates. Arduino projects tend to operate at 115200
* Port – The name of the port being used (find this in device manager)
* Parity bits – These are used for error correction but are not normally used
* Stop bits – Only one stop bit is ever used unless there are timing issues
* Time out – Used to prevent the serial port from hanging<br><br>
Now, follow the code:<br>
~~~
import serial

serialPort = serial.Serial(port = "COM4", baudrate=115200,
                           bytesize=8, timeout=2, stopbits=serial.STOPBITS_ONE)
~~~
Commonly used Functions for using the Serial Port:
* open() – This will open the serial port
* close() – This will close the serial port
* readline() – This will read a string from the serial port
* read(size) – This will read n number of bytes from the serial port
* write(data) – This will write the data passed to the function to the serial port
* in_waiting – This variable holds the number of bytes in the buffer<br><br>
# I2C Protocols
### What are I2C Protocols?
**I2C** _(pronounced I square C)_ or **Inter-Integrated Circuit** is a serial protocol for two-wire interface to connect low-speed devices like *microcontrollers, EEPROMs, A/D and D/A converters, I/O interfaces* and other similar peripherals in embedded systems. I2C bus is popular because it is simple to use, there can be more than one master, only upper bus speed is defined. The easy implementations comes with the fact that only two wires are required for communication between up to almost 128 (112) devices when using 7 bits addressing and up to almost 1024 (1008) devices when using 10 bits addressing. <br><br>
![I2C](http://quanser-update.azurewebsites.net/quarc/documentation/i2c_protocol_diagram.gif)<br><br>
Each I2C slave device needs an address. Each slave device has a unique address. Transfer from and to master device is serial and it is split into 8-bit packets. All these simple requirements make it very simple to implement I2C interface even with cheap microcontrollers that have no special I2C hardware controller. You only need 2 free I/O pins and few simple i2C routines to send and receive commands.

#### I2C Interface
I2C uses only two wires, **SDA** (*serial data*) and **SCL** (*serial clock*). SCA has the purpose of producing a synchronic wave or the clock wave, whereas SDA is a digital wave containing data in form of bits.

#### I2C Address
I2C generally uses 8-bit data transmission. The first 7-bits are the address bits which contains the address of the slave devices, followed by the 8th-bit which indicates *read* or *write*. This forms the **Control Byte**.

#### I2C Protocols
The protocol begins with a *start signal* by the master device which is followed by the *Control Unit*. After every 9th clock cycle we either have an *Acknowledge (ACK)* bit or *Not Acknowledge (NACK)* bit. Further running depends on this. If the bit is ACK, the next 8-bits contain the data sent by the Master device which is again followed by ACK or NACK. When NACK is encountered, it is considered as an end sign.

#### Conclusion
I2C bus is used by many integrated circuits and is simple to implement. Any microcontroller can communicate with I2C devices even if it has no special I2C interface. I2C specifications are flexlible – I2C bus can communicate with slow devices and can also use high speed modes to transfer large amounts of data. Because of many advantages, I2C bus will remain as one of the most popular serial interfaces to connect integrated circuits on the board.
### I2C and Arduino
First and foremost we need to connect the boards.The Serial Clock pin of the Arduino Board will be connected to the Serial Clock pins of the two breakout boards, the same goes for the Serial Data pins and we will *power the boards with the Gnd* and the *5V pin from the Arduino Board*. Note here we are not using pull-up resistors because the breakout boards already have them. However, if that is not the case, pull-up resistors can be used externally as well. Now in order to communicate with these chips or sensors we need to know their unique addresses. We can find them from the datasheets of the sensors .We can also get or check the addresses using the I2C Scanner sketch which can be found from the Arduino official website.<br><br>![I2C and Arduino](https://cdn.instructables.com/FW0/56GU/J8UH083V/FW056GUJ8UH083V.LARGE.jpg)<br><br>
### Source code
Here is an example where we use the GY-80 breakout board which consists 5 different sensors and the GY-521 breakout board which consists 3 different sensors.<br><br>
We will use the Arduino **Wire Library**. *The Wire.begin() function* will initiate the Wire library and also we need to initiate the serial communication because we will use the Serial Monitor to show the data from the sensor.
In the loop() we will start with the *Wire.beginTransmission() function* which will begin the transmission to the particular sensor, the 3 Axis Accelerometer in our case. Then with the *Wire.write() function* we will ask for the particular data from the two registers of the X axis. The *Wire.endTransmission()* will end the transmission and transmit the data from the registers. Now with the *Wire.requestFrom() function* we will request the transmitted data or the two bytes from the two registers. The *Wire.available() function* will return the number of bytes available for retrieval and if that number match with our requested bytes, in our case 2 bytes, using the Wire.read() function we will read the bytes from the two registers of the X axis. At the end we will print the data into the serial monitor.

~~~
#include <Wire.h>
int ADXLAddress = 0x53;  // Device address.
#define X_Axis_Register_DATAX0 0x32   // Hexadecima address for the DATAX0 internal register.
#define X_Axis_Register_DATAX1 0x33   // Hexadecima address for the DATAX1 internal register.
#define Power_Register 0x2D  // Power Control Register
int X0,X1,X_out;
void setup() {
  Wire.begin(); // Initiate the Wire library
  Serial.begin(9600);
  delay(100);
  // Enable measurement
  Wire.beginTransmission(ADXLAddress);
  Wire.write(Power_Register);
  // Bit D3 High for measuring enable (0000 1000)
  Wire.write(8);  
  Wire.endTransmission();
}
void loop() {
  Wire.beginTransmission(ADXLAddress); // Begin transmission to the Sensor 
  //Ask the particular registers for data
  Wire.write(X_Axis_Register_DATAX0);
  Wire.write(X_Axis_Register_DATAX1);
  
  Wire.endTransmission(); // Ends the transmission and transmits the data from the two registers
  
  Wire.requestFrom(ADXLAddress,2); // Request the transmitted two bytes from the two registers
  
  if(Wire.available()<=2) {  // 
    X0 = Wire.read(); // Reads the data from the register
    X1 = Wire.read();   
  }
  
  Serial.print("X0= ");
  Serial.print(X0);
  Serial.print("X1= ");
  Serial.println(X1);
}
~~~
