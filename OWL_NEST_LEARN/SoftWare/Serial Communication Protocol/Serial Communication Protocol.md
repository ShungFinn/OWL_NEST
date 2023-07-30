# Serial Communication Protocol

## I$^2$C

What is I$^2$C?

### Rate

I$^2$C is a simple bidirectional two-wire bus protocol.

With low operating current, reduced system power consumption and perfect response mechanism.

Reliable communication. 

### Signal

The protocol can work in such 4 rate mode on bidirectional bus:

1.Standard-mode(SM): with a bit rate up to 100kbit/s.

2.Fast-mode(FM): with a bit rate up to 400kbit/s.

3.Fast-mode Plus(FM+): 1Mbit/s.

4.High-speed mode(Hs-mode): 3.4Mbit/s.

And on unidirectional bus:

Ultra Fast-mode(UFm): 5Mbit/s.

The rate stands for SCL's frequency.

#### UFm

UFm always be used in LED, LCD like modules which don't need to receive response, just like normal I$^2$C operating sequential but Write-Only ,the ACK response signal is ignored.

#### Normal signals

I$^2$C contents 4 sorts of basic signal: Start, Stop, Reply, Data Validity.

We use 1 to represent high level, and 0 low level, ↑ rising edge, ↓ falling edge.

Idle state: when the SCL and SDA with up-pull resistor, and both level is 1.

##### Start signal

SCL: 1

SDA: ↓

##### Stop signal

SCL: 1

SDA: ↑

##### Reply signal

Its a signal that receiving end telling sending end the data is well received or not, each byte(8bits) sends the sending end would stop and receive the response from receiving end in every 9th clock cycles.

ACK: valid response, and SDA=0.

NACK: invalid response, and SDA=1.

##### Data Validity

Only when SCL = 0, the SDA can change its level,  otherwise SDA's level change is not allowed, apart from Start /Stop signal.

### Read/Write Sequence

#### Read:

When read from slave device:

Master: Start ,7bits slave address, R/W=0

Slave: ACK

Master: 8bits register address

Slave: ACK

Master: Start, 7bits slave address, R/W=1

Slave: ACK, 8bits data byte from register

Master: NACK, Stop

#### Write:

When write to slave device:

Master: Start, 7bits slave address, R/W=0

Slave: ACK

Master: 8bits register address

Slave: ACK

Master: 8bits byte to register

Slave: ACK

Master: Stop

### Address

There's two kinds of address, 7bits and 10bits, latter is not very common used nowadays, they can link on the same I$^2$C BUS.

10bits address is divided into 7bits and 3bits and just like R/W operate in 7bits address, and if the address need to be claimed the second time,just use the 7bits.

The 2nd part comes 1111 0xxx.

### Reserved bytes

start with 0000 and 1111.

### Compare with SPI

1.I$^2$C is a half-duplex and SPI is full-duplex.

2.I$^2$C supports multi-master but SPI only supports one master.

3.Uses less GPIO.

4.With response mechanism, more reliable but SPI don't.

5.Slower than SPI.

6.SPI uses CS to chose slave, it would use extra GPIO.

7.SPI gets data when SCLK on edge, I$^2$C gets data when SCL is high.

8.Both are used in short-range communication of on-board devices.

## SPI

## UART