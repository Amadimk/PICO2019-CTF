# Binary_Exploitation_100_OverFlow_0

------

### Titre : OverFlow_0

### Points : 100

------

### Hints

* Trouver un moyen de déclencher l'affichage du flag.
* Si vous essayez de faire le calcul à la main, essayez peut-être d'ajouter quelques caractères supplémentaires. Parfois, il y a des choses que vous n'attendez pas.
------

### Description 

Cela devrait être facile. Déborder le tampon du programme pour le flag drapeau ?

### Résolution
En regardant, le code ci-dessouss on remarque qu'il suffit de déborder du buffer de taille 128 qui declenchèra un signal SIGFAULT qui sera capturé et affichera le flag dans la sorti stderr.
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <signal.h>

#define FLAGSIZE_MAX 64

char flag[FLAGSIZE_MAX];

void sigsegv_handler(int sig) {
  fprintf(stderr, "%s\n", flag);
  fflush(stderr);
  exit(1);
}

void vuln(char *input){
  char buf[128];
  strcpy(buf, input);
}

int main(int argc, char **argv){
  
  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
    printf("Flag File is Missing. Problem is Misconfigured, please contact an Admin if you are running this on the shell server.\n");
    exit(0);
  }
  fgets(flag,FLAGSIZE_MAX,f);
  signal(SIGSEGV, sigsegv_handler);
  
  gid_t gid = getegid();
  setresgid(gid, gid, gid);
  
  if (argc > 1) {
    vuln(argv[1]);
    printf("You entered: %s", argv[1]);
  }
  else
    printf("Please enter an argument next time\n");
  return 0;
}
...
```
![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/overflow0.png)

Le Flag est : 
`picoCTF{3asY_P3a5y1fcf81f9}`
