
# Buffer Overflow 

Le principe du buffer overflow est de faire exécuter un code malveillant à un programme. Pour mieux comprendre comment faire, prenons comme exemple le programme suivant :  
 
```
#include <stdio.h>
#include <string.h>

void foo(char* string);

int main(int argc, char** argv) {
  if (argc > 1)
    foo(argv[1]);

  return 0;
}

void foo(char* string) {
  char buffer[256];
  strcpy(buffer, string);
}
```
Si nous désassemblons la fonction foo, nous pouvons constater grâce au prologue de la fonction :

```
push   %ebp
mov    %esp,%ebp
sub    $0x108,%esp ; 0x108 = 264
```
le compilateur alloue 264 bytes pour notre buffer de 256 bytes. On peut donc extrapoler la partie du cadre de la fonction qui va nous intéresser.
![screen 1](https://github.com/hbenhaim/TD/blob/master/Rendue/TD3/screen/11.png)

Pour appliquer le principe défini en début de section, nous allons dépasser les 264 bytes alloués à notre buffer jusqu'à écraser la sauvegarde du registre %eip de la fonction appelante de telle manière que, lorsque l'instruction ret sera exécutée, le programme sera redirigé vers notre code malveillant. 

Voyons ce qui se passe si nous dépassons notre buffer et que nous écrasons la sauvegarde du registre %ebp de la fonction appelante en injectant 268 caractères dans notre buffer.
![scren 2](https://github.com/hbenhaim/TD/blob/master/Rendue/TD3/screen/12.png)

Après analyse du fichier core à l'aide de l'utilitaire gdb, nous pouvons observer qu'une erreur de segmentation s'est produite à l'instruction d'adresse 0x00007fe6e4e6d428.

Maintenant, nous allons aller encore plus loin en écrasant la sauvegarde du registre %eip de la fonction appelante de la manière suivante :
![screen 4](https://github.com/hbenhaim/TD/blob/master/Rendue/TD3/screen/15.png)



