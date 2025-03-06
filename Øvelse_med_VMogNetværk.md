# Netværk med VMware Workstation - Guidede Øvelser

## Metadata for læringen
**Relevans:** Netværk og virtualisering er fundamentale komponenter i IT-infrastruktur. At kunne opsætte og forstå netværk i et virtualiseret miljø er en vigtig færdighed i både systemadministration, DevOps og softwareudvikling.

**Læringsmål:**
- Forstå, hvordan virtuelle maskiner kan forbindes i et netværk.
- Opsætte og konfigurere netværksforbindelser mellem VM'er i VMware Workstation.
- Fejlsøge netværksforbindelser i en virtualiseret kontekst.

**Forventet udbytte:** Efter øvelsen vil I have praktisk erfaring med at oprette og administrere netværk mellem virtuelle maskiner i VMware Workstation.

---

## Øvelse 1: Oprettelse af virtuelle maskiner
**Mål:** Opret to virtuelle maskiner i VMware Workstation.

### Trin-for-trin:
1. Åbn VMware Workstation.
2. Klik på **Create a New Virtual Machine**.
3. Vælg **Typical (recommended)** og klik **Next**.
4. Vælg **Installer disk image file (ISO)** og vælg en ISO-fil (fx Ubuntu eller Windows Server).
5. Angiv navn og placering for VM’en.
6. Tildel **RAM** (min. 2GB) og **CPU-kerner** (min. 2).
7. Opret en virtuel harddisk (min. 20GB anbefales).
8. Klik **Finish** og gentag processen for at oprette en anden VM.
9. Start begge VM'er og installer OS efter behov.

---

## Øvelse 2: Forstå VMware's netværkstyper
**Mål:** Forstå forskellen på de tre netværkstyper i VMware Workstation.

### Netværkstyper:
- **Bridged**: VM’en får en IP fra det fysiske netværk og opfører sig som en fysisk maskine.
- **NAT**: VM’en deler værtens IP-adresse og har adgang til internettet via NAT.
- **Host-Only**: VM’er kan kun kommunikere med hinanden og værten, men ikke internettet.

### Opgave:
1. Åbn VMware Workstation.
2. Gå til **Edit** → **Virtual Network Editor**.
3. Undersøg og noter forskellene mellem de tre netværkstyper.

---

## Øvelse 3: Opsætning af et internt netværk (Host-Only)
**Mål:** Opret et netværk mellem to VM’er uden internetadgang.

### Trin-for-trin:
1. Åbn **Edit** → **Virtual Network Editor** i VMware Workstation.
2. Vælg **VMnet1 (Host-Only)** og klik **Apply**.
3. Åbn indstillinger for hver VM:
   - Gå til **VM Settings** → **Network Adapter**.
   - Vælg **Host-Only** og klik **OK**.
4. Start begge VM'er og åbn terminal/kommandoprompt.
5. Kør `ipconfig` (Windows) eller `ip a` (Linux) for at se IP-adressen.
6. Test forbindelsen ved at pinge den anden VM:
   ```sh
   ping <IP-adresse>
   ```

---

## Øvelse 4: Opsætning af et netværk med internetadgang (NAT)
**Mål:** Opret et netværk, hvor VM’er har internetadgang via værtsmaskinen.

### Trin-for-trin:
1. Gå til **Edit** → **Virtual Network Editor**.
2. Vælg **VMnet8 (NAT)** og klik **Apply**.
3. Åbn VM’ens indstillinger → **Network Adapter**.
4. Vælg **NAT** og klik **OK**.
5. Start VM’en og test internetforbindelsen:
   ```sh
   ping 8.8.8.8
   ```
6. Hvis ping virker, men DNS ikke gør, tilføj Google DNS:
   - Windows: `ncpa.cpl` → **Change adapter settings** → **Properties** → **IPv4** → **Use the following DNS** → 8.8.8.8
   - Linux: Rediger `/etc/resolv.conf` og tilføj:
     ```sh
     nameserver 8.8.8.8
     ```

---

## Øvelse 5: Fejlsøgning af netværksforbindelser
**Mål:** Identificér og løs netværksproblemer mellem VM’er.

### Opgave:
1. Brug **`ipconfig` (Windows) / `ip a` (Linux)** til at kontrollere IP-adresser.
2. Brug `ping` for at teste forbindelser mellem VM'er.
3. Tjek firewall-indstillinger på VM’er:
   ```sh
   sudo ufw status  # Linux
   netsh advfirewall show allprofiles  # Windows
   ```
4. Deaktiver firewallen midlertidigt for at teste (ikke anbefalet i produktionsmiljøer):
   ```sh
   sudo ufw disable  # Linux
   netsh advfirewall set allprofiles state off  # Windows
   ```
5. Genstart netværkstjenester:
   ```sh
   sudo systemctl restart networking  # Linux
   ipconfig /renew  # Windows
   ```
6. Hvis problemet fortsætter, tjek VMware's netværksindstillinger i **Virtual Network Editor**.

---

## Afsluttende Refleksion
**Spørgsmål til diskussion:**
1. Hvilken netværkstype vil du vælge til en testserver, der skal være tilgængelig fra værtsmaskinen?
2. Hvordan kan du sikre, at VM'er kun kommunikerer internt uden adgang til internettet?
3. Hvilke værktøjer vil du bruge til at fejlsøge netværksproblemer?

---

Disse øvelser giver jer en solid praktisk forståelse af netværk i VMware Workstation. Held og lykke!