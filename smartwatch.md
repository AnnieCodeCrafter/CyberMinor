# Pentest IoT – Smartwatch
## VivoActive 3 Music
![Smartwatch]( img/watchimg/Capture.png)
I wanted to try and pentest something IoT, because it would be relevant to the proftaak as well. I decided to pentest a smartwatch – it would offer a suitable challenge, and the group had already pentested different watches, although there weren’t a lot of findings around. I chose the VivoActive 3 Music, because it seemed interesting. It could connect to Bluetooth and wifi, and you can upload your own music files to the watch as well as sync with the files on your phone. 

However, I could find very little wrong with the watch. I couldn’t find any big flaws. The first thing that caught me off-guard was that it didn’t use an IP address; instead, it uses a wifi-mac address that acts quite differently. I noticed this with Wireshark. This also means that I couldn’t pick it up with Nmap, and if it did pick it up, it wouldn’t have any ports open. 

I also tried to scan for Bluetooth. However, you’d need a special device to do so, and I didn’t have access to that, so I couldn’t follow up with that. 

All in all I’m disappointed that I couldn’t find anything substantial, but it was still a valid learning experience. You can view the complete document [here](resources/smartwatchdone.pdf).
