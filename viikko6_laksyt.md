# h6 Akkuna

## v) Lue ja tiivistä artikkeli muutamalla ranskalaisella viivalla. Tässä z-alakohdassa ei tarvitse siis tehdä testejä tietokoneella.

* Salt-masterin päivitys uusimpaan versioon Linuxilla, sillä uudemmat versiot Saltista toimivat paremmin Windowsilla ja ma
   sterin on oltava uudempi tai sama versio kuin minionin.
* Minonin asennus Windowsille (asennuksen yhteydessä määritellään master ja minionin id)
* Saltin Windows-ohjelmavarastojen käyttöönotto masterilla *(salt-run winrepo.update_git_repos)*, jonka jälkeen pakettaja pystyy asentamaan pkg.install -funktiolla.
* Saltin käyttö paikallisesti Windows-koneella Powershellin kautta käyttäen *salt-call --local:ia*.
* Pakettien hallinta Chocolateyn avulla (enemmän ohjelmia, kuin Saltin omassa varastossa).

 


##  a) Suolaikkuna. Asenna Salt Windowsiin. Jos ehdit jo asentaa, voit kirjoittaa muistinvaraisesti, mutta muista silloin merkitä, että tämä on muistista kirjoitettu. Näytä testillä (test.ping, file.managed tms), että Salt toimii.

> Kerkesin tämän tehdä jo tunnilla, joten seuraava on kirjotettu muistista. Eli saltin [sivuilta](https://saltproject.io/) latasin salt-minionista uusimman version (Salt-Minion-3004.1-AMD64-Setup.exe). Tämän jälkeen klikkasin asennusohjelman käyntiin, ja siinä ei muistaakseni ollu mitään ihmeellisyyksiä eli pari kertaa klikkailin next/install. Jossain vaiheessa asennusta se kysyttiin mitä konfiguraatiota käytetään, ja masterin ip:tä, sekä minionille nimeä. Konffiksi jäi default, ip:ksi laitoin Linux-koneen, jossa salt-master asennettuna, ja nimeksi win_desktop. Sitten komentokehotteesta kokeilin toimivuutta komennolla: *salt-call --local test.ping*.

![Img](https://i.imgur.com/dmYXUMI.png) 

## b)  Single. Näytä komentorivillä Saltilla (state.single) esimerkit funktioista file ja cmd.

> Eli kuten a -tehtävässäkin ajoin paikallisesti tuota *salt-call --local* käyttäen, mutta tällä kertaa myös lisäten siihen *state.single*, mikä tarkoittaa, että käytetään vain yhtä funktiota. File.managed -funktiolla loin uuden tiedoston tuonne omaan hakemistooni, jonne olin luonut uuden kansion salt, ja cmd.run -funktiolla käytin msinfo32-komentoa, joka näyttää järjestelmätiedot Windowsilla. Varoitus tuossa file.managed -funktiossa kertoo, että koska tiedostolle ei ole määritelty lähdettä eikä sisältöä, se ei päällekirjoita sitä turhaan.

![Img](https://i.imgur.com/akz97VD.png)

![Img](https://i.imgur.com/sdLiX3n.png)

## c) IaCcuna. Tee Windowsissa infraa koodina, ja aja se paikallisesti (salt-call --local state.apply foo)

> Tämän kanssa oli aluksi hieman ongelmia, sillä tuo salt-minionin asennus ei automaattisesti luo kansioita, joista tiloja ajetaan. Eka koitin luoda ne tuonne C:\Program Files\Salt Project, mutta kun tiloja koitti ajaa antoi vain *No matching sls found in env 'base'*. Pienen pähkäilyn jälkeen löytyi C:\ProgramData\Salt Project -kansio, jonne loin kansiot srv/salt, ja niiden sisään kansion, johon loin init.sls -tiedoston. Päätin kokeilla asentaa Chocolateyn Windowsille, joten siihen tarvittiin nuo Saltin Windows-pakettivarastot, mitkä sai paikallisesti komennolla: *salt-call --local winrepo.update_git_repos* & *salt-call --local pkg.refresh_db*. Kirjoitin ensiksi tilan, joka asensi Chocolateyn, ja kun se toimi lisäsin siihen osan, jossa Chocolatey asentaa vlc:n. Aluksi tuo chocolatey.installed, ei toiminut kun siinä oli Teron ohjeen mukaan parametrinä - pkgs:, mikä antoi virheen: *'pkgs' is an invalid argument for 'chocolatey.installed'*. Katsoin Saltin documentaatiotatuosta, niin siellä oli ohjeistettu tuolla - name:lla ja se toimikin.

```
chocolatey:
pkg.installed
vlcinstall:
chocolatey.installed:
- name: vlc
```

![Img](https://i.imgur.com/8THG4TD.png)

![Img](https://i.imgur.com/A2ZInlr.png)

## d) Goal. Tee projektisi palautussivu. Voit tehdä sen GitHubiin, kotisivullesi tai mihin vain haluat. Mistä teet miniprojektin? Kuvaile miniprojektin tarkoitus lauseella tai parilla. Asenna käsin (jokin alustava osa) projektistasi ja ota ruutukaappaus siitä, miten lopputulosta käytetään.

> Loin githubiin uuden varaston ![ln](https://github.com/vilppuuu/salt-miniproject/) ja valitsin lisenssisksi GPL 2.0. Kloonasin tämän varaston Linux-koneelleni (git clone ssh-linkki githubista), ja tein sinne alustavan init.sls -tiedoston.

> Ideana tehdä moduuli, joka asettaa/korjaa Windowsin käyttäjän oletusasetukset siedettäviksi, sekä poistaa Windowsin mukana tulevat turhat ohjelmat, ja asentaa joitakin ohjelmia. Itselle ainakin ollut aina ongelma, kun asentaa "puhtaan" Windowsin, niin sen mukana tulee ohjelmia, joita en todellakaan haluaisi, sekä käytännössä jokainen Windowsin oletusasetus täytyy käydä muuttamassa. Noita roskaohjelmia olen aiemmin poistellut käsin käynnistävalikosta, tai sitten Powershellillä (*get-appxpackage programname | remove-appxpackage*). Asetukset olen käynyt aina klikkailemassa läpi käsin (voi tehdä myös regeditillä). Mitä nyt alustavasti tutkin, niin ainakin Windowsin asetusten muokkaaminen rekisterin kautta pitäisi onnistua Saltilla. Noiden Windowsin oletusohjelmien poistaminen Saltilla käyttämättä cmd.run ja powershell-skriptejä saattaa osoittautua turhan vaikeaksi, sillä niitä ei ole Saltin omassa tai Chocolateyn paketeissa, ja en tiedä saako niitä edes luotua käsin, kun ne asentuvat Windows Storen kautta, eivätkä siis ole "normaaleja" ohjelmia.

![Img](https://i.imgur.com/6vCtfAB.png)
