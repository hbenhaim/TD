# Crack emily

### First we Create our basic program :
![Screen 1](https://github.com/hbenhaim/TD/blob/master/Rendue/TD1/Screen/1.png)

### Then We test :
![Screen 2](https://github.com/hbenhaim/TD/blob/master/Rendue/TD1/Screen/2.png)
li
### We run the command :
```
objdump -S -l -C -F -t -w program | less
```
### Wich give us the result below : 
![Screen 3](https://github.com/hbenhaim/TD/blob/master/Rendue/TD1/Screen/3.png)

The idea is to make the function is_valid return the same result whatever the password taped in to the program.
To make that happen we need to make the ```mov``` instrtuction in line 4006de like the the one on the line 4006d7

### In order to do that, we'll use the command next : 
```
printf '\x01' | dd of=program bs=1 seek=1762 count=1 conv=notrunc
```
Where 40006de is where the instruction we care about,40006de to decimal is 1758 bytes into the file, we only need to change the fourth byte of the instruction, so we can add four to that count to get 1762 bytes. 

### Result : 
![Screen 5](https://github.com/hbenhaim/TD/blob/master/Rendue/TD1/Screen/5.png)


### Last test :
![Screen ](https://github.com/hbenhaim/TD/blob/master/Rendue/TD1/Screen/6.png)
