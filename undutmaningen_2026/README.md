# Undutmaningen

2026-03-21 // 12:00-20:00

Lag: Snöleopard

Okända lagkamrater.


    ├──[-] forensik
    │   ├── Knappast lätt (500)
    │   ├── Sharklocker (500)
    │   ├── Vågor under vattnet (500)
    │   └── Kapten Sträng (500)
    │
    ├──[-] rev
    │   ├── Skattkistan (500)
    │   ├── Manifest deepthingy (500)
    │   └── Ett till pajat fönster (500)
    │
    ├──[-] web
    │   ├── Recycling (500)
    │   ├── Fråga fisken (500)
    │   ├── Undervattensgalleriet (500)
    │   └── Code Control (500)
    │
    ├──[-] misc
    │   ├── Back off man! I'm a scientist. (500)
    │   ├── Åtkomst nekad (500)
    │   ├── echo (500)
    │   └── På botten (500)
    │
    ├──[-] programmering
    │   ├── Deep Blue Sea (500)
    │   └── Biologiskt mångfald (500)
    │
    ├──[-] pwn
    │   ├── Flytväst (500)
    │   └── GIF Splitter (500)
    │
    ├──[-] OSINT
    │   ├── Grundare (500)
    │   └── Flaskpost från kusten (500)
    │
    └──[-] krypto
        ├── Undervattensströmmar (500)
        ├── Infekterad BIOS (500)
        └── AquaCrypt (500)

# Recycling

Jag hittade http-trafiken som verkade vara relevant för hemsidan men jag hittade aldrig en POST-request för att faktiskt logga in. Det tog mig en stund av stirrande innan jag förstod att jag skulle knycka kakan. Men till slut så kom jag på det.

![Packets](<Recycled - Skärmbild 2026-03-21 131319.png>)

    GET /static/js/chart.my.js HTTP/1.1
    Host: x32.cascada.ex:8008
    User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
    Accept: */*
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate
    Connection: keep-alive
    Referer: http://x32.cascada.ex:8008/stats
    Cookie: SECRET=token_wY0aHRM6gqVjEZF1yC92nKxL
    Priority: u=2


    HTTP/1.1 200 OK
    Server: Werkzeug/3.1.3 Python/3.11.14
    Date: Fri, 24 Oct 2025 13:28:30 GMT
    Content-Disposition: inline; filename=chart.my.js
    Content-Type: application/javascript; charset=utf-8
    Content-Length: 206944
    Last-Modified: Wed, 21 May 2025 08:02:25 GMT
    Cache-Control: no-cache
    ETag: "1747814545.77397-206944-2146306452"
    Date: Fri, 24 Oct 2025 13:28:30 GMT
    Connection: close

![Devtools](<Recycled - Skärmbild 2026-03-21 131144.png>)

Fixade cookien från paket 2710 in i DevTools i min browser och sen kunde jag access:a /secret där nyckeln låg!

# Kapten Sträng

![Wireshark](<Kapten Sträng - Skärmbild 2026-03-21 141916.png>)

Sparade konversationen som KaptenRaw, letade rätt på var rar-filen började med:

    xxd KaptenRaw | grep "5261 7221"

Svaret gav mig 00000ba0, vilket blir 2976. Fortsätter med:

    dd if=KaptenRaw of=output.rar bs=1 skip=2976

I pcap-filen så körs följande kommando:

    "rar a -pqazxswedc123 exfil.rar TO_HQ FROM_HQ"

Alltså är lösenordet qazxswedc123, vilket gör att jag kan unrar:a filen, där i ligger det ett par filer, bl.a. OPTI_CAM_01_OCRSCAN_000001.png, när jag kör exiftools så får vi se en av raderna säger

![Exiftools](<Kapten Sträng - Skärmbild 2026-03-21 142314.png>)

Så där hittade jag flaggan: undut{periskopdjup_-8m}

# Sharklocker

Den här tog mig lång tid, det var en flagga som inte var värd mer poäng men för mig kändes det som flera utmaningar i en!

Såhär ser det ut efter man sökt runt och stökat ner en hel del.

![alt text](<Sharklocker - Skärmbild 2026-03-21 170710.png>)

**Här nedan är ungefärlig ordning jag hittade delarna för Recovery Key**

/C/Users/user/Documents/document.pdf
 
 cat document.pdf

    8->406120

/C/Users/user/Documents/Part-6.pdf

Highlightade texten, var vit text mot vit bakgrund.
 
    Part 6: 199694

 /C/Users/user/Downloads/Part4.7z
 
cat ToDo.txt

    hash 7s
    decrypt

Så jag provar det

    7z2john part4.7z > hash.hash 
    john --wordlist=rockyou-65.txt hash.hash --format=7z

cat part4.txt 

    308396


C/Users/User/Music/'P[A]rt$_Warmup^.mp3'

Spelade upp ljudet baklänges så pratar någon på "norska" och i slutet säger den följande siffror på engelska:

    244310

C/Users/User/Pictures/part-7.jpg

Man såg siffran i thumbnailen när man tittade på bilden.

    411587


C/Users/user/AppData/Local/Google/Chrome/User Data/Default/History

    sqlite3 History
    SELECT url, title FROM urls ORDER BY last_visit_time DESC;

    219769

/C/Users/user/Documents/Hajmat.docx

Den här behövde jag återkomma till för att upptäcka. Till slut såg jag numret och "Part 2" i dokumentinformationen i Libreoffice.

    092587

**Recovery Key**:

674806-092587-219769-308396-244310-199694-411587-406120


För att få kolla in vad som finns inuti vhd-filen så tog jag hjälp av AI för att kunna mounta och låsa upp den då jag aldrig gjort något sånt förut. Tyvärr antecknade jag inte i realtid hela tiden, så nedan är ett axplock av det jag tror jag använde mig av för att lösa disken på slutet.

    sudo modprobe nbd
    sudo qemu-nbd --connect=/dev/nbd0 SecretShark.vhd
    sudo mount /dev/nbd0p1 /mnt/SecretShark
    sudo dislocker -V /dev/nbd0p1 -p674806-092587-219769-308396-244310-199694-411587-406120 -- /mnt/SecretShark
    sudo mkdir -p /mnt/vhd
    sudo mount -o loop /mnt/SecretShark/dislocker-file /mnt/vhd

![Flaggan](<Sharklocker - Skärmbild 2026-03-21 170433.png>)


# Aquacrypt

Ej avslutat - blev för avancerat för mig. AI kunde inte ens hjälpa mig hela vägen.

Hittade robots.txt som pekade mig mot /admin, där kunde jag ladda hem ett app.py-script. I den filen fanns följande hash:

    H4sIAAAAAAACA+2dO3brMAxE+6zCXZrsKCf730bq96rYJoH5XNeShqLBwWD40ffnY+D3+fUYwZkD+hPSXGsKOnwNVSumJMCXGhr6ty905wjiVjivDaNwMnwWSDJFhmfGaNhESn4K6L1W/Xv3qdE5eY3vKNkYlguYBeWAZFgn6zmDjP3qnTByGW1I5p2hRnXlhqasXzCagoM3PUjNGCCua1b7P7TS8yH5nhSIgYGG8eBFPHCsDYaTQz+WEBK6iUr8AiKKjNAKJIroJBSYDAZworkgkVmbZgaS5sDD1UVe5Kd7HWKyQdhIKZBNDZismSGvkzAsCPMcyPxV7TfcDdmDsX90GJ0fk1tJ4xq7iEWScBQs/PWKAxRCfVG3qsZsjg46BX3lFbSGoCWLqUCKaxTFsWlreB55ojahKElA3Z4STC75nbUK7bof5U67JeNCmHQyUqOVl7EA+Mqj7zXHq2dFy7WChfx3UYM3IJdtfnP9TwuKtmMPfNPIZDZN3354MG9lVY/MkyHzVtwwcxUrvKK0/hy4fxixcNEamIWLS+BCCxdxL4JGDRwJR2bqpuB+yTWXg/YvX+Xisu9BSB/c+07Tbv3ll4/gRzXYvx5bvUn2YsBhA63K3bxWc2J5nr6G0QTs9LslhUmsUgs98fA0CMWP++thmfoWN4EBGeypkStRa5q4QwXrkRe9LJO2X+/iLee8eVwFjbGbxvCsgSSlOQHVLlZbwjTZ70ayJHlTOhk5MHyfGlsFkVgg2KZ8D+2O4GuIp+/Ms+7negJqChcD7uvLbf/m6FkSvidKJXQX6NlW6ZgLFCukagRCTDfK7YVDxbRkkipZwspLCXDZfmFYMiwZllttZE56UZFUWSFMWuID6L6gtMtN5/vCsgXWIiTY1iNYkJFp0YBGqDjuxx6A467Y0VgHejlyHKz6mGdt9UaiQ5YIg6OoN3Am+JqcUJ8TWqogzMrkMPE6WkV06YSunLrdMgrkhGKcMooyijKKMmrh8ddf/RhXyp7As8kHU+1F+Il2BimdlE5KJ6WT08npV2/iK99cQ67IUB0tIms2tiOPMuk6ZAySF4oG82OZiSx9iTEOHX0qsZRgi6MqljzJsgJmL3WIMDhm7waODF/nfpyAiTi4GW4mXAdwsgo3GWtlpZVuk3vaT8LnSr9GmXZ0DWCi6vQ19ESTRqmiMStmo+x4FxFTAlMCUwJTQp6vH+yxgEVhUVg0ePxh7XrVH+y3xDZpv4as4aM/2G1xo1XstrAf+5C8mTANnZsjxhAkCBIECdFMDImB4yF64Bh5iA2qkmUNK4gc/Q2s4rv5Hv2tr38jl6lBLBCLcJjUGkQZO3d6+9KznMUtls7VHRSEW+xnVRQpoQox+18i+Pn4BSrBPxuYLwEA

Efter jag avkodat det och tagit mig en titt i CyberChef så inser jag att jag kan gunzippa det här.
Här fick Claude hjälpa mig fixa scriptet för att dekoda filen jag gunzippat.

![Aquacrypt](<Aquacrypt - Skärmbild 2026-03-21 195029.png>)

Här frågade jag Claude om hjälp för att förstå, men det devolverade i en halvtimmes prat med en AI om hur vi ska lyckas fixa signaturen. Det löste sig inte tyvärr.

Men jag är nöjd med min prestation. Jag förväntade mig inte att ta tre flaggor på egen hand.

![Stats](<Stats - Skärmbild 2026-03-21 201616.png>)