# Hack the Box - Holmes 2026

Jag var med i en grupp och på grund av lite begränsat med tid, och kanske framför allt att mina gruppkollegor var så snabba och vassa, så hann jag bara bidra och lösa en av utmaningarna. Den var kul dock!

## Whisper Chain

    nmap <ip>

Ser att bl.a. port 443 och 5222 (xmpp) är öppna.

## Flagga 1 - TLS-certifikatet

    openssl s_client -connect <ip>>:443

Base64-dekodar cert-blocket och får ut första flaggan, samt en lista på subdomäner:

    murknet.htb
    command.murknet.htb
    groups.murknet.htb
    upload.murknet.htb

Lägger till `murknet.htb` i `/etc/hosts`. Inget nytt att hitta på de övriga subdomänerna i nuläget.

## XMPP-registrering

    nmap --script xmpp-info -p 5222 murknet.htb

Scriptet ger mig info om att servern kräver in-band registration.

Provar att köra Profanity för att titta närmare på XMPP, men det går inte bra direkt. Byter till Pidgin för att skapa ett konto istället, vilket kräver att man fixar certifikatet först:

    openssl s_client -connect murknet.htb:5222 -starttls xmpp -servername murknet.htb </dev/null 2>/dev/null \
      | openssl x509 -outform PEM > ~/.purple/certificates/x509/tls_peers/murknet.htb

Med certet på plats funkar Pidgin och jag kan skapa ett konto. Går tillbaka till Profanity och kör en item discovery, vilket ger mig kanallistan:

    Infrastructure, Random, Resources, Rules

## Läcka i kanalerna

I chattarna hittar jag flera PDF:er, bl.a. onboarding-dokument som pekar mot en VPN.

    pdfinfo Operational_Onboarding_Guide_v3.2.pdf

Ger mig:

    zytglogge88@murknet.htb
    swissclock

## Gamla meddelanden

Letar vidare i Profanity och hittar kommandona `/history on` och `/mam on` för att se äldre meddelanden i kanalerna. Där hittar jag ett gammalt lösenord att prova, vilket visar sig fungera:

    zytglogge88@murknet.htb : TickTock24!

## Flagga 4 - Operation Sparkling

Genom chattarna på det kontot hittar jag en länk:

    https://security.billblog.co.uk/threat/BalanceRAT-analysis-and-attribution

Länken leder ingenstans längre, men via Wayback Machine hittar jag information om "rattlesnake" och spåret leder vidare till:

    https://stonedforums.htb/@porlock

Lösenordet som nämns i rapporten fungerar för att dekryptera kommandon från Operation Sparkling. För att få fram de här måste jag använda mig av Gajim för att kunna skicka in råa XML-förfrågningar. I kommando 4 hittar vi fjärde flaggan:

    429x3WVq1ucARGXx6NEwL4Sg4iowfW5ZWMAqEDErLxrWdg4ffkonB5tNxg85BKGjDqDQRfBERANhgf6DnGjjFyDR5L7uwye

## Flagga 5 - Operation Snatch

Femte flaggan diskuteras i chatten för Operation Snatch:

    dynamite

## Flagga 7 - Ny dekrypteringsnyckel

För att få tag i nästa dekrypteringsnyckel gräver jag djupare i XMPP genom MAM-förfrågningar (Message Archive Management). Det avslöjar konversationer där nyckeln framgår:

    SHALLOWBLUE

Med den nyckeln kan jag dekryptera fler kommandon, vilket ger sjunde flaggan:

    Victoria Station

## Sista flaggan

Av oklar anledning blir jag inbjuden till en chatt av en bot. Genom den konversationen får jag den sista flaggan:

    Operation Dominance, DIOGENES

# Skärmdump på utmaningen efter den var avklarad

![alt text](<Skärmbild 2026-09-20 151924.png>)

# Mitt lag var för snabba och duktiga

![alt text](<Skärmbild 2026-09-20 151901.png>)