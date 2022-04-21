## Viikko 4: Läksyt

### a) Captain obvious. Linuxissa on paketinhallinta, joten ohjelmien asentaminen on yksinkertaista. Tee tila, joka asentaa 10 suosikkiohjelmaasi paketinhallinnasta.

> Aloitin luomalla srv/salt -kansioon uuden kansion nimeltä paketit ja sinne init.sls -tiedoston. Tämän jälkeen avasin saltin manuaalitiedoston, jonka olin jo aiemmin tallentanut kotihakemistoon (sudo salt-call --local sys.state_doc >> sys.statedoc.txt). Sieltä etsin pkg.installed hakusanalla kohtaa, jossa asennettaisiin useampi paketti kerralla, ja pienen hakemisen jälkeen sellainen löytyikin. Muuten esimerkkitila näytti samanlaiselta kuin olisin itsekin tehnyt, paitsi luultavasti ensimmäinen rivi olisi luultavasti unohtunut aluksi, lisäksi tuo hold: true kohta olisi jäänyt laittamatta (manuaalin mukaan tarkoittaa, että paketti pakotetaan pysymään nykyisessä asennetussa versiossa, järki?).
>>Itseasiassa on erillinen funktio pkg.latest, mikä toimii samoin kun pkg.installed, mutta päivittää aina uusimman version. Eli tuo pkg.installed ja hold käy järkeen siinä mielessä, jos halutaan pysyä tietyssä vanhemmassa versiossa esim. yhteensopivuuden tai riippuvuuksien takia.

![Image](https://i.imgur.com/lkQXnjY.png)

> Muuten tilan kirjoittaminen oli yksinkertaista esimerkkiä noudattaen paitsi käytin tuota pkg.latest -funktiota ja Linuxia vähän käyttäneelle kymmenen eri ohjelman keksiminen oli oma työnsä.
``` 
 1 mypkgs:
 2   pkg.latest:
 3     - pkgs:
 4       - micro
 5       - nmap
 6       - tree
 7       - git
 8       - apache2
 9       - vlc
10       - nginx
11       - ufw
12       - tldr
13       - neofetch
```

### b) CSI Pasila. Tiedostoista saa aikajanan 'cd /etc/; sudo find -printf '%T+ %p\n'|sort|tail'.

* Anna esimerkki aikajanasta: 
>> 
* Selitä jokainen kohta komennosta, jolla aikajana tehdään. Vinkki: '%T+' löytyy 'man find' kohdasta printf.
>> 
* Aja jokin komento, joka muuttaa järjestelmän yhteisiä asetustiedostoja. Ota uusi aikajana ja etsi muutos sieltä. Onko samalla hetkellä muutettu yhtä vai useampaa tiedostoa?
>>

### c)  Tiedän mitä teit viime kesän^H^H^H komennolla. Säädä jotain ohjelmaa ja etsi sen muuttamat tiedostot aikajanasta. Tee sitten tästä oma Saltin tila.

### d)  Asenna jokin toinen ohjelma asetuksineen.
