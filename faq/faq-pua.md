# FAQ-PUA #

## Potentially Unwanted Applications ##

<strong>PUA - Possibly Unwanted Applications</strong>

ClamAV supports the detection of so called PUAs. At the moment the
following categories are available:

<em>Packed</em>

This is a detection for files that use some kind of runtime packer. A
runtime packer  can be used to reduce the size of executable files
without the need for an external unpacker. While this can‘t be
considered malicious in general, runtime packers are widely used with
malicious files since they can prevent a already known malware from
detection by an Antivirus product.

<em>PwTool</em>

Password tools are all applications that can be used to recover or
decrypt passwords for various applications - like mail clients or
system passwords. Such tools can be quite helpful if a password is
lost, however, it can also be used to spy out passwords.

<em>NetTool</em>

Applications that can be used to sniff, filter, manipulate or scan
network traffic or networks.  While a networkscanner - for example -
can be a extremely helpful tool for admins, you may not want to see an
average user playing arround with it. Same goes for tools like netcat
and the like.

<em>P2P</em>

Peer to Peer clients can be used to generate a lot of unwanted traffic
and sometimes it happens that copyrights are violated by downloading
copyright protected content (Music, Movies) - therefore we consider
them possibly unwanted as well.

<em>IRC</em>

IRC Clients can be a productivity killer and depending on the client -
a powerful platform for malicious scripts (take mIRC for example).

<em>RAT</em>
Remote Access Trojans are used to remotely access systems, but can be used also by system admins, for example VNC or RAdmin

<em>Tool</em>
General system tools, like process killers/finders

<em>Spy</em>
Keyloggers, spying tools

<em>Server</em>
Server based badware like DistributedNet

<em>Script</em>
Known "problem" scripts written in Javascript, ActiveX or similar.
