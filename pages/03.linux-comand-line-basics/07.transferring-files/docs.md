---
title: 'Transferring files'
published: true
taxonomy:
    category:
        - docs
---

<p>FTP with a graphic user interface such as  <a href="https://filezilla-project.org/">FileZilla</a> is often the best choice for transferring files to a remote computer but sometimes for a single file or folder, scp is a good alternative. Use this command to send a zipped folder from the current folder to a folder on the remote computer.</p>
<code>
scp myfolder.zip jimmy@vdsbasic.xyz:/home/jimmy/transfer
</code>
<p>Once it is in place we can unzip it:</p>
<code>
unzip myfolder.zip
</code>
<p>It will create a directory structure but the tree will be under ~/transfer. We need to move it while preserving the file structure. The easiest way is to use rsync with the r ( recursive function)</p>

First:<br>
<code>
sudo mkdir /var/www/html/myfolder <br>
rsync -r ~/transfer/ /var/www/html/myfolder <br>
</code>
<p>The command copies all the files to the new directory. You could also use the move command here to preserve the file structure. rsync will leave the original files in place but move will not.</p>

<p>
The rsync command is a very useful one and can also be used to transfer folders from one computer to another. This  <a href="https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories-on-a-vps">page</a> goes into detail for this command.  This <a href="https://linuxize.com/post/how-to-use-scp-command-to-securely-transfer-files/">page</a> covers scp.</p>
<p>
Becoming familiar with these commands is very useful when working in Linux. Don't worry about remembering the syntax - I often google and copy. What is important is knowing which command to use.</

