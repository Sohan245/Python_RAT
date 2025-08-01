
## My Findings

1. Unauthorized software download due to weak endpoint security configurations.
2. Unauthorized script execution allowed by improper script execution policies.
3. Successful evasion of Microsoft Defender and Intune using steganography.
4. Cross-Origin Resource Sharing (CORS) abuse due to improper access control implementation.
5. Installation and execution of blocked applications and utilities due to weak EDR configurations.

> **Note**: This blog focuses on how I utilized Python to gain Remote Access Toolkit (RAT) persistence.<br>
> Python is really venomous!

---

## Introduction

Hi! I'm a cybersecurity enthusiast currently (at the time of writing this blog) pursuing my Master's degree in Cybersecurity.

I was fortunate—and pleasantly surprised—to land an internship without an interview. Special thanks to my professor, [Wm. Arthur (Art) Conklin](https://www.linkedin.com/in/waconklin/), for referring me.

My internship at [Third Coast](https://www.linkedin.com/company/third-coast/) began in mid-2025. Initially, I was assigned to perform a cybersecurity audit to evaluate the company's internal security posture and gain practical exposure.

> *What began as a routine audit turned into an eye-opening lesson on trusted sources and endpoint evasion.*

---

## Setting the Stage

The first month of the internship was dedicated to auditing, allowing me to understand real-world security control implementations, the internal workings of the organization, and to get acclimated. Later, I transitioned to ICS security controls and penetration testing.

On day one, I informed my manager, [Brent Foster](https://www.linkedin.com/in/brent-foster-14463017/), of my interest in penetration testing and red team operations. I had no idea then that Brent was an expert in this domain—a runner-up at DEF CON's ICS Village hacking competition. He granted me permission to pursue penetration testing efforts as long as I kept him informed.

The main objective of my internship was simple: to learn and apply something meaningful in a real-world context.

> *If you want something, just ASK!*

---

## The Discovery: Endpoint Security Misconfiguration

I initially tried a variety of penetration testing techniques I had learned through certification courses and CTF platforms—with little success.
Let's just say, it was me vs. "Brent-level security."

With no low-hanging fruit found on the domain controllers, I turned my attention to host machines. A few days in, I noticed that all software installations required admin credentials—but oddly, it was prompting me for those credentials on **my** system. That raised a red flag.

I initially intended to use a keylogger and trick an admin into authorizing the install, but soon realized that wouldn't work. Still, I needed Python.

I typed `which python` in PowerShell—no response. I couldn’t download it via a browser either. Out of curiosity, I typed `python.exe` directly in PowerShell, expecting an access-denied error.

**To my surprise, PowerShell silently pulled Python from the Microsoft Store—no admin authentication required!**

I tested this with other Microsoft Store applications as well. It worked. I immediately reported this to Brent, and the Microsoft Store was subsequently blocked. Since I already had Python installed, Brent allowed me to continue using it to explore what I could accomplish under the MITRE ATT&CK framework.

I began with a simple Python-based keylogger, but results were limited. I moved on to testing reverse shells using [revshells](https://www.revshells.com). Naming my file `ping_me.py`, I found that Windows Defender flagged and deleted it. However, executing the same code directly in the PowerShell environment—in-memory—worked flawlessly.

> *Have you tried asking yourself "WHY"?*

---

## Establishing the RAT

After establishing a successful reverse shell, Brent suggested pushing further—create a Remote Access Toolkit (RAT) using steganography.

Taking his advice, I integrated an existing open-source [Python RAT](https://github.com/FZGbzuw412/Python-RAT/tree/main). With this, I managed to establish a fully functional Python-based RAT without triggering Defender, antivirus software, or EDR systems.

<img width="1604" height="804" alt="proof1" src="https://github.com/user-attachments/assets/ecdbc15d-b22c-4f79-9e98-e66a1eaab789" />

Still, I wasn't satisfied. A `.py` file execution isn't stealthy or realistic in an actual attack scenario. So, I used `PyInstaller` to convert the Python script into a `.exe` file for better believability.

---

## Persistence & Automation

The `.exe` file worked, but required manual execution to establish the RAT connection. That wasn’t practical.

To maintain persistence, I developed `start_up.py`, which programmatically added the `test_exe.exe` file to the system's startup programs. Interestingly, I couldn't add it manually, but the script succeeded—a testament to privilege gaps and inconsistent security policies.

<img width="831" height="311" alt="workflow" src="https://github.com/user-attachments/assets/8559e98e-db82-4f3f-bda3-c8b7671bc6ff" />

#### {PROOF.TXT}

<img width="1604" height="1155" alt="proof2" src="https://github.com/user-attachments/assets/b527e206-6a4b-4680-b1b5-4c0da912709e" />

---

## Lessons & Takeaways

- Always assume **nothing is fully secure**, even trusted sources.
- Endpoint Detection and Response (EDR) tools can miss creative evasion techniques like steganography.
- Chaining overlooked misconfigurations can result in powerful real-world exploits.
- Trusted sources like Microsoft Store can be weaponized if not properly restricted.

---

## Final Thoughts

- Huge thanks to my supervisor, [Brent Foster](https://www.linkedin.com/in/brent-foster-14463017/), for his mentorship and trust.
- Grateful to [Third Coast](https://www.linkedin.com/company/third-coast/) for the opportunity and real-world exposure.
- Special thanks to the open-source community—your tools and contributions made this possible.

Feel free to connect with me on [LinkedIn](https://www.linkedin.com/in/sohan-simha-p/). Thank you!

