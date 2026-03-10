---
title: "How to gain a Reverse Shell on Joomla"
description: "This article is a step-by-step tutorial on how to gain reverse shell of Joomla CMS."
date: "2022-05-22"
headerImage: "https://images.ctfassets.net/e16kkd4dabkb/7BKU74c5JlBNRqIQl8mqcI/7bfda3ecc9bd049caf92a27425e012f5/logo-joomla.jpg?h=250"
tags: ["projects"]
---

Joomla! is one of the most popular CMS to build web sites and powerful online applications. While working a TryHackMe room, Daily Bugle, I came across a web server which used Joomla. I successfully completed the room after gaining a reverse shell of Joomla. Write up can be found [here].

 First we need to log into the Joomla admin. The dashboard should look like this.

![joomla-dashboard-capture](//images.ctfassets.net/e16kkd4dabkb/2kmXbyTX8ZrvZtJkcUwQ35/49b347e071a8dcd276780ba6c0c41009/joomla-dashboard.png)

Click Extensions tab and select Templates in the drop down menu. We'll see the default template used for all pages. Click that template (in this case, it's protostar)

![joomla-default-theme](//images.ctfassets.net/e16kkd4dabkb/5AmVfglw5V6AUJ5AkU9Zas/86b465a3a3b06b205f6419677ab9bf86/default-template.png)

Click the Templates option on the left menu > Select the default template used for all pages.

![joomla-select-default-theme](//images.ctfassets.net/e16kkd4dabkb/f4TIToboOVsAyz5OePTpJ/a647dd3d1e8e954716b5363fa1a8ac4f/select-default-template.png)

Select index.php on the left menu, then paste the reverse shell code from 
[pentestmonkey]("https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php"). Make changes to the code so it connects back to the attacking machine.

![joomla-reverse-shell-code](//images.ctfassets.net/e16kkd4dabkb/2ZnmuDUzRdhNVBOoNW6bnu/f65084c83919526ce46f4f7ef3d3379e/reverse-shell-code.png)

Run nc listener on attacker machine.
```bash
	nc -lnvp 5555
```

Revisit the web page and we'll get the reverse shell.

![joomla-gain-reverse-shell](//images.ctfassets.net/e16kkd4dabkb/16dmpuyOSiULJUvJDo1HWr/722da15d9f2e185af786f99c46e04edd/gain-reverse-shell.png)

That's it for this tutorial. Hope you enjoy it :)
