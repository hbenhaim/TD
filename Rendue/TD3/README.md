
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
