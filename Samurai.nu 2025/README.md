# ctf.samurai.nu

##  WARMUP: TXT Stands for Text 

dig TXT samurai.nu

        
    ; <<>> DiG 9.20.15-2-Debian <<>> TXT samurai.nu
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 63280
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 1232
    ;; QUESTION SECTION:
    ;samurai.nu.                    IN      TXT

    ;; ANSWER SECTION:
    samurai.nu.             3600    IN      TXT     "Flag: Wh4t_D0_W3_H4v3_H3r3"

    ;; Query time: 44 msec
    ;; SERVER: 192.168.1.1#53(192.168.1.1) (UDP)
    ;; WHEN: Mon Dec 15 16:34:09 CET 2025
    ;; MSG SIZE  rcvd: 78

"Flag: Wh4t_D0_W3_H4v3_H3r3"

##  WARMUP: ElfTP 

 sudo nmap -sV -O -T5 -p- 207.154.225.131 

    Starting Nmap 7.95 ( https://nmap.org ) at 2025-12-15 16:38 CET
    Nmap scan report for 207.154.225.131
    Host is up (0.012s latency).
    Not shown: 65533 filtered tcp ports (no-response)
    PORT   STATE SERVICE VERSION
    21/tcp open  ftp     vsftpd 3.0.5
    22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.13 (Ubuntu Linux; protocol 2.0)
    Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
    Device type: bridge|VoIP adapter|general purpose
    Running (JUST GUESSING): Oracle Virtualbox (98%), Slirp (98%), AT&T embedded (95%), QEMU (94%)
    OS CPE: cpe:/o:oracle:virtualbox cpe:/a:danny_gasparovski:slirp cpe:/a:qemu:qemu
    Aggressive OS guesses: Oracle Virtualbox Slirp NAT bridge (98%), AT&T BGW210 voice gateway (95%), QEMU user mode network gateway (94%)
    No exact OS matches for host (test conditions non-ideal).
    Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

ftp 207.154.225.131


    Connected to 207.154.225.131.
    220 (vsFTPd 3.0.5)
    Name (207.154.225.131:kali): anonymous
    331 Please specify the password.
    Password: 
    230 Login successful.
    Remote system type is UNIX.
    Using binary mode to transfer files.
    ftp> ls
    229 Entering Extended Passive Mode (|||21009|)
    150 Here comes the directory listing.
    -rw-r--r--    1 0        0              57 Dec 11 13:00 flag.txt
    226 Directory send OK.
    ftp> get flag.txt
    local: flag.txt remote: flag.txt
    229 Entering Extended Passive Mode (|||21002|)
    150 Opening BINARY mode data connection for flag.txt (57 bytes).
    100% |**********************************************************************************************************************************************************************|    57       70.37 KiB/s    00:00 ETA
    226 Transfer complete.
    57 bytes received in 00:00 (2.34 KiB/s)
    ftp> exit
    221 Goodbye.

└─$ cat flag.txt         
H0_H0_H0_R3m3mb3r_T0_U53_Pr0p3r_Cr3d3nt14l5_F0r_S3rv1c35

##  TUTORIAL: Using the Broker 

Ja, det här var verkligen en tutorial, klicka sig genom och få flaggan:

Flag: Br0k3r_Tut0r14l_C0mpl3t3d

## Rulez

Tittade på källkoden, sökte rätt på en QR-kod.png-fil och körde den i en online qr-läsare för att få fram:

O24{I_hereby_consent_to_and_will_fully_comply_with_all_CTF_rules_terms_scope_and_consequences_and_accept_full_responsibility_for_my_actions}

## WARMUP: Reversing 

strings kongstrong 

    === KONG-STRONG FLAG TEST ===
            ,--.
        ,-(    ).
        ,'        `.    
        (  O    O    )
    /|     __     |\
    / |\   (__)   /| \
    /  | `-.____.-' |  \
    |   |  /      \  |   |
    |   | |        | |   |
    \  | |        | |  /
    `.| |        | |.'
        `-'        `-'
    Correct! Kong-Strong is pleased
    Enter flag: 
    Input error.
    O24{String_Strong}
    Wrong Kong! Try again!

O24{String_Strong}

## WARMUP: A Crack in the Nick of Time 

Körde 3EB4B55827DDEB3DCD78AB47D42A5E4D genom crackstation och fick fram:

Neonlights

## WARMUP: Look Closer

Här bråkade jag ett tag med binwalk och extraherade alldeles för långt innan jag började inspektera de tidigare filerna.

binwalk -e flag.png <br>
 cd _flag.png.extracted <br>
 cat 128686.zip<br>

    PK
    �H�[    Ҏ�/flag.txtUT   �{:i�{:iux
                                    ��O24{f!l3$_4R3_noT_A1w@y$_wh@t_7heY_sE3m_T0_b3}
    PK
    �H�[    Ҏ�/▒����flag.txtUT�{:iux
                                    ��PKN��       

O24{f!l3$_4R3_noT_A1w@y$_wh@t_7heY_sE3m_T0_b3}

## Live Substring

Tittade på videon och då fick jag flaggan:

1. O24{
2. s3
3. v3
4. rG
5. 1n
6. }

Så flaggan blir: O24{s3v3rG1n}

## WARMUP: Hacking a Bank App 

Jag använde mig av Burp Suite. Hoppade in på den tänkta IP:n - http://157.230.19.8/ och inspekterade vad som dök upp när jag klickade runt. Det jag hittade som var intressant var framför allt detta:

![Burp Suite](<Skärmbild 2025-12-17 warmup hacking a bank app - burp suite .png>)

Med den informationen så skickade jag den requesten till intruder och började kolla på andra nummer som låg i närheten av det befintliga numret. Och vips fick jag detta som svar:

    {"date":"2025-12-04","message":"Uh yeah I've been having some card issues lately, sometimes it will error out when attempting to pay even though theres money in the account. These issues have started to po up more ferequently the last 3 or 4 months or so. Is there something wrong with my card? Here are my card details, ca you please check if there something wrong with it:<br><br>Card number: 4916 1234 5678 9012<br>Expiry date: 11/29<br>CCV: 574<br><br><br><br>--- 4TH WALL BREAK ---<br>------------------------------------------------------------------------------------------------------------<br><i>This is a message from John, a junior webapp pentester at Outpost24 and the author of this challenge</i><br><br>Congratulations, you solved the challenge! Here is your flag: <b>N3v3r_Und3r35t1m4t3_Th3_P0w3r_Of_ID0R</b><br><br>And just like that, you discovered that you can read arbitrary users' bank messages, as the adrenaline hits you and thrill of the finding sets in. Sometimes, it really is this easy to be a hacker, you just change a number. The difficulty comes from the scale of the apps we test here in AppSec, and the fact that you can't know for sure whether a certain feature or parameter is even vulnerable to begin with. In this exercise, I tried to mimic a fraction of that scale by utilizing a lot of parameters in the POST request, most of which don't have any purpose in this mock app. This was also the only funcionality of the app, a far cry from the real world. As a pentester, you need to be able to quickly and efficiently identify which parts of an application look interesting and focus your efforts there - you won't have time to go in-depth on every single parameter. This is in contrast to a CTF challenge, where the apps are orders of magnitue smaller and you know the app is vulnerable. Still, playing CTFs and doing labs is one of the best and most fun ways to get into web app pentesting, take it from me!<br><br>I hope you enjoyed and good luck with the rest of the CTF!<br>------------------------------------------------------------------------------------------------------------","subject":"Card issues","type":"Support"}

Alltså är flaggan:
N3v3r_Und3r35t1m4t3_Th3_P0w3r_Of_ID0R

## WARMUP: Asssisted Overflow

Här fick jag lära mig nya saker. Jag har aldrig använt pwntools tidigare. Det tog mig ett tag att få igång en virtuell miljö att arbeta i så att jag kunde installera och få igång pwntools. Men till slut så gick det vägen. Eftersom det var första gången jag hållit på med något sånt här så tog jag rätt stor hjälp av AI för att lära mig hur jag skulle skriva min kod och sedan få det att fungera. Provade mig fram med en hel del varianter på den nedladdade c-programmerade versionen. Där hittade jag ledtråden om struct.pack. Efter det förstod jag att ledtråden bara var 8 bytes och att jag behövde det på slutet av strängen som skulle levereras. Det hela resulterade i följande kod.

Riktigt roligt och lärorikt för mig som är nybörjare när det kommer till CTF.

![VSCode + terminal](<Skärmbild 2025-12-17 warmup assisted overflow.png>)

Flaggan är alltså:

O24{0verfloW_@_uR3_$3rv!cE}

## WARMUP: Programmering

Ännu en intressant, får fortsätta använda mig av pwntools och det blev en hel del konversation med AI fram och tillbaka för att få programmet att fungera. Det här är ju som sagt något jag är obekant med. Men jag fick det att fungera till slut!

![Mer VSCode + terminal](<Skärmbild 2025-12-18 warmup programming.png>)

Faktiska koden jag använde till slut, en del skriven av mig och en del av AI.

    from pwn import *
    import re

    context.log_level = 'debug'

    pattern = re.compile(
        rb'\s*'
        rb'(?P<a>\d{1,2})\s+'
        rb'(?P<symbol>[+\-*/])\s+'
        rb'(?P<b>\d{1,2})\s+='
    )

    io = remote('64.226.92.2', '5000')

    while True:
        try:

            question = io.recvuntil(b'= ')
            log.info(f"Question raw: {question!r}")

            if not question:
                break

            m = pattern.search(question)
            if not m:
                log.info("Inte en fråga")
                continue

            a = int(m.group('a'))
            op = m.group('symbol')
            b = int(m.group('b'))

            if op == b'+':
                result = a + b
            elif op == b'-':
                result = a - b
            elif op == b'*':
                result = a * b
            elif op == b'/':
                result = a // b


            log.info(f"Answer: {result}")
            io.sendline(str(result).encode())

        except EOFError:
            log.info("Connection closed by remote")
            break

    io.interactive()

Nice! Here is your flag: O24{fast_fingers_or_fast_code}

## FlagInThePackets

Ännu ett för mig klurigt och roligt rum!

Jag började med att öppna filen i Wireshark och tittade på TCP streamen, där hittade jag följande text:

![Ledtråd 1](<Skärmbild 2025-12-18 flaginthepacket - wireshark.png>)

Det fick mig att tänka på att jag nog kan extrahera alla liknande i terminalen med:


 strings capture.pcap | grep "You picked up the"         

    You picked up the I character at position 29:12/1
    You picked up the P character at position 31:1541
    You picked up the T character at position 33:1671
    You picked up the T character at position 33:1771
    You picked up the M character at position 17:15@1
    You picked up the N character at position 15:17B1
    You picked up the R character at position 5:16E1
    You picked up the _ character at position 3:12O1
    You picked up the I character at position 1:15R1
    You picked up the R character at position 21:5
    You picked up the O character at position 9:5
    You picked up the 2 character at position 7:1
    You picked up the { character at position 2:5
    You picked up the O character at position 1:1
    You picked up the D character at position 31:5
    You picked up the A character at position 53:16R2
    You picked up the O character at position 47:15Z2
    You picked up the E character at position 43:11_2
    You picked up the 4 character at position 55:3m2
    You picked up the S character at position 57:12x2
    You picked up the _ character at position 57:13x2
    You picked up the R character at position 55:11
    You picked up the } character at position 57:17

Efter det så satt jag och klurade och frågade AI vad positionerna kunde betyda, den ville att det skulle vara byte-positioner och det ena med det andra. Men den tyckte att jag skulle titta efter kolon-tecknet. Vilket jag till slut gjorde och eftersom jag vet att flaggan börjar med O24{ och slutar med } så började jag sätta dem i ordning manuellt och landade till slut på följande:

    You picked up the O character at position 1:1
    You picked up the 2 character at position 7:1
    You picked up the 4 character at position 55:3m2
    You picked up the { character at position 2:5
    You picked up the O character at position 9:5
    You picked up the R character at position 21:5
    You picked up the D character at position 31:5
    You picked up the E character at position 43:11_2
    You picked up the R character at position 55:11
    You picked up the _ character at position 3:12O1
    You picked up the I character at position 29:12/1
    You picked up the S character at position 57:12x2
    You picked up the _ character at position 57:13x2
    You picked up the I character at position 1:15R1
    You picked up the M character at position 17:15@1
    You picked up the P character at position 31:1541
    You picked up the O character at position 47:15Z2
    You picked up the R character at position 5:16E1
    You picked up the T character at position 33:1671
    You picked up the A character at position 53:16R2
    You picked up the N character at position 15:17B1
    You picked up the T character at position 33:1771
    You picked up the } character at position 57:17

Flagga: O24{ORDER_IS_IMPORTANT}

# WARMUP: Stego

Det här var en av de första jag provade under dag ett. Jag ska göra en ny omgång nu och se om jag missade något förra gången.

 exiftool stego.jpg

    ExifTool Version Number         : 13.36
    File Name                       : stego.jpg
    Directory                       : .
    File Size                       : 473 kB
    File Modification Date/Time     : 2025:12:15 16:26:39+01:00
    File Access Date/Time           : 2025:12:18 14:00:48+01:00
    File Inode Change Date/Time     : 2025:12:18 14:00:48+01:00
    File Permissions                : -rw-rw-r--
    File Type                       : JPEG
    File Type Extension             : jpg
    MIME Type                       : image/jpeg
    JFIF Version                    : 1.01
    Resolution Unit                 : inches
    X Resolution                    : 96
    Y Resolution                    : 96
    Image Width                     : 1536
    Image Height                    : 1024
    Encoding Process                : Baseline DCT, Huffman coding
    Bits Per Sample                 : 8
    Color Components                : 3
    Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
    Image Size                      : 1536x1024
    Megapixels                      : 1.6


 binwalk stego.jpg  

    DECIMAL       HEXADECIMAL     DESCRIPTION
    --------------------------------------------------------------------------------
    0             0x0             JPEG image data, JFIF standard 1.01

steghide extract -sf stego.jpg

    Enter passphrase: 
    steghide: could not extract any data with that passphrase!

Jag provade köra strings stego.jpg, inget utmärkande såg ut att finnas där.

convert stego.jpg -channel Red -separate red_channel.png

convert stego.jpg -channel Green -separate green_channel.png

convert stego.jpg -channel Blue -separate blue_channel.png

convert stego.jpg -edge 1 stego_edges.png

Tittar på alla bilderna och kan inte se något uppenbart.

 binwalk red_channel.png

    DECIMAL       HEXADECIMAL     DESCRIPTION
    --------------------------------------------------------------------------------
    0             0x0             PNG image, 1536 x 1024, 8-bit grayscale, non-interlaced
    95            0x5F            Zlib compressed data, best compression

Efter ovanstående försökte jag extrahera zlib-datan, men det kändes för djuptgående. Så jag går vidare.

zsteg stego.png      

    meta date:create    .. text: "2025-12-18T13:00:48+00:00"
    meta date:modify    .. text: "2025-12-15T15:26:39+00:00"
    meta date:timestamp .. text: "2025-12-18T13:39:13+00:00"
    imagedata           .. text: "\n\n\n\r\r\r\t\t\t"
    b2,r,lsb,xy         .. text: "UQTE-@ZASLi"
    b2,g,lsb,xy         .. text: "dUUUZUY@iF"
    b2,g,msb,xy         .. file: OpenPGP Public Key
    b4,r,lsb,xy         .. text: "ECDD\"\"fffUDDD!"
    b4,g,lsb,xy         .. file: TeX font metric data
    b4,b,lsb,xy         .. text: "DT3EVfUgDC3D3EUB4C4CDETD3DDDDUUg#333!"
    b4,rgb,lsb,xy       .. text: "A%\"2#A5$af"
    b4,bgr,lsb,xy       .. text: "!2#\"1D%af"

zsteg stego.png -E b2,g,msb,xy > key.bin

 file key.bin 

    key.bin: OpenPGP Public Key


strings key.bin | grep "O24" verkar inte ha gett något.

gpg --list-packets key.bin

    gpg: keybox '/home/kali/.gnupg/pubring.kbx' created
    gpg: packet(6) with unknown version 160
    # off=0 ctb=9a tag=6 hlen=5 plen=1621647402
    :key packet: [unknown version]

Då kollar vi i /home/kali/.gnuopg/pubring.kbx

Ingenting av värde där heller. Känns som jag börjar vara ute och cyklar igen.

Tyvärr måste jag nog överge det här rummet ändå. Det känns som att jag inte kommer på något mer själv och jag har dividerat väldigt mycket fram och tillbaka med AI utan framgång.

## Silent Knight

Öppnade Burp igen för att titta runt. Hittade inte så mycket. Frågade AI efter idéer eftersom det inte fanns så mycket ledtrådar. Läste om texten och fick känslan att det borde ha med git eller något att göra. Frågade AI lite mer och den föreslog att jag skulle prova om jag kom åt /dev/ vilket jag gjorde!

http://165.22.24.188:3000/dev/.git/logs/HEAD 

    0000000000000000000000000000000000000000 a4370e16394b6d237ed364a83866348e7cedcda3 Kenshi Kringle <kringle@silentknight.com> 1765179060 +0000	commit (initial): Initial commit O24{giTing_

Så jag bestämde mig för att ladda hem alltihop så det är lättare att titta närmare på saker.

 wget -r -np -nH --cut-dirs=1 -R "index.html" http://165.22.24.188:3000/dev/.git/


    ...
    FINISHED --2025-12-19 08:08:30--
    Total wall clock time: 12s
    Downloaded: 270 files, 5.2M in 0.3s (19.3 MB/s)

Nu ska vi testa trufflehog för första gången, se om det fungerar som jag tror och hoppas.

Det fungerade inte som jag hade hoppats, det söker efter specifika saker, inte riktigt det jag var ute efter, men såhär i efterhand så verkar det ha gjort ett dåligt jobb också, för det borde faktiskt ha hittat andra delen av flaggan eftersom andra delen var ett password inbäddat i git-grejerna.

I alla fall. Jag provade väldigt mycket saker efter trufflehog, jag konverserade mycket med AI för att komma framåt och det tog mig ett par timmars arbete. Som tidigare sagt så är jag oerfaren och kan inte hur git fungerar exakt men har lärt mig en del om det hela nu gällande tree-strukturer och hur .git/objects/ har en massa hash-filer som man kan titta i osv osv.

Jag fick känslan att det var någon borttagen fil eller liknande som innehöll andra delen av flaggan. Jag försökte med hjälp av "relevanta" commits packa upp vissa hashar för att läsa innehållet och förhoppningsvis ta mig vidare, jag använde ett AI-genererat script till detta som såg ut såhär:


    #!/bin/bash
    # Script: decompress_git_object.sh
    # Usage: ./decompress_git_object.sh <git-object-file>

    if [ -z "$1" ]; then
        echo "Usage: $0 <git-object-file>"
        exit 1
    fi

    FILE="$1"

    python3 - <<EOF
    import zlib
    data = open("$FILE", "rb").read()
    print(zlib.decompress(data).decode('utf-8', errors='replace'))
    EOF

Det var mycket användbart för det ändamålet. Jag kunde alltså skriva:

./unpack.sh .git/object/xx/resten-av-hashen

Och med det så fick jag innehållet och kunde försöka traggla mig vidare. Men när jag förstod att jag inte kommer hitta något vettigt på det här sättet så skapade jag följande script med hjälp av AI:

    for file in .git/objects/*/*; do
        ./unpack.sh "$file"
    done > tmp/allt.txt

Det här borde då, om jag förstått det rätt, extrahera allt från alla hashes och stoppa det i min allt.txt-fil. För att göra det läsbart för mig så körde jag:

strings allt.txt > allt2.txt

grep "}" allt2.txt | less

Och det första jag ser är:

    const controller = {}
        res.render('login', { message: 'Login Page' });
        if (username === "admin" && password === "b3ttEr_I_se3}") {
            res.send({ "message": "Successful login" })
        } else {
        }

Lärorikt och frustrerande men nu har jag äntligen hela flaggan som är:

O24{giTing_b3ttEr_I_se3}