# Binary_Exploitation_200_slippery_shellcode

------

### Titre : slippery-shellcode

### Points : 200

------

### Description 

Ce programme est un peu plus compliqué. Pouvez-vous générer un shell et l'utiliser pour lire le fichier flag.txt? Vous pouvez trouver le programme dans /problems/slippery-shellcode_6_7cf1605ec6dfefad68200ceb12dd67a1 sur le serveur shell

------


### Résolution

Comme pour le handy shellcode je commence à analyser le code C ci-dessous :

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>

#define BUFSIZE 512
#define FLAGSIZE 128

void vuln(char *buf){
  gets(buf);
  puts(buf);
}

int main(int argc, char **argv){

  setvbuf(stdout, NULL, _IONBF, 0);
  
  // Set the gid to the effective gid
  // this prevents /bin/sh from dropping the privileges
  gid_t gid = getegid();
  setresgid(gid, gid, gid);

  char buf[BUFSIZE];

  puts("Enter your shellcode:");
  vuln(buf);

  puts("Thanks! Executing from a random location now...");

  int offset = (rand() % 256) + 1;
  
  ((void (*)())(buf+offset))();


  puts("Finishing Executing Shellcode. Exiting now...");
  
  return 0;
}
...
```
Je comprends vite que ce code est presque le même qu'avec le handy-shellcode sauf qu'ici notre shellcode est executé après  un offset qui est modulo 256. Pour un début je tente donc de lui envoyer mon shellcode directement :  

![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/slippery1.png)

Mon shellcode est effectivement executer mais pas à la bonne adresse et j'ai un segmentation fault.
Je tente ensuite d'executer le programme en remplissant son buffer de 256  caractères suivi de mon shellcode.

![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/slippery2.png)

Et Hop j'ai effectivément un shell et le flag.


`picoCTF{sl1pp3ry_sh311c0d3_5a0fefb6}`
