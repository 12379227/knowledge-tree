#### TCP协议中的Sequence number和Acknowledge number含义及其变化规则

- Ack: 
  - acknowledge number: 表示的是期望的对方(接收方)的下一次sequence number是多少. 
  - acknowledge number: **表示对本方已经成功接收对方多少报文的确认**. 
  - 接收到的上一次远端主机传来的seq, 然后+1, 在发送给远端主机. 提示远端主机已经成功接收上一次所有数据. 
- Seq: 
  - sequence number: 表示的是我方(发送方)这边, 这个packet的数据部分的第一位***应该***在整个data stream终端所在的位置. 
  - sequence number: **表示对本方已经发送了多少报文的确认**
  - 经过wireshark显示, tcp握手第一个SYN报文的seq = 0. 
- SYN/FIN的传输虽然没有data, 但是会让下一次传输的packet seq➕1, 但是, ACK的传输, 不会让下一次的传输packet seq➕1



```c++
//seq: 相同方向的上个报文(last_my_packet)的seq + 相同方向的上个报文(last_my_packet)的length
if (current_my_packet is my first packet)
  current_my_packet.seq = 0;
else
{
  if (last_my_packet.SYN == 1 || last_my_packet.FIN == 1)
  	current_my_packet.seq = last_my_packet.seq + 1;
  else
  	current_my_packet.seq = last_my_packet.seq + last_my_pacetk.len;
}


//ack: 相反方向上的上个报文(last_your_packet)的seq + 相反方向上的上个报文(last_your_packet)的length
if (you never send any packet)
  current_my_packet.ack = 0
else
{
  if (last_your_packet.SYN == 1 || last_your_packet.FIN == 1)
    current_my_packet.ack = last_your_packet.seq + 1
  else
    current_my_packet.ack = last_your_packet.seq + last_you_packet.len
}
  
```


