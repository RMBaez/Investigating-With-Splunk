<h1>Investigating-With-Splunk</h1>


<h2>Description</h2>
SOC Analyst Johny has observed some anomalous behaviours in the logs of a few windows machines. It looks like the adversary has access to some of these machines and successfully created some backdoor. His manager has asked him to pull those logs from suspected hosts and ingest them into Splunk for quick investigation. Our task as SOC Analyst is to examine the logs and identify the anomalies. 

<h2>Questions</h2>

- <b>How many events were collected and Ingested in the index main?</b>
- <b>On one of the infected hosts, the adversary was successful in creating a backdoor user. What is the new username?</b>
- <b>On the same host, a registry key was also updated regarding the new backdoor user. What is the full path of that registry key?</b>
- <b>Examine the logs and identify the user that the adversary was trying to impersonate.</b>
- <b>What is the command used to add a backdoor user from a remote computer?</b>
- <b>How many times was the login attempt from the backdoor user observed during the investigation?</b>
- <b>What is the name of the infected host on which suspicious Powershell commands were executed?</b>
- <b>PowerShell logging is enabled on this device. How many events were logged for the malicious PowerShell execution?</b>
- <b>An encoded Powershell script from the infected host initiated a web request. What is the full URL?<b/>


<h2>Languages and Utilities Used</h2>

- <b>Splunk</b> 


<h2>Environments Used </h2>

- <b>TryHackMe VM</b> 

<h2>Program walk-through</h2>

<b>Answer the question below <br/>

How many events were collected and Ingested in the index main? ?

<p align="center">
To get the complete total number of events, we must first set the time filter to "All Time"(enclosed in pink on the top right). Then I input "index=main" on the search bar. The answer is enclosed in the red box. <br/>
<img width="1440" alt="Screenshot 2025-04-15 at 12 19 26 PM" src="https://github.com/user-attachments/assets/ef61c4eb-b481-4dec-baa5-3f5069cc5b2d" />


<br />
<br />
Answer is 12,256: <br/>





<h2>Program walk-through</h2>

<b>Answer the question below <br/>
On one of the infected hosts, the adversary was successful in creating a backdoor user. What is the new username?

<p align="center">
The question states the adversaty CREATING a backdoor user. I googled the event id associated with user creation and recieved event id 4720. I then added EventID=4720 into the search bar. I scrolled down and found the newly created user. At a quick glance it might read Alberto but the "l" is actually a "1" thus giving the answer.    <br/>
<img width="1440" alt="image" src="https://github.com/user-attachments/assets/e77b5546-f0b4-47fc-88db-8ea4b0051113" />
<img width="1440" alt="image" src="https://github.com/user-attachments/assets/e13a9add-d45e-4f67-9e21-d4a601e6cb01" />
<img width="1440" alt="Screenshot 2025-04-15 at 12 35 46 PM" src="https://github.com/user-attachments/assets/74aa703f-b5d0-4eca-b7eb-2b1cd55fc5b0" />




<br />
<br />
Answer is A1berto <br/>




<h2>Program walk-through</h2>

<b>Answer the question below <br/>
3. ?


<p align="center">
The command needed to find the answer is given during the module explaination. The filter is icmp.type == 3 and icmp.code == 3. The numbers to the right of Displayed, is the answer: <br/>
<img width="1440" alt="Screenshot 2025-04-04 at 10 55 20 AM" src="https://github.com/user-attachments/assets/3b407a04-e9cb-4e36-81d6-7d67f1d40dc0" />


<br />
<br />
Answer is 1083: <br/>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>
4.?

<p align="center">
The filter to be used is udp.port in {55..70}. As we can see from the search results, the first UDP attempts met a closed port. But upon the third try, an open port was found.: <br/>
<img width="1440" alt="Screenshot 2025-04-04 at 11 16 50 AM" src="https://github.com/user-attachments/assets/3b057665-c16b-4b45-aad4-79e4bc86fb30" />


<br />
<br />
Answer is 68: <br/>



<h2>Program walk-through</h2>

<b>Answer the question below <br/>

1. ?

<p align="center">
The command needed to find the answer is given during the module explaination. The question is looking for any packet that has a size greater than 1024 bytes. The command will be, tcp.flags.syn == 1 and tcp.flags.ack == 0 and tcp.window_size > 1024. . The numbers to the right of Displayed is the answer: <br/>
<img width="1440" alt="Screenshot 2025-04-04 at 10 23 13 AM" src="https://github.com/user-attachments/assets/c081e3aa-b58a-4a90-ab7a-d8d4ede25e09" />




<br />
<br />
Answer is 1000: <br/>





<h2>Program walk-through</h2>

<b>Answer the question below <br/>
2. ?

<p align="center">
Since we want to know that we are looking at port 80 via TCP. Click on the mint green filter bar, and use the filter tcp.port == 80. Looking at the first 4 results we can see that they are all part of the same stream by the connecting bracket. So next we want to move down to the Info section, to figure out what type of scan this could be. Taking a look at the different flags that were used, we see SYN, SYN ACK, ACK, RST ACK. It looks like the process of a Three-way Handshake.: <br/>
<img width="1440" alt="Screenshot 2025-04-04 at 10 35 15 AM" src="https://github.com/user-attachments/assets/88a82ae6-6f08-4f6c-bc79-6c5d2bd95312" />





<br />
<br />
Answer is TCP Connect: <br/>




<h2>Program walk-through</h2>

<b>Answer the question below <br/>
3. ?


<p align="center">
The command needed to find the answer is given during the module explaination. The filter is icmp.type == 3 and icmp.code == 3. The numbers to the right of Displayed, is the answer: <br/>
<img width="1440" alt="Screenshot 2025-04-04 at 10 55 20 AM" src="https://github.com/user-attachments/assets/3b407a04-e9cb-4e36-81d6-7d67f1d40dc0" />


<br />
<br />
Answer is 1083: <br/>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>
4.?

<p align="center">
The filter to be used is udp.port in {55..70}. As we can see from the search results, the first UDP attempts met a closed port. But upon the third try, an open port was found.: <br/>
<img width="1440" alt="Screenshot 2025-04-04 at 11 16 50 AM" src="https://github.com/user-attachments/assets/3b057665-c16b-4b45-aad4-79e4bc86fb30" />


<br />
<br />
Answer is 68: <br/>







