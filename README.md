# The prophet TetCTF 2020 WEB

This is a write up for the prophet web challenge from TetCTF 2020, 
a nice challenge and it was very fun


<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr1.png?raw=true" alt="challenge" class="inline"/>

## Write-up

For this challenge, when you access the web site for the first time, you notice a link `Read some oracle here`, when you click on it, it takes to new route `http://45.77.245.232:7004/read/oracle/4.txt` and when you keep clicking on here, it keeps changing the file randomly until this nice hint showed up.

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr2.png?raw=true" alt="challenge" class="inline"/>

then I tried to change the text file name to flag.txt and oops!

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr3.png?raw=true" alt="challenge" class="inline"/>

you can easily notice the shell prompt icon and when you click it asks you for pin code!

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/shell.png?raw=true" alt="shell" class="inline"/>

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr4.png?raw=true" alt="challenge" class="inline"/>

it was an obvious LFI on route `/read/oracle/` so i give it a famous test `/read/oracle/../../../../../../../etc/passwd`
aannd OOPS here we go again! I found another bug!

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr12.png?raw=true" alt="challenge" class="inline"/>

I tried to read the app source code using the LFI bug, it's just a code that serves the files randomly

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr13.png?raw=true" alt="challenge" class="inline"/>

the debugger that they are using is named: Werkzeug, so we take a look on its code on GitHub and we found the function that gets the code pin.

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr6.png?raw=true" alt="challenge" class="inline"/>

We took another look at the function `get_pin_and_cookie_name` that gets the pin and cookie name, it's job is described very well in its documentation.

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr7.png?raw=true" alt="challenge" class="inline"/>

after reading and understanding the function above, we end up writing a script to generate the pin code, and to generate it we need this data:

- username
- modname
- getattr(app, '__name__', getattr(app.__class__, '__name__'))
- getattr(mod, '__file__', None)
- str(uuid.getnode()),
- get_machine_id(),

we get the username from /etc/password: web3_user

modname from debugger error: flask.app

getattr(app, '__name__', getattr(app.__class__, '__name__')) from the app source code: Flask

getattr(mod, '__file__', None) from  debugger error: /usr/local/lib/python3.5/dist-packages/flask/app.py

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr8.png?raw=true" alt="challenge" class="inline"/>

str(uuid.getnode()) the address mac of the network interface: 56:00:02:7a:23:ac

we get the network interfaces using LFI: ens3

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr16.png?raw=true" alt="challenge" class="inline"/>

then we get the mac address: 56:00:02:7a:23:ac 

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr19.png?raw=true" alt="challenge" class="inline"/>

and convert it to the decimal value:

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr20.png?raw=true" alt="challenge" class="inline"/>

we get the machine id using LFI: `/etc/machine-id`

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr17.png?raw=true" alt="challenge" class="inline"/>

now we have the data to generate our code pin:

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr21.png?raw=true" alt="challenge" class="inline"/>

and here we go, code pin generated successfully: 

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr22.png?raw=true" alt="challenge" class="inline"/>

we enter our code and pin:

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr9.png?raw=true" alt="challenge" class="inline"/>

we get our python interpreter shell prompt:

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr11.png?raw=true" alt="challenge" class="inline"/>

and we get our flag using LFI:

<img src="https://github.com/vvxhid/The_prophet_TetCTF2020-WEB-/blob/master/scr/scr24.png?raw=true" alt="challenge" class="inline"/>

we learned a lot from this challenge and we had a lot of fun, a nice experience!
