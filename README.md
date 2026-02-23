# Enterprise High-Availability Network Design 

## Prezentare Generală
* Acest proiect simulează o infrastructură de rețea enterprise completă, proiectată să susțină operațiunile unei companii medii. Accentul principal este pus pe redundantă (High Availability), scalabilitate și securitate cibernetică la nivel de Layer 2 și Layer 3.


## Arhitectura Rețelei (Design Ierarhic)
Proiectul utilizează modelul ierarhic Cisco (Core-Access) pentru a asigura o gestionare eficientă a traficului:
* Edge Layer: Un router Cisco 4321 care gestionează ieșirea către ISP, NAT și rutarea externă.
* Core Layer: Două switch-uri Multilayer 3650 configurate în mod redundant.
* Access Layer: Switch-uri Layer 2 unde sunt conectate stațiile de lucru, segmentate pe departamente.


## Detalii Tehnice și Implementare
1. Switching & Redundanță L2
* VLAN Segmentation: Rețeaua este divizată în 3 segmente logice: IT (VLAN 10), Sales (VLAN 20) și HR (VLAN 30).
* LACP EtherChannel: Agregarea a două link-uri Gigabit între switch-urile Core pentru a dubla lățimea de bandă și a asigura redundanța link-ului.
* Rapid-PVST+: Optimizarea Spanning Tree prin configurarea Bridge Priority (Root Primary/Secondary) pentru a preveni buclele și a asigura că switch-urile Core controlează topologia.

2. Rutare și Înaltă Disponibilitate (HA)
* HSRP (Hot Standby Router Protocol): Implementarea gateway-ului redundant. Ambele switch-uri Core partajează un Virtual IP (VIP), asigurând continuitatea serviciului chiar dacă unul dintre ele se defectează fizic.
* OSPF (Open Shortest Path First): Protocol de rutare dinamică utilizat pentru a propaga rutele între VLAN-uri și Router-ul de Edge.
* Transit VLAN Design: Utilizarea unui switch de tranzit pentru a conecta routerul la ambele switch-uri Core simultan, eliminând orice Single Point of Failure (SPOF).

3. Securitate și Servicii
* Extended Access Control Lists (ACLs): Politici stricte de filtrare a traficului (ex: Blocarea accesului departamentului HR la serverele IT).
* Port-Security: Porturile de access sunt limitate la o singură adresă MAC folosind tehnologia Sticky, cu acțiune de Shutdown în caz de încălcare.
* NAT/PAT (Overload): Translatarea adreselor private interne într-o adresă publică pentru acces securizat la Internet.
* DHCP Server: Configurat pe switch-urile Core pentru gestionarea automată a adreselor IP.
* SSH v2 Management: Toate echipamentele sunt configurate pentru acces remote securizat (criptat), Telnet fiind dezactivat.


## Teste de Funcționare
* Failover Test: Rețeaua rămâne activă și ping-ul continuă chiar dacă unul dintre switch-urile Core este oprit.
* Security Check: Departamentul HR nu poate accesa rețeaua IT (confirmat prin ACL logs).
* Connectivity: Tracert complet de la orice host până la adresa IP externă (Internet).

##Cum se utilizează
1. Descarcă fișierul `.pkt`.
2. Deschide-l în **Cisco Packet Tracer **.
3. Credențiale SSH: `Username: admin | Password: cisco`.
