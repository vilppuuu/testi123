## Viikko 5: Uusi komento

### a) Hei komento! Tee järjestelmään uusi "hei maailma" -komento ja asenna se orjille Saltilla. Liitä raporttiisi 'ls -l /usr/local/bin/' tulosteesta ainakin se rivi, jolla näkyy uuden komentotiedostosi oikeudet. Vinkkejä: tee shell script, joka tulostaa "hei maailma". Kokeile ensin käsin, sitten automatisoi. Luonteva paikka paketinhalllinnan ulkopuolelta asennetuille ohjelmille on /usr/local/bin/. Katso myös 'salt-call --local sys.state_doc file.managed'. 
> Aloitin suunnistamalla tuonne /usr/local/bin -kansioon, ja luomalla sinne hello.sh -tiedoston, johon lisäsin seuraavat rivit:
```
 1 #!/bin/bash
 2 echo "Hello world"
```
> Annoin skriptille ajo-oikeudet komennolla: sudo chmod 744  hello.sh ja testasin toimivuuden ajamalla skriptin komennolla bash hello.sh.

![Image](https://i.imgur.com/IPjh5PS.png)

> Siirryin /srv/salt -kansioon, jonne loin uuden kansion hello ja sinne uuden init.sls tiedoston (kopioin myös käsinkirjoitetun skriptin tähän kansioon),johon aloin kirjottamaan salt-tilaa skriptille. Käytin jo tutuksi tullutta file.managed -funktiota, jossa ensiksi määrittelin mistä kansiosta tämä skripti pitäisi löytyä sen jälkeen määrittelin mitä tiedostoa käytetään lähteenä, ja tuolla mode: -parametrillä annoin oikeudet 744, eli käyttäjä saa lukea, kirjoittaa, ja ajaa, ryhmä ja muut vain lukea. Ennen testiä poistin /usr/local/bin -kansiosta hello.sh -tiedoston ja sen jälkeen ajoin tilan, joka toimikin toivotusti, eli tiedosto ilmestyi sinne ja sen pystyi ajamaan, ja kun katsoi ls -l tiedoston tietoja olivat sen oikeudetkin kunnossa.

```
 1 /usr/local/bin/hello.sh:
 2   file.managed:
 3     - source: /srv/salt/hello/hello.sh
 4     - mode: 744
```

![Image](https://i.imgur.com/AnhSzvx.png)

![Image](https://i.imgur.com/KQ51eam.png)

### b) whatsup.sh. Tee järjestelmään uusi komento, joka kertoo ajankohtaisia tietoja; asenna se orjille. Vinkkejä: Voit näyttää valintasi mukaan esimerkiksi päivämäärää, säätä, tietoja koneesta, verkon tilanteesta.

> Suuntasin taas tuonne /usr/local/bin -kansioon ja loin sinne whatup.sh -tiedoston. Ainakin omilla skriptaustaidoillani helpoin keino saada tehdä skripti,joka näyttää tietoja oli käyttää tuota echoa, ja laittaa se tulostamaan komentojen tuloksia, jotka antavat tietoa järjestelmästä. Kun komento laitetaan gravismerkkien` sisällä tuohon echo stringin sisään, tulostuu sen output siihen. Laitoin skriptiin muutamia komentoja, jotka kertovat mm. käyttöjärjestelmän tietoja (uname -a), päivämäärän ja ajan (date), ip-osoitteen (hostname -I), prosessorin tietoja (lscpu), ja vapaan ram-muistin määärän (free). Kuten edellisessäkin tehtävässä skriptille pitää antaa oikeudet (sudo chmod 744), ja sitten testata.

```
#!/bin/bash
echo "Whatsup?"
echo "System information: `uname -a` "
echo "Time & Date: `date`"
echo "IP: `hostname -I`"
echo "------------------------------------------------------------------------"

echo "Processor: `lscpu|head -16|tail -6` "
echo "------------------------------------------------------------------------"

echo "Free ram: `free`"
echo "------------------------------------------------------------------------"
```

![Image](https://i.imgur.com/PHOMDYz.png)

> Salt-tilan luominen tästä meni samalla tavalla kuin edellisessä tehtävässä, eli /srv/saltiin uusi kansio, jonne kopioin skriptin ja uusi init.sls -tiedosto. Eka testi tuotti *mapping values are not allowed in this context* errorin, mikä johtui puuttuvasta kaksoispisteestä ensimmäisellä rivillä. Toisella yrittämällä toimi oletetulla tavalla.

```
/usr/local/bin/whatup.sh:
  file.managed:
    - source: /srv/salt/whatsup/whatup.sh
    - mode: 744
```
![Image](https://i.imgur.com/WV9gjvh.png)

### c) hello.py. Tee järjestelmään uusi komento Pythonilla ja asenna se orjille. Vinkkejä: Hei maailma riittää, mutta propellihatut saavat toki koodaillakin. Shebang on "#!/usr/bin/python3". Helpoin Python-komento on: print("Hei Tero!")

> Eli taas tuonne /usr/local/bin -kansioon, ja sinne uusi tiedosto python_testi.py, johon ensimmäiselle riville tuo "#!/usr/bin/python3", mikä siis kertoo järjestelmälle kyseessä olevan python-skriptin. Skriptin sisällöksi kopioin Campusonlinen python-kurssille tekemäni alkukantaisen laskimen. Sitten taas chmodilla oikeudet skriptille ja testi. Toimi niinkuin pitääkin, kun löytyi oikea komento, eli pitää olla tuo python3, ei tunnista pelkkää pythonia.

![Image](https://i.imgur.com/mF3VaTn.png)

###  d) Laiskaa skriptailua. Tee kansio, josta jokainen skripti kopioituu orjille. Vinkki: 'salt-call --local sys.state_doc file.recurse'. Kun tämä on valmis, on todella helppoa laittaa orjille mikä tahansa yhden tiedoston shell script, Python-ohjelma, Perl-ohjelma, Go-binääri tai muu yhden binäärin ohjelma.

> Aloitin luomalla /srv/salt:iin uuden kansion lazy ja sen sisään toisen kansion skriptit, jonne siirsin (sudo mv) aiemmissa tehtävissä tehdyt skriptit tuolta usr/local/bin -kansiosta. Kävin kurkkaamassa saltin dokumentaatiosta, mitä siellä sanotaan tuosta vinkatusta file.recurse -funktiosta ja kokeilin kirjoittaa sitä käyttäen tilan, joka kopioisi skriptit kansion minioneille. Ensimmäinen yritys tarjoili vain alla olevan errorin, mistä kyllä selviää että source:n polku oli väärin (absoluuttinen polku ei kelpaa), vaan  pitää käyttää tuota salt://. Tämän korjattuani tilan sai ajettua ja se toimi halutusti, eli skriptit kopioituivat sinne minne pitikin ja niitä pystyi ajamaan, kun niille oli annettu oikeudet tuolla file_modella.

![Image](https://i.imgur.com/oZLUhhq.png)

![Image](https://i.imgur.com/uYnHMFQ.png)

```
/usr/local/bin:
  file.recurse:
    - source: salt://lazy/skriptit
    - file_mode: 744
```

### e) Intel. Etsi kolme loppuprojektia joltain vanhalta kurssitoteutukselta. Kuvaile projektit tiiviisti ja linkitä alkuperäiseeen raporttin.

> [Andersin kouluprojektit](https://koodiprojektit.wordpress.com/h7-oma-projekti/)

* Projektin ideana oli tehdä salt-tila, jossa asennetaan ja konfiguroidaan LAMP(Linux, Apache, MySQL/MariaDB, PHP) stackin sisältyvät ohjelmat.
* Sekä kirjoittaa erinäisiä shell-skriptejä, jotka keräävät ja tallentavat tietoa järjestelmästä(resurssit, päivitykset ym.) yhteen lokitiedostoon ja näiden jako minioneille salt-tilalla.
* Myös rsync:in (tiedostojen kopiointityökalu Linuxille) käyttöä käyttäjän kotihakemiston varmuuskopiointiin.
* Pari erilaista ohjelmamoduulia saltille, joissa työpöytä ja komentorivi ohjelmia, ja kaikkien salt-tilojen yhdistäminen top.sls -tiedostoon.

> [Palvelinten hallinta h6 8.5.2018 – Oma miniprojekti Saltilla](https://katrilaulajainen.wordpress.com/2018/05/10/palvelinten-hallinta-h6-8-5-2018-oma-miniprojekti-saltilla/)

* Tässä projektissa tarkoituksena oli asentaa muutama ohjelma, sekä muokata niiden oletusasetukset tiettyyn muotoon.
* Tehty salt-tila asensi ja konfiguroi Firefoxin, SSH:n, UFW:n sekä GIMP:in.
* Näihin tehdyt muutokset olivat Firefoxiin oletussivun vaihto, SSH:n oletusportin vaihto, UFW:n käyttöönotto sekä joidenkin porttien avaaminen.

> [windowsgames](https://github.com/jaketin/windowsgames)

* Käytännössä pelaamiseen tarvittavien ohjelmien asennusta saltin avulla Windowsille.
* Käyttää chocolatey -nimistä ohjelmaa, joka on kai ohjelman muodossa vähän kuin Linuxin paketin hallinta Windowsille.
* Salt-tila, jolla paketit asennetaan Windowsille chocolateylla näyttää muuten kohtuu samalta kuin vastaava Linuxille, paitsi että funktiona on chocolatey.installed.
* Myöskään ilmeisesti kaikkien ohjelmien asennus ei chocolateylla onnistu, vaan esim. Discord piti asentaa puoliksi käsin.

### e) Lukua, ei luottamusta. Kokeile yhtä kohdassa d-Intel löytämääsi modulia koneella. Tämä on infraa koodina, joten luottamusta ei tarvita. Voit lukea koodista, mitä olet ajamassa.

> Elikkä päätin ladata KatriL:n tekemän tulikettu -moduulin, jonka siis sai menemällä hänen postauksessa maintsemaansa github -varastoon, ja sieltä vihreätä Code -painiketta painamalla saa ladattua zip:inä kloonin omalle koneelle. Purin zip:in downloads -kansiossa, jonka jälkeen siirsin salt-tilan sisältävänfirefox -kansion /srv/salt. Kokeilin ajaa, mutta antoi "No matching sls found for firefox" erroria. Koska ne tiedostot ovat kyllä siellä kansiossa oletin, että kyseessä saattaisi olla oikeuksien kanssa joku ongelma, joten nopeasti ls -l ja erilaisethan ne ovat verrattuna muihin(ei luku -tai ajo-oikeutta, myös eri omistaja?), annoin chmodilla kaikki oikeudet (777), ja kokeilin toimiiko ja tilan sai ajettua, mutta antaa erroria tuosta firefoxin syspref.js -tiedostosta. Nopeasti katsoin init.sls, ja siellä tuon firefoxin file.managed -funktion kohdalla olisi pitänyt olla se - makedirs: True, niin olisi oletettavasti toiminut.

![Image](https://i.imgur.com/JD2CSP6.png)

![Image](https://i.imgur.com/0GrG1KF.png)

![Image](https://i.imgur.com/ImL5h0P.png)

### f) Palauta linkki raporttiisi Laksuun.

### g) Anna palaute kahdelle opiskelukaverille Laksussa. (Täsmennys: siis tästä tehtävästä h5)
