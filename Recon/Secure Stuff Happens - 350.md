<b> Problem </b> <br>
Go ahead, try and brute force my SSH server! But be warned... I'm watching you....

Did you really think I'd use a combination that only an idiot would use on his luggage?

162.243.0.171:22

<b> Solution </b> <br>

This was one of the least-solved problems, so I figured an official writeup is necessary. There were several points of misdirection that I believe ended up to be a little <i>too</i> tricky.

First, the problem asks the user to brute force my SSH server running on port 22. The context of the clue was designed to lead people into googling "password an idiot would use on his luggage", aka 12345 (Spaceballs reference ![here](https://www.youtube.com/watch?v=a6iW-8xPw3k)!)
However, this didn't immediately work but the correct root password was actually "123456". We were hoping that it would be bruteforced with either Hydra or Ncrack to discover this, or by sheer random guessing. 

After a successful login, the user was presented with a hostname of <code>nas3</code> and found a nearly empty shell. Grep was disabled as well as many other useful commands. This eventually caused much frustration and many red herrings as people sought to somehow find the flag on the server.

As it turns out, this was actually a Kippo honeypot. There were several giveaways. First, the hostname of <code>nas3</code> was the default for kippo, the default password of '123456' was the default for kippo, and could actually be identified as such with a detailed Nmap or Metasploit scan.

The hint, "I happen to enjoy analytics, stats, facts, figures..." was supposed to lead the user in a particular direction. Namely, a honeypot is useless without logs, and these logs are not stored directly IN the honeypot. With a little bit of googling, you can find that Kippo is often bundled with the "Kippo-graph" utility for visualizations. A web scanner like Nikto would also have picked up on the following directory: http://162.243.0.171/kippo-graph/

While this had a bunch of graphs and statistics, the relevant portion was the user playback tab. This was the reference to "I'm watching you.." since all user input was logged and could be played back in an ascii-cinema-like page. If you go to the earliest logon, you'll find that the user types the flag as a command. http://162.243.0.171/kippo-graph/include/play.php?f=3b96654ac5d611e5a7b004019feafc01

<b>flag{uh_huh_h0n3y}</b>
