# HTB CTF
### Fri-sat-sun 20-23 november 2020

## CTF
A while ago, I participated in the Hack the Box CTF, with a group of fellow Fontys students. It went from Friday 20 november 2020 until Sunday the 23rd. With this group, we placed 77th out of 200 competing universities. It was a very fun weekend, where we worked together to find the answers to many of these problems. I myself sunk most of my energy in one challenge in particular, called Arcade. It was a text game, where it posed a question and you had to type your answer. 

## Gameplay
When the game starts, you’re greeted with “Welcome to Super Arcade!” and prompted to select a difficulty. 2, hard, is the only option. Then, you are shown a menu. You see what your stats are and how many credits you have left. You start with four credits, and every time you create a profile you lose a credit. Choosing option 2, to play, will not lose you credits. Further, you are shown choices: to create your profile, to play the game, to claim a prize, and to exit. With option 1, you are prompted to create a profile. There, you put in your name, values for health, attack and speed, and a ‘password’. Your name has to start with “Super-“ and you’re suggested to add something to it. It will tell you the name can’t be blank, but it can be just “Super-“. Then, you can choose which stat you want to heighten. Whichever you pick, you cannot enter a value higher than 120. If you do,  the game will throw an error message and send you back to the menu.  Lastly, you fill in a catchphrase. This will act like a sort of ‘passphrase’ later on.

[gameplay](img/htbctf/gameplay.png)

## Objdump
With Objdump, I tried to find the main codeblocks that would be of interest. 
[Objdump](img/htbctf/objdump.png)
 
## Ghidra
With Ghidra, I managed to get the source code as well. This is the part that caught my eye:
[ghidra](img/htbctf/ghidraplaygame)
 
In it, it said that your health, attack and speed points had to be a certain value before the game decided you’d win. So the goal is to get the values to these levels. I tried this out at profile creation. You can fill in numbers higher than 120 and it will save the number. However, since filling in numbers higher than allowed makes you skip the last part, it won’t count as a win.
```if (((long)local_28 < 0x79) && (0 < (long)local_28)) { fwrite("\nInsert a catch-phrase for your character!\n> ",1,0x2d,stdout); __src = (char *)malloc(local_28); read(0,__src,local_28); strncat(passw,__src,0x100); }```
You can see that in this excerpt; if this is skipped, the “password” won’t be complete. 
I decided to use GDB to skip through the assembly. 
[assembly](img/htbctf/disass) 

In that, I found a few blocks of note: menu, create_profile, play_game, and prize. You can see here, in the disassembled menu, that they get referred to in certain places. I used the play_game to figure out exactly where the values were used; however, this proved unnecessary as the real game depended on something else as well: the password. This is where I had gotten stuck on the day itself. In the end, I found out from a different writeup that the crux lies on using buffer overflow. By loading the “passphrase” with a specific number of ‘-‘s you will be able to trick the game into thinking all the values are correct and allows you to win the game and gain the flag. 

```target.recv() # Insert a catch-phrase for your character!
target.send("-"*10)```

The full code can be found [here](https://github.com/Zarkrosh/ctf-writeups/blob/master/2020-HTBxUni-Quals/solvers/misc_arcade/solver.py) 

## Conclusion
All in all, it was a very fun experience. I would do another CTF in the future if I can. 