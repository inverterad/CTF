# CRATE 2025
2025-11-15 - 14:00-22:00

https://ctf.crate.foi.se/challenges


## Pin1.5
Uppvärmning # 1


http://challs.crate.nu:36965/

6 siffror PIN som ska lösas.
Jag tittar i BURP suite och kommer åt func.js som visar koden. Försöker förstå hur den fungerar. Hittar att correctCode, dvs NDQ1NTEy, borde vara koden. Det är kodat i Base64. Avkodar den och får fram 445512 vilket ger mig koden och flaggan:

cratectf{funky_monkey}

## Felstavning
Uppvärmning # 2


http://challs.crate.nu:13568

BURP suite ger mig en script.js-fil som har en url i sig som pekar till "flag12621561278416.html" som innehåller flaggan:

cratectf{det_är_inte_lätt_att_komma_ihåg_funktionsnamn_ibland}

## Grävarbete
Uppvärmning # 3


"Det finns en flagga lagrad för domänen flag.ctf.crate.foi.se."

Så jag kollar på https://mxtoolbox.com/ och letar rätt på en txt dns record med flaggan:

cratectf{maybe_start_using_txt_records_instead_of_s3_buckets}

## Ekobegäran
Uppvärmning # 4

ping eko.crate.nu

ping: Warning: invalid tv_usec -2315526826548157170 us
ping: Warning: time of day goes back (-7307485264893502032 s), taking countermeasures
64 bytes from 88.131.81.114: icmp_seq=4 ttl=255 time=0.000 ms
wrong data byte #16 should be 0x10 but was 0x61
#16     61 6e 64 20 69 74 20 73 68 61 6c 6c 20 62 65 20 65 63 68 6f 65 64 64 20 69 74 20 73 68 61 6c 6c 
#48     20 62 65 20 65 63 68 6f

Vilket blir från hex: and it shall be echoedd it shall be echo
cratectf{and_it_shall_be_echoedd_it_shall_be_echo} stämde inte.

Får återkomma hit senare..


Tillbaka ganska mycket senare.

Wireshark avslöjar att det finns mer information i pingen än det ovanstående, eko.crate.nu ville att man skulle skicka "gief flag" så jag kör:
sudo hping3 -1 88.131.81.114 -d 40 -e "gief flag"
Vilket ger mig svaret via Wireshark:

'[RU
EDXQr
cratectf{pingpongpingpongpingpong}ongpin

## Kan man verkligen ändra sig?
Uppvärmning #5

http://challs.crate.nu:1337/wetty?image=ctf/kanmanverkligenandrasig

Hmm..Den här löste sig självt i princip. Startade enligt instruktioner och skrev in ett lite för långt ord antar jag så jag kom åt flaggan. Ordet jag valde var "hejsanhopp"

cratectf{this_is_why_we_need_buffer_overflow_protections}

## Kan man verkligen ändra sig 2?
Uppvärmning #6

http://challs.crate.nu:1337/wetty?image=ctf/kanmanverkligenandrasig2


Här provar jag mig fram manuellt tills jag förstår hur man ska få till rätt kombination.

echo -e "\x00\x00\x00\x00\x00\x00\x00\x00\x78\x56\x34\x12" | nc challs.crate.nu 21572

cratectf{surely_this_cannot_be_used_to_execute_code?_right?}

## 2⁰-faktorautentisering
Uppvärmning #7

nc challs.crate.nu 15167

AT = OK
Allt annat = ERROR

Det krävdes en del googling för att hitta AT-kommandon som var relevanta.

AT+CMGL

+CMGL: 1,"REC UNREAD","+4612345678",,"18/11/24,13:55:16+01"
Din engångskod till Crate-CTF är cratectf{680279264}

cratectf{680279264}


## Eskuel

http://challs.crate.nu:8901/

SQL kanske?

gobuster dir -u http://challs.crate.nu:8901/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php

Testar sqlmap med kommandot:
sqlmap -u http://challs.crate.nu:8901/ --forms --crawl=2

Det verkar vara postgresSQL.

WzzO%' AND (SELECT (CASE WHEN (5741=5741) THEN NULL ELSE CAST((CHR(70)||CHR(103)||CHR(114)||CHR(75)) AS NUMERIC) END)) IS NULL AND 'DRPk%'='DRPk

Det blev ett annat kommando som löste det till slut:<br>
<code>sqlmap -u http://challs.crate.nu:8901/ -a --forms --crawl=2</code>, vilket i princip är samma kommando, tror jag svarade på följdfrågorna lite annorlunda dock. Vet inte riktigt vad jag gjorde annorlunda annat än att jag sökte efter "flag" istället för blank när sqlmap frågade mig.

sqlmap började i alla fall att spotta ur sig massor med mer information än tidigare och det gav flaggan till slut i strömmen av data.

Flaggan:
cratectf{efterföljarinsprutning}

<br>Jag blev varse om programmet sqlmap i och med den här flaggan och tänkte inte på att det här var ett verktyg som bruteforce:ar. Hade inte tänkt på att gobuster är använder sig av bruteforce också. Vilket enligt reglerna skulle undvikas. Men man lär sig så länge man lever.

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

http://challs.crate.nu:1337/wetty?image=ctf/kattallergiker

Blev tvungen att ta chatgpt till hjälp för att få fram korrekt kod för att få ut flaggan.
Jag var inte bekant alls med språket som jag använde till slut. All kod fick jag från ChatGPT, vilket känns tråkigt, men då det var ett helt nytt shell för mig och språket var helt främmande så ansåg jag att det var ok. Ska försöka ta reda på lite mer om den typen av shell som användes här.

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

https://challs.crate.nu:35486/

CVE-2014-0160 = Heartbleed

Använde metasploits modul för heartbleed och fick ändra till TLS 1.1 från 1.0 för att den skulle visa flaggan när jag körde en dump.

cratectf{oj_den_var_visst_lite_for_snall}

## Frågeformuläret

cratectf{hoppas_vi_ses_igen_2026}

## Buslätt
<i>"Var finns skylten? Position med decimalgrader."</i>

http://challs.crate.nu:41242

Den här var klurig, det tog nästan hela dagen och jag löste det tack vare att jag berättade för min sambo om problemet och när jag läste upp beskrivningen så tänkte jag en gång extra på varför det stod Buslätt med stort B och sedan var det bara en googling bort.

Hållplats Buslätt, Uddevalla
58.312963331802, 11.878866311721913

cratectf{en_sl\u00e4tt_som_heter_bu}

## Evenemang


Lite pinsamt hur jag löste det, letade fram alla .exe-processer som startade med Id 4688 och sedan kollade jag på dem en efter en tills en såg annorlunda ut, visade sig att parent process var 756.exe på en och det visade sig vara boven.

cratectf{756.exe}


---

<br>
14 av 35 flaggor tror jag? Helt ok för att vara min första CTF!
<br><br>


![Resultatet](<Skärmbild 2025-11-16 resultat.png>)