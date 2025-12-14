# Capstone Project Write-up : Introduction to Digital Forensics (Security Blue Team)

<img width="786" height="728" alt="image" src="https://github.com/user-attachments/assets/4689d809-4354-4183-93e2-ebb86c80ccea" />

One best path of Cyber Security I really see myself through is the field of Digital Forensics. After completing this course, I additionally gained insight on command line tools (CLI) to perform investigations, but before that, I had already gained knowledge in using software tools like Autopsy and FTK Imager. Again, with determination, I was able to learn and gain new skills to my Digital Forensics journey.

## Introduction

Again, the course introduced us to how investigators collect, analyse and investigate and produce legal case documents to a law court. It also showcased the tools needed and skills on how to use these tools with correct approach in analyzing digital evidence.

## Challenge Scenario

The SOC has received an anonymous report that a user is potentially exfiltrating data from the company. An image of the user’s hard drive has been taken, and you are responsible for analyzing the contents of a perfect copy to find any evidence of malicious activity. Using your newly developed skills, search through the folders and files using techniques to uncover 4 pieces of hidden information (**each piece of evidence will contain the string {1 of 4} or similar**). You will be tested on your ability to discover this information using all of the techniques taught in this course; *Linux CLI navigation, identifying incorrect file extensions, identifying hidden files/folders, steganography, and password cracking.*

## Challenge Walk-through

First, I need to check what the main folder contains.


<img width="729" height="298" alt="image" src="https://github.com/user-attachments/assets/f5f17134-ce8c-4b41-af37-72ca42a3650d" />

Opening it, I realized it has 5 folders and a text file (a hidden file).


<img width="1216" height="193" alt="image" src="https://github.com/user-attachments/assets/40b9ecd3-e584-4969-be17-2128bb6509ed" />

Since the starting point of the project suggested we start at the “Saved Emails” directory, I decided to do a long-listing and also the “do not ignore entries starting with .” argument, “ **ls -la** “

The command listed two main files:

- Form1.jpg
- Website Update Report 01_10_2019.eml

Using the “ **file** “ command to check what type of file they are is very important in Digital Forensics.


<img width="727" height="376" alt="image" src="https://github.com/user-attachments/assets/8e921222-a7be-4725-b827-feedb337cb08" />

Reading the .eml file gives me some vital information I can find in the Form1.jpg image.


<img width="940" height="64" alt="image" src="https://github.com/user-attachments/assets/19b79337-c6ca-489e-96af-85cac84aa645" />

Using the **strings** command, we can check for  **printable character sequences** from the Form1.jpg image file.


<img width="1449" height="82" alt="image" src="https://github.com/user-attachments/assets/61c14384-e656-43e4-b9f8-b93c370e5567" />

After the “ **strings “** command successfully completed, it extracted some readable text from the image file.

**Simon, I have usernames and passwords for the VPN. Still on my work PC. Don’t want to risk emailing them just yet. When I do, the file is a .jpg image + password for extraction is ‘password’ . Use steghide. Talk again soon “**

> From the printed text, I had a hint about other information that can be found in a .jpg image file with the password as “password”.
>


<img width="1104" height="63" alt="image" src="https://github.com/user-attachments/assets/29f689c3-0414-4ee3-81e5-af3c7c43052a" />

I tried using the “ **steghide “** command on the Form1.jpg image to check for any hidden file, but turns out there is none.


<img width="1060" height="769" alt="image" src="https://github.com/user-attachments/assets/c6ab56d0-c6d8-44b2-8a72-58bc0eea182b" />

Since the hint tells me about a .jpg image file, I moved into the “ **Images ”** folder and then started doing my checks on the images.

The command to extract files from another file 

```bash
steghide extract -sf <file>
```

This command extracts hidden files from a target file. This technique is known as **Steganography.**

So with that being used on couple images, I was able to get a new file, “**passwords”** from the image file, “**laptop.jpg”** 

There, I got the flag, **{2/4}**


<img width="1093" height="361" alt="image" src="https://github.com/user-attachments/assets/01d46167-b521-4d39-95bc-076dab7130c9" />

Now, in that same Images directory, I decided to check for incorrect extensions, since this tactics can be used to hide the true content of a file.

First, using the “ **file** ” command to get the type of file I am dealing with. At the first red mark, we could see the **mp3** file in its original form is a **jpeg** image.

**Bad news, doing this but couldn’t get any information form it.**


<img width="1190" height="215" alt="image" src="https://github.com/user-attachments/assets/77f6d176-9ac3-4650-9e4e-ac67573b3bd6" />

Next, I moved into a different directory, the “ **to-do “** directory. Before this, I have moved through different directories under the “ **WebDev work “** directory and came across this. I did a deep listing and found a .zip file.

Attempting to unzip it with the **unzip** command, issued a password entry.


<img width="1464" height="136" alt="image" src="https://github.com/user-attachments/assets/89c6282c-4ee9-4b4f-b21c-608cba1b4ae5" />

Using the **fcrackzip** tool, I was able to retrieve the password for the zip file

```bash
fcrackzip -D -u -p /usr/share/wordlists/rockyou.txt .a0415ns.zip
```

The whole command use a dictionary password cracking method, associated with the **rockyou.txt** passwords to get the original password of the .zip file.


<img width="1204" height="406" alt="image" src="https://github.com/user-attachments/assets/548c26fd-2b7b-4902-9b91-bced39a55897" />

After gaining the zip file password, I was able to another file which contains employee details.

With this giving us the flag, **{1/4}.** 


<img width="1218" height="628" alt="image" src="https://github.com/user-attachments/assets/fd74992d-5f44-4c96-bf8c-b9217f6e46ba" />

A normal check on the directory path**, /WebDev work/unfinished webpages/templatemo_508_power/css/,** I came across a random file, **bootstrap.min.abc,** which gave me the answer to the other part of the challenge.

There with the flag, **{4/4}.**


<img width="1279" height="331" alt="image" src="https://github.com/user-attachments/assets/f100dc89-a378-452e-84cb-5aa37c0106b6" />


Navigating into the “**Week 10”** directory, I came across two files;

- **posidon.xml**
- and **tue.txt**

Checking the format of the file, presented me with a **PNG** format.

**NOTE: The “file” command can also show the original file format.**


<img width="1141" height="149" alt="image" src="https://github.com/user-attachments/assets/9f4f98d2-fab8-47a1-bab1-91a84a264fa2" />


Converting the .xml file into a .png extension can provide us with what is missing inside.


<img width="848" height="395" alt="image" src="https://github.com/user-attachments/assets/ae71ccfb-685f-4f58-b758-7661bec7a867" />

Then, the final flag, **{3/4}**

This was the new file I had, **new.png** from converting the .xml file.


## Conclusion

Digital Forensics is one of the major skills under Defensive Security which helps to identify evidence  for use in security investigations or criminal cases. Understanding how the process of collecting evidence to analyzing it till it is presented in the court of law is an important skill set needed in Digital Forensics.

## Thank You.
