
# Buffer Overflow 

Le principe du buffer overflow est de faire exécuter un code malveillant à un programme. Pour mieux comprendre comment faire, prenons comme exemple le programme suivant :  
 
```
#include <stdio.h>
#include <string.h>

int main(void)
{
    char buff[15];
    int pass = 0;

    printf("\n Enter the password : \n");
    gets(buff);

    if(strcmp(buff, "thegeekstuff"))
    {
        printf ("\n Wrong Password \n");
    }
    else
    {
        printf ("\n Correct Password \n");
        pass = 1;
    }

    if(pass)
    {
       /* Now Give root or admin rights to user*/
        printf ("\n Root privileges given to the user \n");
    }

    return 0;
}
```
Le programme ci-dessus simule un scénario dans lequel un programme attend un mot de passe de l'utilisateur et si le mot de passe est correct, il accorde des privilèges root à l'utilisateur.

Lançons le programme avec le mot de passe correct, c'est-à-dire «insa_2su»:
![screen 1](https://github.com/hbenhaim/TD/blob/master/Rendue/TD3/screen/21.png)

Il y a une possibilité de débordement de tampon dans ce programme. La fonction gets() ne vérifie pas les limites du tableau et peut même écrire une chaîne de longueur supérieure à la taille du tampon dans lequel la chaîne est écrite. Maintenant, on peut même imaginer ce que peut faire un attaquant avec ce genre d'échappatoire?

Voici un exemple :   

![screen 2](https://github.com/hbenhaim/TD/blob/master/Rendue/TD3/screen/22.png)

Il y a une logique derrière la sortie ci-dessus. Ce que l’attaquant a fait, c’est qu’il a fourni une entrée de longueur supérieure à ce que le tampon peut contenir et à une longueur d’entrée particulière, le débordement de tampon a donc eu lieu qu’il a écrasé la mémoire de l’entier «passe». Ainsi, malgré un mot de passe incorrect, la valeur de «pass» est devenue non nulle et, par conséquent, les privilèges root ont été accordés à un attaquant.

