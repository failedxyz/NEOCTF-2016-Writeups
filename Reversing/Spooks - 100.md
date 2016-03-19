
<b> Problem: </b> <br>
I've got this really scary program... kinda gives me the creeps... https://dl.dropboxusercontent.com/u/12992225/spook.exe

Credits to Sean D. for his contribution to Programming Club.

P.S. you are missing out if you don't actually run this one.

<b> Writeup: </b> <br>

This was supposed to be a very straightforward reversing exercise just to give people a chance to find out about the <code>strings</code> command. The binary is largely inconsequential, but if you type 'skeleton' when it asks you 'spook'?, you get a beautiful ascii representation of a dancing skeleton.

To find the flag, simply run the strings command by saying <code>strings spook.exe</code>, You can also find the easter egg password this way.

flag{sp00000000000kie_crisp}
