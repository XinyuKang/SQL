# Knowledge about computer networking

## Communication Protocals
### User Datagram Protocol (UDP/IP)
The User Datagram Protocol, or UDP, is a communication protocol used across the Internet for especially **time-sensitive** transmissions such as video playback or DNS lookups. It speeds up communications by **not formally establishing a connection before data is transferred**. This allows data to be transferred very quickly, but it can also cause packets to become lost in transit.

### TCP vs. UDP
![tcpUdp](https://github.com/XinyuKang/techInterviewPrep/assets/46883505/df37487a-b934-4dd9-9853-c7b577953f77)

UDP is faster but less reliable than TCP, another common transport protocol. In a TCP communication, the two computers begin by establishing a connection via an automated process called a **handshake**. Only once this handshake has been completed will one computer actually transfer data packets to the other.

UDP communications do not go through this process. Instead, one computer can simply begin sending data to the other.

In addition, TCP communications indicate the order in which data packets should be received and confirm that packets arrive as intended. If a packet does not arrive — e.g. due to congestion in intermediary networks — TCP requires that it be re-sent. **UDP communications do not include any of this functionality (no order or arrival check)**.

These differences create some advantages. Because UDP does not require a ‘handshake’ or check whether data arrives properly, it is able to transfer data much faster than TCP.

However, this speed creates tradeoffs. If a UDP datagram is lost in transit, it will not be re-sent. As a result, applications that use UDP must be able to tolerate errors, loss, and duplication.

## Different Casts (transferring of data)
![cast](https://github.com/XinyuKang/techInterviewPrep/assets/46883505/f4dbdefd-4167-447b-b7d9-951132878804)

### Multicast
Multicast is group communication where data transmission is addressed to a group of destination computers simultaneously.
