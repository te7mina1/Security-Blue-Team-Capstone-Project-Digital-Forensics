# Capstone Project Write-up : Introduction to Digital Forensics (Security Blue Team)

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image.png)

One best path of Cyber Security I really see myself through is the field of Digital Forensics. After completing this course, I additionally gained insight on command line tools (CLI) to perform investigations, but before that, I had already gained knowledge in using software tools like Autopsy and FTK Imager. Again, with determination, I was able to learn and gain new skills to my Digital Forensics journey.

## Introduction

Again, the course introduced us to how investigators collect, analyse and investigate and produce legal case documents to a law court. It also showcased the tools needed and skills on how to use these tools with correct approach in analyzing digital evidence.

## Challenge Scenario

The SOC has received an anonymous report that a user is potentially exfiltrating data from the company. An image of the user’s hard drive has been taken, and you are responsible for analyzing the contents of a perfect copy to find any evidence of malicious activity. Using your newly developed skills, search through the folders and files using techniques to uncover 4 pieces of hidden information (**each piece of evidence will contain the string {1 of 4} or similar**). You will be tested on your ability to discover this information using all of the techniques taught in this course; *Linux CLI navigation, identifying incorrect file extensions, identifying hidden files/folders, steganography, and password cracking.*

## Challenge Walk-through

First, I need to check what the main folder contains.

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%201.png)

Opening it, I realized it has 5 folders and a text file (a hidden file).

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%202.png)

Since the starting point of the project suggested we start at the “Saved Emails” directory, I decided to do a long-listing and also the “do not ignore entries starting with .” argument, “ **ls -la** “

The command listed two main files:

- Form1.jpg
- Website Update Report 01_10_2019.eml

Using the “ **file** “ command to check what type of file they are is very important in Digital Forensics.

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%203.png)

Reading the .eml file gives me some vital information I can find in the Form1.jpg image.

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%204.png)

Using the **strings** command, we can check for  **printable character sequences** from the Form1.jpg image file.

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%205.png)

After the “ **strings “** command successfully completed, it extracted some readable text from the image file.

**Simon, I have usernames and passwords for the VPN. Still on my work PC. Don’t want to risk emailing them just yet. When I do, the file is a .jpg image + password for extraction is ‘password’ . Use steghide. Talk again soon “**

> From the printed text, I had a hint about other information that can be found in a .jpg image file with the password as “password”.
> 

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%206.png)

I tried using the “ **steghide “** command on the Form1.jpg image to check for any hidden file, but turns out there is none.

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%207.png)

Since the hint tells me about a .jpg image file, I moved into the “ **Images ”** folder and then started doing my checks on the images.

The command to extract files from another file 

```bash
steghide extract -sf <file>
```

This command extracts hidden files from a target file. This technique is known as **Steganography.**

So with that being used on couple images, I was able to get a new file, “**passwords”** from the image file, “**laptop.jpg”** 

There, I got the flag, **{2/4}**

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%208.png)

Now, in that same Images directory, I decided to check for incorrect extensions, since this tactics can be used to hide the true content of a file.

First, using the “ **file** ” command to get the type of file I am dealing with. At the first red mark, we could see the **mp3** file in its original form is a **jpeg** image.

**Bad news, doing this but couldn’t get any information form it.**

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%209.png)

Next, I moved into a different directory, the “ **to-do “** directory. Before this, I have moved through different directories under the “ **WebDev work “** directory and came across this. I did a deep listing and found a .zip file.

Attempting to unzip it with the **unzip** command, issued a password entry.

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%2010.png)

Using the **fcrackzip** tool, I was able to retrieve the password for the zip file

```bash
fcrackzip -D -u -p /usr/share/wordlists/rockyou.txt .a0415ns.zip
```

The whole command use a dictionary password cracking method, associated with the **rockyou.txt** passwords to get the original password of the .zip file.

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%2011.png)

After gaining the zip file password, I was able to another file which contains employee details.

With this giving us the flag, **{1/4}.**  

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%2012.png)

A normal check on the directory path**, /WebDev work/unfinished webpages/templatemo_508_power/css/,** I came across a random file, **bootstrap.min.abc,** which gave me the answer to the other part of the challenge.

There with the flag, **{4/4}.**

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%2013.png)

Navigating into the “**Week 10”** directory, I came across two files;

- **posidon.xml**
- and **tue.txt**

Checking the format of the file, presented me with a **PNG** format.

**NOTE: The “file” command can also show the original file format.**

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%2014.png)

Converting the .xml file into a .png extension can provide us with what is missing inside.

![image.png](Capstone%20Project%20Write-up%20Introduction%20to%20Digital%20/image%2015.png)

Then, the final flag, **{3/4}**

This was the new file I had, **new.png** from converting the .xml file.

## Conclusion

Digital Forensics is one of the major skills under Defensive Security which helps to identify evidence  for use in security investigations or criminal cases. Understanding how the process of collecting evidence to analyzing it till it is presented in the court of law is an important skill set needed in Digital Forensics.

## Thank You.