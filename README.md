# The prophet TetCTF 2020 WEB

This is a write up for the prophet web challenge from TetCTF 2020, 
nice challenge and it was very fun


<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr1.png?raw=true" alt="challenge" class="inline"/>

## Write-up

For this challenge, when you access the web site for first time, you notice a link `Read some oracle here`, when you click on it, it takes to new route `http://45.77.245.232:7004/read/oracle/4.txt` and when you keep clicking on here, it's keep changing the file randomly until this nice hint showed up.

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr2.png?raw=true" alt="challenge" class="inline"/>

then i tried to change the txt file name to flag.txt and oops!

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr3.png?raw=true" alt="challenge" class="inline"/>

you can easily notice the shell prompt icon and when you click it's ask you for pin code!

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/shell.png?raw=true" alt="shell" class="inline"/>

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr4.png?raw=true" alt="challenge" class="inline"/>

it was an obvious LFI on route `/read/oracle/` so i give it a famous test `/read/oracle/../../../../../../../etc/passwd`
aannd OOPS here we go again! I found another bug!

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr12.png?raw=true" alt="challenge" class="inline"/>

i tried to read the app source code using the LFI bug, it's just a code to serve the files

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr13.png?raw=true" alt="challenge" class="inline"/>

