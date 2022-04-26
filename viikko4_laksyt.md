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
![Image](https://i.imgur.com/)
* Selitä jokainen kohta komennosta, jolla aikajana tehdään. Vinkki: '%T+' löytyy 'man find' kohdasta printf.
>> Find etsii tiedostoja hakemistoista, -printf tulostaa etsityt tiedostot, %T+ näyttää tiedoston viimeisimmän muokkausajan, %p näyttää tiedoston nimen, \n rivittää tekstin, sort järjestää tiedostot muokkausajan mukaan, ja tail näyttää vain kymmenen viimeisintä tiedostoa.
* Aja jokin komento, joka muuttaa järjestelmän yhteisiä asetustiedostoja. Ota uusi aikajana ja etsi muutos sieltä. Onko samalla hetkellä muutettu yhtä vai useampaa tiedostoa?
>> Kävin vaihtamassa koneen nimeä /etc/hostname ja /etc/hosts -tiedostoissa. Aikajana näyttää vain yhden muutoksen molempiin tiedostoihin, toisaalta asetukset tulevat käyttöön vasta bootin jälkeen (aikajana näytti samalta bootin jälkeenkin).
![Image](https://i.imgur.com/l)

### c)  Tiedän mitä teit viime kesän^H^H^H komennolla. Säädä jotain ohjelmaa ja etsi sen muuttamat tiedostot aikajanasta. Tee sitten tästä oma Saltin tila.
> Ohjelma, jota päätin säätää on UFW (palomuuri). Aloitin tarkastamalla, että se on päällä ja "aktivoitu" kirjoittamalla komennon "sudo ufw enable", johon tuli vastaus "Firewall is active and enabled on system startup", eli toimii. Tämän jälkeen lisäsin palomuurille random säännön, eli blokkasin satunnaisen ip-osoitteen (sudo ufw deny from 123.123.123.123). Sitten kävin ottamassa aikajanan /etc -kansiosta, jossa näkyi kaksi muokattua tiedostoa ufw:n kansiossa.
![Image](https://i.imgur.com/)
>Loin /srv/salt hakemistoon ufw -kansion, jonne kopioin nämä kaksi asetustiedostoa.
### d)  Asenna jokin toinen ohjelma asetuksineen.
