# Binary_Exploitation_50_Handy_shellcode

------

### Titre : Handy_shellcode

### Points : 50

------

### Description 

This program executes any shellcode that you give it. Can you spawn a shell and use that to read the flag.txt? You can find the program in /problems/handy-shellcode_6_f0b84e12a8162d64291fd16755d233eb on the shell server.

Ce programme exécute n'importe quel shellcode que vous lui donnez. Pouvez-vous générer un shell et l'utiliser pour lire le fichier flag.txt? Vous pouvez trouver le programme dans /problems/handy-shellcode_4_037bd47611d842b565cfa1f378bfd8d9 sur le serveur shell. (Merci à google translate :))

------

#### Hints

* You might be able to find some good shellcode online. (Vous pourrez peut-être trouver un bon shellcode en ligne ) Merci encore google. 

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

Ce programme utilise un buffer pour lire l'input de l'utilisateur on peut donc s'en servir pour envoyé un shellcode (Il s’agit donc essentiellement d’un morceau de code compilé arbitraire qui peut être injecté dans un programme afin d’engendrer un shell dans le système d’exploitation exécuté par le programme) aprés quelques recherches sur le shellcode j'ai trouvé ce shellcode :
```bash
$ 
...

Et cela confirme ma supposition le programme recupere effectivement un input et à  les mêmes droits qu'etant sudo ou root le bit s nous l'indique et donc reste plus qu'a trouvé un  bon shellcode (google nous répond vite ) qu'on va donner à manger à notre binaire et hop on peut lire notre fichier flag.txt 

Vous pouvez utiliser des blocs de code comme celui-ci:

```bash
$ nc ctf.teamcryptis.fr 50004
...
```

Copier/coller votre commande avec son output, si la sortie de la commande est trop longue, gardez que la partie essentielle.

Et terminez par donner le flag comme ça:

`picoCTF{flag_example}`
