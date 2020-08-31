---
title: 'Unattended Upgrades'
published: true
---

If security patches are not applied, your server can be exploited. These patches can be applied automatically.

<pre>
sudo apt-get install unattended-upgrades -y
sudo apt-get install update-notifier-common -y
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
</pre>
Change the line "Unattended-Upgrade::Automatic-Reboot" by removing any leading slashes and changed false to true.
   
<pre>   
// Automatically reboot *WITHOUT CONFIRMATION*
//  if the file /var/run/reboot-required is found after the upgrade
Unattended-Upgrade::Automatic-Reboot "true";
</pre>
Some more information is available [here](https://www.techrepublic.com/article/how-to-enable-automatic-security-updates-for-ubuntu-server/).

When I logged on and before making thos change I was told
65 packages can be updated.
46 updates are security updates.

The change should now keep my server up to date.

