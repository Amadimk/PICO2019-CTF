# Crypto_300_waves_over_lambda 

------

### Titre : waves-over-lambda 
### Points : 300

------

### Description 

Nous avons fait beaucoup de substitutions pour chiffrer cela. Pouvez-vous le déchiffrer ?
Connectez-vous sur  `nc 2019shell1.picoctf.com 4164`

------

#### Hints

* Le Flag n'est pas dans le format habituel. 

------

### Résolution
Le serveur me renvoi le message ci-dessous. 
```bash
$ nc 2019shell1.picoctf.com 4164
-------------------------------------------------------------------------------
evinptbm djpj um svxp wgtn - wpjyxjies_um_e_vojp_gtkaht_euokajujko
-------------------------------------------------------------------------------
dtouin dth mvkj bukj tb ks humfvmtg cdji ui gvihvi, u dth oumubjh bdj apubumd kxmjxk, tih kthj mjtped tkvin bdj avvzm tih ktfm ui bdj guaptps pjntphuin bptimsgotiut; ub dth mbpxez kj bdtb mvkj wvpjzivcgjhnj vw bdj evxibps evxgh dtphgs wtug bv dtoj mvkj ukfvpbtiej ui hjtguin cubd t ivagjkti vw bdtb evxibps. u wuih bdtb bdj humbpueb dj itkjh um ui bdj jqbpjkj jtmb vw bdj evxibps, lxmb vi bdj avphjpm vw bdpjj mbtbjm, bptimsgotiut, kvghtout tih axzvouit, ui bdj kuhmb vw bdj etpftbduti kvxibtuim; vij vw bdj cughjmb tih gjtmb zivci fvpbuvim vw jxpvfj. u ctm ivb tagj bv gundb vi tis ktf vp cvpz nuouin bdj jqteb gvetgubs vw bdj etmbgj hptexgt, tm bdjpj tpj iv ktfm vw bdum evxibps tm sjb bv evkftpj cubd vxp vci vphitiej mxpojs ktfm; axb u wvxih bdtb aumbpubr, bdj fvmb bvci itkjh as evxib hptexgt, um t wtupgs cjgg-zivci fgtej. u mdtgg jibjp djpj mvkj vw ks ivbjm, tm bdjs kts pjwpjmd ks kjkvps cdji u btgz vojp ks bptojgm cubd kuit.
...
```
Dans la description nous savons que c'est plusieurs substitutions qui ont été effectuer sur le message initial qui contient donc notre flag, je me lance alors à la recherche des algorithmes de décodage de substitution et l'un d'eux est l'analyse fréquentielle. Par exemple, la lettre la plus courante en anglais et français est E, la lettre la plus courante dans le texte crypté est donc probablement la substitution de E. J'utilise donc le site [https://planetcalc.com/8047/](https://planetcalc.com/8047/) qui implémente un bon algorithme utilisaant le principe de la génétique pour trouver la clé de substution utilisée.

![alt text](https://raw.githubusercontent.com/Amadimk/PICO2019-CTF/master/waves.png)


Le Flag est : 
`picoCTF{bad_1d3a5_4986370}`
