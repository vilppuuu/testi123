## Viikko 4: Läksyt

### a) Captain obvious. Linuxissa on paketinhallinta, joten ohjelmien asentaminen on yksinkertaista. Tee tila, joka asentaa 10 suosikkiohjelmaasi paketinhallinnasta.

> Aloitin luomalla srv/salt -kansioon uuden kansion nimeltä paketit ja sinne init.sls -tiedoston. Tämän jälkeen avasin saltin manuaalitiedoston, jonka olin jo aiemmin tallentanut kotihakemistoon (sudo salt-call --local sys.state_doc >> sys.statedoc.txt). Sieltä etsin pkg.installed hakusanalla kohtaa, jossa asennettaisiin useampi paketti kerralla, ja pienen hakemisen jälkeen sellainen löytyikin. Muuten esimerkkitila näytti samanlaiselta kuin olisin itsekin tehnyt, paitsi luultavasti ensimmäinen rivi olisi luultavasti unohtunut aluksi, lisäksi tuo hold: true kohta olisi jäänyt laittamatta (manuaalin mukaan tarkoittaa, että paketti pakotetaan pysymään nykyisessä asennetussa versiossa, järki?).

![Image](https://i.imgur.com/lkQXnjY.png)

