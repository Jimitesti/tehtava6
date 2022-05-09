
# H6 Akkuna

## v) Lue ja tiivistä artikkeli muutamalla ranskalaisella viivalla. Tässä z-alakohdassa ei tarvitse siis tehdä testejä tietokoneella.

Karvinen 2018: [Control Windows with Salt](https://terokarvinen.com/2018/04/18/control-windows-with-salt/)
- Tein pelkästään lokaalisti Windowsilla, mutta tämä artikkeli tiivistää hyvin mitä vaaditaan Windows koneen ohjaamiseen Saltilla.
- Verrattuna Linuxiin paljon enemmän rajatumpaa, mutta onneksi Choco on myös olemassa
- Ongelmakoodit myös paljon selvempiä Linuxissa kuin Windowsissa.
## a) Suolaikkuna. Asenna Salt Windowsiin. Jos ehdit jo asentaa, voit kirjoittaa muistinvaraisesti, mutta muista silloin merkitä, että tämä on muistista kirjoitettu. Näytä testillä (test.ping, file.managed tms), että Salt toimii.
Tätä tehtävää varten käytän viime tunnilla asennettua Windowsia, sekä kurssin alussa tehtyä Debian virtuaalikonetta. Windowsin iso-tiedoston sain ladattua emuloimalla Chromen dev-tooleilla Safaria (F12->Network Conditions->User Agent->Safari) Microsoftin sivuilta, josta normaalisti ladataan asennustyökalu.

Kirjoitan tämän muistista, mutta salt-masterin versio on jo kirjoitushetkellä uusin, joten minun piti vain ladata Windowsille salt-minion saltprojectin verkkosivuilta, asennuksessa annetaan vain masterin, eli Debianin IP ja Windowsille id, eli tunnistusnimi, tässä tapauksessa annoin nimen "windowsslave". 
Asennuksen jälkeen hyväksyin masterilla avaimen ja pingasin.

		$ sudo salt '*' test.ping
		
![testping](https://raw.githubusercontent.com/Jimitesti/tehtava6/main/Kuvat/testping.png)

Ja jos myöhemmin haluaa muokata Windowsin salt-asetuksia, ne löytyvät polusta:

		C:\ProgramData\Salt Project\Salt\conf
		
ProgramData on vakiona piilotettu kansio, joten piilotetut kansiot pitää kaivaa esiin Explorerin asetuksista.

## b) Single. Näytä komentorivillä Saltilla (state.single) esimerkit funktioista file ja cmd.

state.singlellä voidaan ajaa stateja suoraan komentorivistä esimerkkinä vaikkapa:

		$ sudo salt-call --local state.single file.managed /tmp/testi 

![tmptesti](https://raw.githubusercontent.com/Jimitesti/tehtava6/main/Kuvat/tmptesti.png)
Tekee kansioon tmp tiedoston testi.
Ja ajoin windowsslave minionilla komennon ipconfig nähdäkseni sen IP:n.
![ipconfigtesti](https://raw.githubusercontent.com/Jimitesti/tehtava6/main/Kuvat/ipconfigtesti.png)

## c) IaCcuna. Tee Windowsissa infraa koodina, ja aja se paikallisesti (salt-call --local state.apply foo)

Sain selville ajamalla tyhjän state.apply komennon powershellissä, että Salt hakee state-tiedostot seuraavasta polusta:

		C:\ProgramData\Salt\srv\salt
		
Joten tein kansiot srv ja salt, jonka jälkeen asensin Python3:n ja Git for Windowsin, jotta Saltstackin omat windows repot saadaan toimimaan.
Tämän jälkeen ajoin komennot

		salt-call --local winrepo.update_git_repos
		salt-call --local pkg.refresh_db
		
Joka periaatteessa toimii apt-get updatena. Tämä saltin paketinhallintaohjelma hakee siis SaltStackin omasta [GitHubista](https://github.com/saltstack/salt-winrepo-ng) paketit, jotka voidaan asentaa, joten tein staten joka asentaa Notepad++:n ja puttyn, staten nimi oli essentials.
Asennus toimi moitteetta.

![windowsessentials](https://raw.githubusercontent.com/Jimitesti/tehtava6/main/Kuvat/windowsessentials.png)
![windowsessentialsdone](https://raw.githubusercontent.com/Jimitesti/tehtava6/main/Kuvat/windowsessentialsdone.png)

Käytin tätä tehtävää varten seuraavaa SaltProjectin [dokumentaatiota.](https://docs.saltproject.io/en/latest/topics/windows/windows-package-manager.html)

## Lähteet:
Tero Karvinen 2018: https://terokarvinen.com/2018/04/18/control-windows-with-salt/
SaltProject - Windows Package Manager:  https://docs.saltproject.io/en/latest/topics/windows/windows-package-manager.html