## Introduction

If you are a Python developer then you are aware of [pip](https://pypi.org/project/pip/). However, were you aware of the potential [malware](https://en.wikipedia.org/wiki/Malware) threat associated with Python's recommended package-management system? This article will discuss the security threats associated with pip and what you can do to protect yourself against them.

## What is PIP?

_Package Installer for Python_ or (pip) is the de facto and recommended package-management system written in Python and is used to install and manage software packages. It connects to an online repository of public packages, called the Python Package Index(PyPI).
For example, let's say you want to install the `request` module. You would use the following syntax:

```cmd
pip install request
```
This command will download the source code of the `request` package and install it into your local Python environment, allowing you to utilize its functionality.

## PIP's Vulnerabilities

In general practice, Python developers will usually upload secure and ethical code to the PyPI repository. However, you would be surprised to know, there are no third-party checks on the code that is uploaded to PyPI. The only restriction is that once a package name exists, only the maintainer(s) can upload packages with that name. Meaning you can't submit a package using an already established name.

Unfortunately, this security feature can be exploited. In 2016, [research](https://incolumitas.com/2016/06/08/typosquatting-package-managers/) proved that PyPI could be exploited through [typosquatting](https://en.wikipedia.org/wiki/Typosquatting). The researcher uploaded some harmless "simulation malware" to PyPI under names that were misspelled versions of popular package names, in order to collect data on how often these misspelled packages were installed. Now, if a black-hat hacker executed this task, then they could have used a much more malicious script.

## Recent Events

In 2022, a group of [school-age hackers](https://www.darkreading.com/threat-intelligence/school-kid-uploads-ransomware-scripts-to-pypi-repository-as-fun-research-project) based in Verona, Italy  uploaded multiple malicious Python packages containing ransomware scripts to PyPI. 
The packages were named "`requesys`," "`requesrs`," and "`requesr`," which are all common typosquats of "`requests`" — a legitimate and widely used HTTP library for Python.

According to the researchers at Sonatype who detected the malicious code on PyPI, one of the packages (`requesys`) was downloaded about 258 times — presumably by developers who made typographical errors when attempting to download the real "`requests`" package.

One version of the `requesys` package contained the encryption and decryption code in plaintext Python. But a subsequent version contained a Base64-obfuscated executable that made analysis a little harder, according to Sonatype.

### No Threat Detected

Developers who ended up with their system encrypted received a pop-up message instructing them to contact the author of the package for the decryption key. 

Victims were able to obtain the decryption key without having to make a payment for it, Sonatype says. "And that makes this case more of a gray area rather than outright malicious activity," Sonatype concludes. Information on the hacker's Discord channel shows that at least 15 victims had installed and run the package.

Sonatype discovered the malware on July 28 and immediately reported it to PyPI's administrators, the company says. Two of the packages have since been removed and the hacker has renamed the `requesys` package, so developers no longer mistake it for a legitimate package.


## Conclusion

Hopefully after reading this article, you now realize why it is important to pay close attention to what you download from public code repositories such as PyPI. Security researchers state that organizations must pay closer attention to their software supply chains — especially when it comes to using open source software from public repositories such as PyPI.

If you are ever the victim of a ransomware attack, always remember, `paying the ransom` does `not guarantee` you will get the private key to restore your data. Therefore, it behooves you to take preemptive measure to protect your files in your day-to-day operations. Remember, as a Python developers, it is always your responsibility to ensure your packages are secure. Be very careful when typing out the names of popular libraries, as `typosquatting` is one of the most common methods for this exploitation. Additionally, maintain backups, and [data reduancy](https://www.techtarget.com/searchstorage/definition/redundant) just as a precautionary measure.


If you found this article helpful please leave a `like`, or have any questions, leave a comment. 
