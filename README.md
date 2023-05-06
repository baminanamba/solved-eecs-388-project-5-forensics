Download Link: https://assignmentchef.com/product/solved-eecs-388-project-5-forensics
<br>
This is a group project; you will work in teams of two and submit one project per team. Please find a partner as soon as possible. If you have trouble forming a team, post to Piazza’s partner search forum.

Strict <u>no-leaks </u>policy. In this project, you play the role of a computer forensic analyst working to solve a murder case. Since you don’t want to be fired for jeopardizing an ongoing criminal investigation, you need to follow a strict policy on collaboration. <em>You are bound by the Honor Code not to communicate with anyone regarding any aspect of the case or your investigation (other than within your group or with course staff)</em>. The number of pieces of evidence you find, the techniques you try, how successful said techniques are, the general process you follow, etc. are all considered part of your solution and must not be discussed with members of other groups.

Start early. It may be impossible to complete this project before the deadline unless you begin several days beforehand. Please plan accordingly.

Solutions must be submitted electronically via Canvas, following the submission checklist below. Please coordinate carefully with your partner to make sure at least one of you submits on time.

<h1>Introduction</h1>

In this project, you will play the role of a forensic analyst and investigate a murder mystery. On

Thanksgiving 2009, a terrible crime occurred on campus. Hapless Victim, a leading figure in the university community, was shot while working in the EECS building. Victim was last seen alive on November 25, 2009, shortly before midnight, and was discovered dead at approximately 6 a.m. the next morning. Officers recovered the projectile shown below (Exhibit A), which appears, inexplicably, to have been the cause of death.

The case went cold on December 1, when the leading suspect, Nefarious Criminal, fled the country and disappeared. Officers seized their computer, but the hard disk was encrypted and investigators

were unable to crack the password. No further evidence could be found.

There was finally a break in the case last week, when Nefarious Criminal was picked up by authorities on a beach in South America. Among their possessions was a Post-It note with the hard disk password on it. We have decrypted the drive and made it available for your analysis.

Your job is to conduct a forensic examination of the disk image and document any evidence related to the murder. If you find sufficient evidence, Nefarious Criminal will be extradited and face trial.

Exhibit A—Projectile recovered at the crime scene. Ballistics experts have identified it as a “Nerf blaster dart.”

Objectives:

<ul>

 <li>Understand how computer use can leave persistent traces and why such evidence is often difficult to remove or conceal.</li>

 <li>Gain experience applying the security mindset to investigate computer misuse and intrusion.</li>

 <li>Learn how to retrieve information from a disk image without booting the operating system, and understand why this is necessary to preserve forensic integrity.</li>

</ul>

<h1>Getting Started</h1>

The tools and techniques you use for your investigation are up to you, but here are some suggestions to help you get started.

General Knowledge A general working knowledge of Linux is undoubtedly helpful for this project. If you don’t have this yet, you may need to spend time Googling and/or experimenting to get up to speed. The TA will also answer general Linux questions as a last resort. For an excellent reference book, try <em>UNIX and Linux System Administration Handbook </em>by Nemeth, Snyder, Hein, and Whaley. See <a href="https://en.wikipedia.org/wiki/Disk_partitioning">http://en.wikipedia.org/wiki/Disk_partitioning</a> for some additional background.

Live Analysis   Live analysis is a forensic technique in which the investigator examines a running copy of the target system. We suggest using VirtualBox for this purpose.

<ol>

 <li>Download the raw disk image (1.2 GB): <a href="https://eecs388.org/*/forensics_release_w17.raw.gz">https://eecs388.org/*/forensics_release_w17.raw.gz</a></li>

 <li>Decompress the disk image. Note: some browsers seem to automatically decompress after having downloaded the file. Check to see if the file size is about 4.3 GB; if so, it has been decompressed already. Otherwise, decompress the image.</li>

 <li>Convert the raw disk image to a VirtualBox disk image:</li>

</ol>

VBoxManage convertdd forensics_release.raw forensics.vdi –format VDI

<ol start="4">

 <li>Use the VirtualBox GUI to create a new VM. Select Linux / Ubuntu as the machine type. Select “Use an existing virtual hard disk file” and select the VDI you just created.</li>

 <li>Start the VM and explore the system.</li>

</ol>

Dead Analysis  In dead analysis, the forensic investigator examines data artifacts from a target system without the system running. We suggest trying dead analysis with the Autopsy open-source forensics tool. The procedure below assumes you are working on Ubuntu Linux. (If you like, you can reuse the VM from the previous project.) Autopsy will also run on Windows and OS X.

<ol>

 <li>Install the Autopsy digital forensics suite:</li>

</ol>

$ sudo apt-get install autopsy

<ol start="2">

 <li>Launch Autopsy in the background and open the browser-based GUI:</li>

</ol>

$ sudo autopsy &amp;

In a browser on the local machine, go to the URL <a href="http://localhost:9999/autopsy">http://localhost:9999/autopsy.</a>

<ol start="3">

 <li>Create a new case and add the disk image:

  <ul>

   <li>Click New Case. Enter a case name and click New Case.</li>

   <li>Go back to <a href="http://localhost:9999/autopsy">http://localhost:9999/autopsy</a> and open the case you created.</li>

   <li>Click Add Host. Enter a host name and click Add Host.</li>

   <li>Click Add Image. Click Add Image File. Enter the path to the decompressed raw disk image. Make sure you select Type=Disk and Import Method=Symlink. Click Next.</li>

   <li>Leave the Image File Details and File System Details as the defaults. (Note that the disk image contains 3 partitions, which Autopsy will allow you to examine separately.) Click Add. Click OK.</li>

   <li>Select a partition to examine and click Analyze. The buttons at the top give you several analysis tools. Try File Analysis and Keyword Search to get started.</li>

  </ul></li>

 <li>In addition to hints dropped elsewhere, here is an incomplete list of things to try:

  <ul>

   <li>Examine the system logs.</li>

   <li>Check for deleted or encrypted files.</li>

   <li>Search the drive image for strings that may indicate relevance to your investigation.</li>

  </ul></li>

</ol>

Password Cracking      Password crackers may be helpful in trying to brute-force decrypt passwordprotected files. John the Ripper <a href="https://www.openwall.com/john/">(http://www.openwall.com/john/)</a> is the canonical Unix password cracker. Hydra <a href="http://www.thc.org/thc-hydra/">(http://www.thc.org/thc-hydra/)</a> is a tool used to brute force remote login passwords, fcrackzip <a href="http://home.schmorp.de/marc/fcrackzip.html">(http://home.schmorp.de/marc/fcrackzip.html)</a> is a ZIP password cracker, and pdfcrack <a href="https://sourceforge.net/projects/pdfcrack/">(http://sourceforge.net/projects/pdfcrack/)</a> is a PDF password cracker. John, fcrackzip, and pdfcrack are conveniently available in the Debian package repositories and can be installed with apt-get.

When using a password cracker, it is wise to make sure that the password is not susceptible to a dictionary attack and does not use a restricted character set (e.g., lowercase letters, letters only, letters and numbers only) before spending time on a full brute-force crack. It is also a good idea to crack a very vulnerable password first to make sure you are using the tool correctly.

<h1>Tasks and Deliverables</h1>

The deliverables for this project are your answers to the questions below. Your answers should be <em>complete </em>but <em>concise</em>. None of the questions should require more than 1–2 paragraphs to answer.

For each prompt, explain the investigatory methods you used and the evidence that supports your conclusion. Place your responses in a plain text file called report.txt. If you recover files that are relevant to your responses, mention them by name and include them with your submission in a directory named evidence/.

<ol>

 <li>Try booting the suspect’s machine and using it normally. What <em>specific </em>behaviors of this machine make this a bad idea? Attach relevant evidence.</li>

 <li>What operating system does the suspect use? Be careful and specific; e.g., say “Windows 2000” instead of just “Windows.”</li>

 <li>What is the username of the account typically used by the suspect?</li>

 <li>Are there any indications that the suspect had an accomplice who was physically present on the night of the crime? Attach relevant evidence.</li>

 <li>Were there any suspicious-looking encrypted files on the machine? If so, please attach them and their decrypted contents as evidence and briefly describe how you obtained the contents.</li>

 <li>Are there any indications that the suspect owned or was researching weapons of the kind involved in the murder? Attach the specific evidence and give a brief explanation.</li>

 <li>Did the suspect try to delete any files before their arrest? Please attach the name(s) of the file(s) and any indications of their contents that you can find. (Hint: We will be impressed if you manage to recover the <em>original </em>contents of a particular incriminating file, but we do not expect you to do so.)</li>

 <li>Reconstruct the timeline of actions by the suspect that may be relevant to the investigation. (Make a list in this format: &lt;date&gt; &lt;time&gt;: &lt;event description&gt;.) Include any activities related to your other responses, if you can identify when they occurred. Include each time the suspect logged in to or booted the machine to do something interesting. When was the last activity before the suspect fled the country?</li>

 <li>Is there anything else on the computer that would imply the suspect had malicious intents? Attach relevant evidence as appropriate.</li>

</ol>

As you investigate, be on the lookout for evidence of any other machines or network services that the suspect may have used. These may contain important evidence and raise further questions you’ll need to investigate (<em>hint, hint!</em>). Before attempting to access any such machines or accounts, be sure to contact your supervisor for permission by emailing <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1673737565252e2e3b6664797c2356637b7f757e3873726338">[email protected]</a> Failure to ask permission is guaranteed to be a waste of your time and may violate the course ethics policy. Again, start early; headquarters has been known to take up to 24 hours to approve such requests.

<h1>Policies and Hints</h1>

Collaboration: Strictly prohibited outside your group.        As stated above, you are bound by the

Honor Code not to communicate with anyone regarding any aspect of the case or your investigation (other than within your group or with course staff). The number of pieces of evidence you find, the techniques you try, how successful said techniques are, the general process you follow, etc. are all considered part of your solution and must not be discussed with members of other groups. If anyone brings up the project, start yelling “LALALALA” and refer them to your supervisor for an official spokesperson.

If you get stuck… Requesting Hints    Given the nature of this assignment and its strict collaboration policy, HQ recognizes the need for some hints. We have standard hints for each question. If your group gets stuck, email <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7217171101414a4a5f02001d184732071f1b111a5c171607">[email protected]</a> with the names of your group members, the question for which you would like a hint, and the progress you have made thus far on that question.

To help you get started, hints on the first three questions are free (and so are requests to access remote machines). After that, each group may receive a maximum of 3 hints, and we will enforce a one-hour delay between hints for each group. Purely administrative questions or general questions about Linux do not count towards this limit.

<h1>Submission Checklist</h1>

Upload to Canvas a gzipped tar file named project5.<em>uniqname1</em>.<em>uniqname2</em>.tar.gz that contains only the files listed below. These will be autograded, so make sure you have the proper filenames, formats, and behaviors. You can generate the tarball at the shell using this command:

tar -zcf project5.<em>uniqname1</em>.<em>uniqname2</em>.tar.gz report.txt evidence/

The tarball should contain only the files below:

report.txt      A plain text file with your answers to the numbered prompts. evidence/    A directory containing any recovered files referenced in your report.