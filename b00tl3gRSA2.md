# Crypto_400_b00tl3gRSA2

------

### Titre : b00tl3gRSA2

### Points : 400

------

### Description 

En RSA, d est beaucoup plus gros que e, pourquoi ne pas utiliser d pour chiffrer au lieu de e?
Connectez-vous sur  `nc 2019shell1.picoctf.com 10814`


------

#### Hints

* C'est quoi la valeur de "e" en général ?. 

------


### Résolution

une fois connecter au serveur il nous renvoi les paramétres suivants :

```bash
$ nc 2019shell1.picoctf.com 10814
c: 3246567211807903956035755386967723378469000089645359852430311338899740839190191186912796367655591221991073923539783802534343637991859596366133917926183487070028511851491672546150628697450709648276484740441059116769268880830587810651048199357915546440564242235096147024745525549383938798867512940345819471689
n: 92862994629391715025973326227583472781928051965578118595096998621079516709489523781529594005972923927674610395292210963782547003449976221876961532867478284466283591394095929545356891486644591390877842583027695288887951335923580147704761840841012240495813817062197826156384498734601243045712509995933135061613
e: 55534726162469284328134833374089125970705191581654686697554612920273578259533132513991933989595141790093397702407611338686991542277109389566561688171186896367716544979539952509680212727662952068636577177373930069448696402121437219985978915997070443631144215446467780163599410246770789910349000677632900578365
...
```
En se basant sur l'enoncé du challenge, l'hint et connaissant un peu la cryptographie RSA je comprends vite que dans cette configuration la clé privé "d" pour dechiffrer le chiffré "c" est la valeur de "e" par défaut qui est généralement 65537.

connaissant maintenant tous les paramétres pour dechiffrer le message chiffré, j'ai tous simplement converti ces diffèrents paramètres en hexadecimal puis sur le site : [https://nmichaels.org/rsa.py](https://nmichaels.org/rsa.py) j'ai reussi à dechiffré le message. (Une autre méthode était d'utiliser [RsaCtftool](https://github.com/Ganapati/RsaCtfTool) qui est trés bien pour manipuler RSA ).

![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/rsadecryp.png)

Puis ensuite avec python j'ai converti la chaine hexadecimal dechiffré en utf-8 et bingo flag.
![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/rsa2.png)

`picoCTF{bad_1d3a5_4986370}`
