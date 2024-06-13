---
created: 2023-09-10T00:04:54 (UTC -04:00)
tags: 
source: https://learn.snyk.io/lesson/prototype-pollution/
author: null
dg-publish: true
---

# What is unrestricted file upload? | Tutorial & examples | Snyk Learn

> ## Excerpt
> Learn about the dangers of file uploads and the inefficiently restricted file uploads with dangerous types. Learn to mitigate and fix the vulnerability from experts.

---
### What is the impact of unrestricted upload of file with dangerous type?

The impact varies based on several factors. The most important two are:

-   Are the uploaded files accessible? For example, via the `/static/` folder on the web application
-   What is the backend using? For example, the impact with NodeJS is limited because you can not get a file to execute, unlike other languages or frameworks such as PHP

Given that the uploaded file is accessible, such as in the scenario of BigCorp, and the backend is using NodeJS, then an attacker can host malicious files on the server, and perform stored cross-site scripting attacks.

The term "malicious files" encompasses many different file types, not just server-side scripts. It could be malicious .exe files, Microsoft Word documents with malicious macros, ransomware, spyware, etc.

Another common scenario is when an attacker wants to phish a victim with a Word document that contains a macro. The attacker may use a vulnerable file upload function to get the malicious Word document onto the victim's computer, and then just link to the document in a message.

FUN FACT

### Directory traversal + unrestricted uploads

Sometimes unrestricted file upload can be escalated by utilizing a directory traversal bug. In the example of BigCorp, the `avatar.name` variable is user supplied. A malicious file name such as `../../file.txt` might result in the server storing the file at `/static/avatars/423543/../../file.txt`, which translates to `/static/file.txt`. Learn more about [directory traversal vulnerabilities](https://learn.snyk.io/lesson/directory-traversal/).

![](https://res.cloudinary.com/snyk/image/upload/v1642680335/snyk-learn/patchonaut.svg)
