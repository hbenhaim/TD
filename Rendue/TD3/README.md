
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