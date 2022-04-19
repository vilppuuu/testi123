## Viikko 3 - Läksyt

### z) Commonmark contributors: Markdown Reference

* Artikkelissa listataan tärkeimmät Markdown komennot ja niiden käyttämät symbolit.
* Esim. Yksi risuaita (#) luo otsikon (heading 1) ja kaksi alaotsikon.
* Listan saa luotua joko tähdellä, tai järjestysluvulla (numerolista).
* Linkit toimivat hakasulkeilla ja sulkeilla, eli hakasulkeisiin linkin nimi ja sulkeisiin itse linkki.
* Kuvat toimivat samalla periaatteella paitsi ennen hakasulkuja tulee huutomerkki.
* Sisennys >> -merkkejä käyttäen, mitä useampi sen sisennetympi.
* Kolmella yläpilkulla (`) saadaan luotua *koodilohkoja*.

### a)  MarkDown.

> Q.E.D?

### b) Pull first. Tee useita muutoksia git-varastoosi.

>  Loin viikko3_laksyt markdown tiedoston git-varastoon, lisäksi toisen tyhjän tiedoston nimellätesti_b, jonka jälkeen käytin git add . -komentoa. Poistin myös git rm komennolla turhan tiedoston, jonka jälkeen tein commitin (git commit).
![Image](https://i.imgur.com/2VQwwQQ.png)

### b) Kaikki kirjataan. Näytä omalla git-varastollasi esimerkit komennoista ‘git log’, ‘git diff’ ja ‘git blame’. Selitä tulokset.
* git log näyttää tehdyt commitit, niiden kuvaukset ja timestampit, eli näkee nopeasti mitä on tehty ja milloin, jos kuvaukset ovat selkeitä.
![Image](https://i.imgur.com/sedOyPj.png)

* git diff näyttää taas muokatut tiedostot sisältöineen, ja niihin tehdyt muutokset.
![Image](https://i.imgur.com/XJBYnAG.png)
* git blame näyttää yhteen tiedostoon tehdyt muutokset (esim git blame viikko3_laksyt.md näyttäisi tämän tiedoston ja siihen tehdyt muutokset ja niiden tilan (commit vai ei) rivikohtaisesti. Tälle voi myös antaa erinäisiä arvoja esim. blame -L 1,10 näyttäisi vain rivit 1-10.
![Image](https://i.imgur.com/84snvxg.png)
 
### c) Huppis! Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’.

> Poistin tiedoston asdasdasdsa git rm -komennolla, jonka jälkeen varmistin vielä että se on poistettu. Tämän jälkeen ajoin git reset --hard -komennon, mikä siis palauttaa varaston viimeisimmän commitin tilaan.

![Image](https://i.imgur.com/ax1BbO1.png) 

### d) Formula. Tee uusi salt-tila (formula, moduli, infraa koodina).

> Päätin tehdä yksinkertaisen tilan, joka tarkastaa onko nano asennettu (pkg.installed) ja, että se käyttää(file.managed) määrittämääni asetustiedostoa. Aloitin luomalla srv/salt -kansioonnano -kansion, johon loin init.sls -tiedoston ja kopioin nanon asetustiedoston (etc/nanorc), johon olin jo tehnyt haluamani muutokset.

![Image](https://i.imgur.com/husBWZk.png)

> Testataksen poistin nanon (apt-get remove nano) ja nanon asetustiedoston (etc/nanorc). Ajoin tilan sudo salt "*" state.apply nano (aluksi unohtui tähti tuosta komennosta, jolloin antaa virheen: "no minions matched the target..."), mutta muuten tila toimi odotetulla tavalla.

![Image](https://i.imgur.com/TFiaHgO.png)
