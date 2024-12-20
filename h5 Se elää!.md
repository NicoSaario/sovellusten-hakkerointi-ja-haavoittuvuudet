# Kotitehtävät
Kotitehtävät ovat kurssilta "Sovellusten hakkerointi ja haavoittuvuudet - Application Hacking" ja löytyvät osoitteesta https://terokarvinen.com/application-hacking/#laksyt

Kotitehtävät on tehty Windows 11 - Home - käyttöjärjestelmällä, päivitykset ajettu 24/11/2024 asti.

AMD Ryzen 5 4500U, RAM 8 Gt.

Aika: Itä-Euroopan normaaliaika Aikavyöhyke: Suomi (UTC+2) 24/11/2024 (pvm/kk/v)

Kaikissa testaukseen liittyvässä:
Oracle VM VirtualBox ja Debian 12 Bookworm


## a) Lab1. Tutkiminen mikä on ohjelmassa vialla ja miten se korjataan. lab1.zip

Aloitin tehtävän tekemällä vanhalla tutulla ```strings``` - komennolla. Tällä kertaa sieltä ei kuitenkaan löytynyt muuta mielenkiintoista, kuin nämä good ja bad - messaget: <img width="194" alt="image" src="https://github.com/user-attachments/assets/14bdbecd-2f0e-4ddd-ab58-f774cb1fb911">

- Käännetään koodi siis ajettavaan muotoon jaettujen kalvojen ohjeiden mukaan, eli ```gbd -o main gbd_example1.c -g```:

<img width="329" alt="image" src="https://github.com/user-attachments/assets/fc96923f-abfa-4781-ac85-3d6d5771a60f">

- Nähdään, että sinne ilmestyi tuo "main" -  <img width="174" alt="image" src="https://github.com/user-attachments/assets/52e6cc3d-80c8-47d5-b1da-57f51acc5f41">

- Ajetaan se debuggerilla ```gbd ./main```
- ```run```- komennolla katsotaan, mitä se tekee <img width="233" alt="image" src="https://github.com/user-attachments/assets/eb556956-1bf1-4396-b5a6-9a5f39822f41">

- Jouduin hieman opettelemaan tän koko homman käyttöä, koska jäi ensimmäinen tunti oppitunnista väliin
- Löysin kuitenkin hyvän tutoriaalin aiheeseen liittyen, jossa avataan vähän enemmän sitä, miten eri näkymiä ja muita saa oikeasti selville https://www.youtube.com/watch?v=Dq8l1_-QgAc

- Lisäsin breakpointin main - funktioon ```breakpoint main``` ja hakkasin ```nexti```ä niin kauan, että tapahtui jotain. Ensin tuli tuo "Khoor/#zruog1" ja vähän aikaa naputtelemalla tuli Segmentation fault, joka oli myös aikaisemmissa kuvankaappauksissa. Ongelma oli siis kuvassa näkyvien valkoisten viivojen välillä.

- Hetken ihmettelin, miksei mikään näkymä kerro mitään ja katsoin videon uudelleen, joten käytin ```lay next```, jolloin myös koodi tuli näkyviin
- Tein saman homman, eli naputtelin nextiä hetken aikaa ja tuli seuraava näkymä

<img width="344" alt="image" src="https://github.com/user-attachments/assets/b8882e24-4f57-4e30-ba1b-9ca91f4ad20c">

- Ongelma löytyy siis tuosta kuvassa näkyvästä kohdasta

- Halusin vähän lisää selvitystä, joten kysäsin ChatGPTltä, miten saisin tuosta nimenomaisesta kohdasta lisätietoa
- Se ehdotti, että laitan ```watch *message```ja breakpointin tuohon kohtaan, jossa ongelma ilmenee eli ```break *0x555555514f```

- <img width="456" alt="image" src="https://github.com/user-attachments/assets/ca74a560-c9fc-47d4-83da-a36bdd162a3a">

- <img width="419" alt="image" src="https://github.com/user-attachments/assets/0236b28b-0b7c-494c-abb1-af89b7355ad9">

<img width="416" alt="image" src="https://github.com/user-attachments/assets/82d69817-ed2e-469d-99f8-5fff6522868c">

<img width="443" alt="image" src="https://github.com/user-attachments/assets/b621ecd7-4db2-4101-aa90-d04a99ae1d0a">

<img width="435" alt="image" src="https://github.com/user-attachments/assets/255aed16-a562-454c-a207-2d3621eb9619">

- Kuten kuvista näkyy, "Hello World" pienenee joka kohdassa yhdellä kirjaimella. Viimeisenä palauttaa arvon 18, tämän jälkeen koko ohjelma kaatuu

- <img width="434" alt="image" src="https://github.com/user-attachments/assets/1c10e8e2-e6e5-4edf-ae70-99a742319285">

- Eli Lopputulos: Kun silmukka on käynyt kaikki "Hello World" -merkkijonon merkit läpi, message siirtyy muistialueen ulkopuolelle ja kaatuu.
- Tämä jäi taas vähän oman osaamisalueeni ulkopuolelle, joten konsultoin hieman tekoälyä apuna ja vastauksena oli, että saadaan 

<img width="493" alt="image" src="https://github.com/user-attachments/assets/41e298c3-a9b8-4338-9283-352ee3766da7">

- Tehtävä vähän laajeni, mutta halusin testata tuon lopputuloksen. Muutin siis koodin kokonaan seuraavasti:

<img width="274" alt="image" src="https://github.com/user-attachments/assets/0316c3df-3b68-41e8-a2a7-ff36673168a8">

- Ja lopputuloksena ohjelma antoi seuraavan:
- ![Näyttökuva 2024-11-24 203514](https://github.com/user-attachments/assets/fc0e3637-f855-407e-a9e5-ea96a031d105)
- Nyt se palauttaa Error: message is Null.

## b) Lab2. Selvitä salasana ja lippu + kirjoita raportti siitä miten aukesi. lab2.zip

Sain ensin tuon passtr - salasanan ja lipun pelkällä ```strings```- komennolla

<img width="316" alt="image" src="https://github.com/user-attachments/assets/b585e03c-1fd9-49b7-8161-d3a8e0da4b79">

- Lähdin sitten selvittelemään tuota passtr2o - tiedostoa joka varmaan se oikea tehtävä oli. ```strings``` - komennolla ei tullut mitään maata mullistavaa:

  <img width="186" alt="image" src="https://github.com/user-attachments/assets/ee5d8439-9885-4537-a2a7-1f2cf7bcb948">

- Olin siis aivan kassalla koko tehtävän kanssa pitkän aikaa. Leikin gdb:n kanssa pitkän aikaa ja kokeilin lähes kaikkea, mitä tuli mieleen enkä edennyt mihinkään suuntaan. Päätin lopulta avata tiedoston ghidralla ja löysin sieltä salasanan noin 15 - sekunnissa.

<img width="652" alt="image" src="https://github.com/user-attachments/assets/b2b245c7-7930-4f30-a448-24a2615a6b84">

- Kokeilin tätä salasanaa ja se ei toiminut

  ![image](https://github.com/user-attachments/assets/b13bc15e-efe7-4df6-b7cb-942179356093)

- Mietin tässä kohtaa, että onko kyseessä sama jippo, mitä käytiin tunnilla läpi eli salasana pitää muuttaa ASCII - merkistöllä toiseen muotoon
- Mutta miten? Siihen pisteeseen oon nyt päässy, että "anLTj4u8" ASCII-koodina on:
97 110 76 84 106 52 117 56

- Se jollain tapaa varmaan viittaa tohon "mAsdf3a", mikä näkyy Ghidrassakin, mutta en saa kyllä mitään tolkkua että miten.
- Sen verran kauan on päätä hakattu nyt seinään, että pieni tauko tekee hyvää. Palaan myöhemmin

---Tähän siis luova tauko---25.11.2024 klo 01:15 - Aloitus oli 24.11.2024 noin klo 17
---Jatkuu 25.11 klo 10.30---

- Jatkoin vähän lisää tuon ghidran pyörittämistä ja selailin lisää tuota mAsdf3a - kohtaa ja kiinnitti huomiota tuo +3 ja + -7. Sen verran näitä on tullu pyöriteltyä viimeaioina, että ymmärsin niiden liittyvän jollain tavalla salasanaan.

<img width="878" alt="image" src="https://github.com/user-attachments/assets/e91a7f18-3e65-4c7f-8fff-4ab414e22832">


- Kysyin jälleen ChatGPT:ltä, että miten nuo voisivat liittyä tuohon salasanaan, johon se vastasi näin:


> Käydään tämä askel askeleelta:

> Funktio käy läpi jokaisen alkuperäisen merkkijonon merkin ja suorittaa sille seuraavat operaatiot:

> Parillisilla indekseillä (index % 2 == 0):
> Lisää merkin ASCII-koodiin arvon +3.

> Parittomilla indekseillä (index % 2 != 0):
> Vähennä merkin ASCII-koodista arvon -7.

- Eli jos "anLTj4u8" ASCII-koodina on:
97 110 76 84 106 52 117 56

- Kokeillaan muuttaa se tuon avulla
- 97 + 3 = 100
- 110 + 3 = 103
- 76 - 7 = 69
- 84 + 3 = 87
- 106 + 3 = 109
- 52 + 3 = 55
- 117 - 7 = 110
- 56 + 3 = 59
- Lopputulos: ```100 103 69 87 109 55 110 59```
- Muutetaan se takaisin alkuperäiseen muotoon tämän avulla https://www.asciitable.com/
- 100 = d
- 103 = g
- 69 = E
- 87 = W
- 109 = m
- 55 = 7
- 110 = n
- 59 = ;

- Tämän mukaan oikea salasana olisi siis ```dgEWm7n;```

- Ei mennyt läpi. Huomasin, että olin tehnyt aika monta huolimattomuusvirhettä itse laskennassa ja tajusin samalla, ettei tuo pariton - parillinen tarkoita itse lukuja, vaan järjestystä. Eli 1 = 97, 2 = 110 jne.

- Parillinen (0) 97 + 3 = 100 = d
- Pariton (1) 110 - 7 = 103  = g
- Parillinen (2) 76 + 3 = 79 = O
- Pariton (3) 84 - 7 = 77 =  M
- Parillinen (4) 106 + 3 = 109 = m
- Pariton (5) 52 - 7 = 45 = -
- Parillinen (6) 117 + 3 = 120 = x
- Pariton (7) 56 - 7 = 49 = 1

- Lopullinen salasana olisi siis ```dgOMm-x1```

  <img width="365" alt="image" src="https://github.com/user-attachments/assets/b5c1899e-0eab-459d-ad6e-600fd1d8d6c1">

- Yritin opetella tuota gbd käyttöä ja pääsin mielestäni aika pitkälle sen kanssa, mutta en olisi kyllä löytänyt tässä tehtävässä perille ilman ghidran apua. Pyrin palaamaan tähän tehtävään sitten, kun ymmärrän enemmän koko gbd:n toiminnasta
  
## c) Lab3. Kokeile Nora Crackmes harjoituksia tehtävä 3 ja 4 ja loput vapaaehtoisia. Tindall 2023: NoraCodes / crackmes.
Tämän teen vielä. Aika ei riittänyt

### Lähteet

Intro to Debugging w/ GDB (PicoCTF 2022 #11 'gdb-test-drive'), John Hammond, 2022, Katsottavissa:
https://www.youtube.com/watch?v=Tmdnsre9z7s&t=26s (Katsottu 25.11.2024)

https://www.asciitable.com/

ChatGPT - molemmissa apuna

Läksyt, Tero Karvinen, Luettavissa:
https://terokarvinen.com/application-hacking/#laksyt

Intro to Debugging w/ GDB (PicoCTF 2022 #11 'gdb-test-drive'), John Hammond, Katsottavissa:
https://www.youtube.com/watch?v=Tmdnsre9z7s&t=26s (katsottu 24.11.2024)

