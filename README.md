<h1>Investigating-With-Splunk</h1>


<h2>Description</h2>
SOC Analyst Johny has observed some anomalous behaviours in the logs of a few windows machines. It looks like the adversary has access to some of these machines and successfully created some backdoor. His manager has asked him to pull those logs from suspected hosts and ingest them into Splunk for quick investigation. Our task as SOC Analyst is to examine the logs and identify the anomalies. 

<h2>Questions</h2>

- <b>How many events were collected and Ingested in the index main?</b>
- <b>On one of the infected hosts, the adversary was successful in creating a backdoor user. What is the new username?</b>
- <b>On the same host, a registry key was also updated regarding the new backdoor user. What is the full path of that registry key?</b>
- <b>Examine the logs and identify the user that the adversary was trying to impersonate.</b>
- <b>What is the command used to add a backdoor user from a remote computer?</b>


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
On the same host, a registry key was also updated regarding the new backdoor user. What is the full path of that registry key?


<p align="center">
We now know which device the user A1berto was created on. <img width="1440" alt="Screenshot 2025-04-15 at 12 42 58 PM" src="https://github.com/user-attachments/assets/410b3f31-0e99-452d-a84d-25bf5fcd724b" /> <br/>

  
We add Micheal.Beaven in the seach bar
<img width="1440" alt="image" src="https://github.com/user-attachments/assets/7ad5b04f-82e5-441f-88ac-cde3c3f14371" />

  
To find the answer I did a few things. Frist, I google for the event id that was associated with registry modification. The event id I received and inputted resulted in zero events. I removed the attempted event id search and instead, I then added the user "A1berto" into the search bar since the user "A1berto" is still the topic. It helped to reduced the number of events low enough where I wouldn't mind scrolling and reading through the events to find the answer. However, I want to be a bit more precise. I looked through the fields and found "extracted_EventType" which caught my eye. I clicked it and saw CreateKey so I added that into my search resulting in 1 event.
<img width="1440" alt="Screenshot 2025-04-15 at 1 11 52 PM" src="https://github.com/user-attachments/assets/7ee146cd-e597-4777-9b80-ad9c98b649d2" />
<img width="1440" alt="image" src="https://github.com/user-attachments/assets/5025cc9a-d8b0-4bde-98bd-2e1cbc0edc0d" />


I scrolled down and under the field "Target_Object" the answer appears.
<img width="1440" alt="Screenshot 2025-04-15 at 1 16 33 PM" src="https://github.com/user-attachments/assets/21d3fae0-3243-4c74-99de-b65a400e6c24" />




<br />
<br />
Answer is HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto <br/>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>
Examine the logs and identify the user that the adversary was trying to impersonate.

<p align="center">
The answer was dicussed in an earlier question. User A1berto is impersonating Alberto


<br />
<br />
Answer is Alberto <br/>



<h2>Program walk-through</h2>

<b>Answer the question below <br/>

What is the command used to add a backdoor user from a remote computer?

<p align="center">
I knew to look for an executable file(.exe). Since the question asked what command was used, I looked the a field containing the word command and "CommandLine" appeared. I did a quick look at it and saw one that looked suspicious. I decided to seach the field "CommandLine" as a wildcard just to not overlook anything. Also, I once again added user "A1berto" to focus the search. <br/>
<img width="1440" alt="image" src="https://github.com/user-attachments/assets/c295758a-0cf0-42ab-be0c-7f12cb61cc74" />

  
With 7 results I looked through them all. Looking at each CommandLine field within the events, the one that had looked suspicious to me was the answer to the question.
<img width="1440" alt="Screenshot 2025-04-15 at 1 37 12 PM" src="https://github.com/user-attachments/assets/8d058594-06b9-4abf-b23c-5d9dbc1f936f" />

After looking at how others solved this question, there is a more efficient way. There is an event id associated with program execution (Event ID 4688)
<img width="1440" alt="image" src="https://github.com/user-attachments/assets/1b2a15b3-7489-4844-831e-0e2871c29a48" />
The full search should be " index=main EventID=4688 A1berto ". Look at the CommandLine field and you'll find the same answer. Through this search I learned that WMIC is a software utility that allows users to perform Windows Management Instrumentation operations with a command prompt. That ransomware authors have been seen to use wmic.exe to gain access to remote systems and then perform processes on it to prepare for or execute the ransomware attack.
<img width="1440" alt="Screenshot 2025-04-15 at 1 50 01 PM" src="https://github.com/user-attachments/assets/ad356081-fbc7-434e-ad31-fe34cf339e34" />






<br />
<br />
Answer is C:\windows\System32\Wbem\WMIC.exe" /node:WORKSTATION6 process call create "net user /add A1berto paw0rd1 <br/>






