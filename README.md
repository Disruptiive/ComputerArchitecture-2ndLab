# 2ο Εργαστήριο Αρχιτεκτονικής Υπολογιστών
##  <p align="center"> Βήμα 1 </p>
1) 
a) Βλέπουμε ότι ο αριθμός των committed instructions (sim_insts) είναι όσες προσδιόρισα με τη σημαία -I 100000000 στα 3 από τα 5 και 100000001 στα άλλα 2. Τα committed instructions δεν είναι ίσα με τα executed instructions πιθανώς γιατί το πρόγραμμα τερματίζει οταν γίνει committed η 100000000η εντολή και κάποιες εντολές δεν προλαβαίνουν να εκτελεστούν.

b) Ψάχνουμε για το ```system.cpu.dcache.replacements``` 

Για το specbzip: 710569  
Για το spechmmer: 65718   
Για το speclibm: 1486955    
Για το specmcf: 54452   
Για το specsjeng: 5262377  

c) Ψάχνουμε για το ```system.l2.overall_accesses::total``` 

Για το specbzip: 712341  
Για το spechmmer: 70563   
Για το speclibm: 1488538    
Για το specmcf: 724390   
Για το specsjeng: 5264051  

Θα μπορούσαμε να υπολογίσουμε περίπου τον αριθμό των accesses της l2 από τον συνολικό αριθμό misses της l1.


2)

i) Ψάχνουμε για το ```sim_seconds``` 

Για το specbzip: 0.083982  
Για το spechmmer: 0.059396   
Για το speclibm: 0.174671    
Για το specmcf: 0.064955   
Για το specsjeng: 0.513528

ii,iii) Ψάχνουμε τα ```system.cpu.dcache.overall_miss_rate::total```, ```system.cpu.icache.overall_miss_rate::total```, ```system.l2.overall_miss_rate::total``` και ```sim_insts``` και υπολογίζουμε το CPI από τον τύπο του προηγούμενου εργαστηρίου ![Formula](https://user-images.githubusercontent.com/43075884/143724729-b66cb8b2-3c21-4102-a2a6-818282ddf972.png)  

|           | CPI   | dcache_misses | icache_misses | l2cache_misses | sim_insts |
|-----------|-------|---------------|---------------|----------------|-----------|
| specbzip  | 1.68  | 0.014798      | 0.00007       | 0.282163       | 100000001 |
| spechmmer | 1.19  | 0.001637      | 0.000221      | 0.077760       | 100000000 |
| speclibm  | 3.49  | 0.060972      | 0.000094      | 0.999944       | 100000000 |
| specmcf   | 1.30  | 0.002108      | 0.023612      | 0.055046       | 100000001 |
| specsjeng | 10.27 | 0.121831      | 0.000020      | 0.999972       | 100000000 |

![all_stats](https://user-images.githubusercontent.com/43075884/146436989-68ec1116-dad1-4412-98b0-e90d8698bb4a.png)

![CPI_SIM_SECS](https://user-images.githubusercontent.com/43075884/146447084-656ad01b-df69-4cc0-a797-d6ec73b3d787.png)

![cpi_misses](https://user-images.githubusercontent.com/43075884/146447837-ba32114b-3329-48b3-b7ef-632e5217d5ab.png)


Από τα γραφήματα φαίνεται ότι το CPI μεταβάλλεται ανάλογα με τα L2cache_misses και DCache_misses κυρίως, ενώ δεν φαίνεται να μεταβάλλεται σε σχέση με το χρόνο εκτέλεσης.

3)
Bλέπουμε ότι το ```system.clk_domain.clock``` παραμένει 1000 πριν και μετά την αλλαγή του cpu clock σε 1.5GHz ενώ το ```system.cpu_clk_domain.clock``` μεταβάλεται μετά την αλλαγή απο 500 σε 667. Επομένως συμπεραίνουμε ότι το ρολόι του συστήματος είναι χρονισμένο στα 1GHz ενώ το ρολόι της CPU χρονίζεται τώρα στα 1.5GHz ενώ η default τιμή του ήταν 2GHz. Αν προσθέσουμε άλλον έναν επεξεργαστή η συχνότητα του θα είναι αυτή που ορίσαμε με τη σημαία --cpu-clock (1.5GHz σε αυτήν την περίπτωση). Δεν υπάρχει τέλειο scaling μεταξύ συχνότητας επεξεργαστή και χρόνο εκτέλεσης, γιατί υπάρχουν άλλα bottlenecks στο σύστημα οπως οι μνήμες.

##  <p align="center"> Βήμα 2 </p>
Μετά απο δοκιμές κατέληξα σε αυτές τις παραμέτρους:
* L1 Associativity:8
* L2 Associativity:8
* L1 Instruction cache: 256kB
* L1 Data cache: 256kB
* L2 Cache: 4MB
* Cache line: 128 bytes

### <p align="center"> Παρατηρήσεις </p>

Όπως φαίνεται στα γραφήματα η αύξηση του associativity δεν μειώνει ιδιαίτερα το CPI, ίσως επειδή έχω επιλέξει πολύ μεγάλες cache και δεν έχω πολλά misses. Για την L1 Cache, βλέπουμε ότι τόσο στην L1 Instruction Cache όσο και στην L1 Data Cache το CPI μειώνεται μέχρι ένα σημείο και μετά παραμένει σχεδόν σταθερό, αφού προσπαθώ όμως να φτιάξω το πιο αποδοτικό σύστημα χρησιμοποιώ όλη τη διαθέσιμη cache μοιράζοντας την εξίσου σε instruction και data cache. Για τον ίδιο λόγο, επιλέγω το μέγιστο της L2 cache που είναι διαθέσιμη, δηλαδή 4MB αν και δεν έχει μεγάλη επίδραση στο CPI μετά από ένα σημείο. Τέλος, παρ'ότι το CPI μειωνόταν σημαντικά όσο αύξανα το cache line ακόμη και στα 512 bytes, επέλεξα τα 128 bytes που φαίνεται ότι είναι η μέγιστη ρεαλιστική τιμή.

![cpi_cache_associativity](https://user-images.githubusercontent.com/43075884/146441738-76eafe0a-7fbe-46fd-9418-418945c01c5b.png)
![cpi_l1d](https://user-images.githubusercontent.com/43075884/146443728-3be5cc38-8383-4870-96a4-a77124526b8f.png)
![cpi_l1i](https://user-images.githubusercontent.com/43075884/146443740-3aa6dec9-2b29-49c6-9236-54553d1fc06b.png)
![cpi_l2](https://user-images.githubusercontent.com/43075884/146444259-6531c25a-99e5-4523-aefa-68e0c6ce904d.png)
![cpi_cacheline](https://user-images.githubusercontent.com/43075884/146445790-77de0723-e5e1-4593-81db-56baa856f4fe.png)

![CPI_before_After](https://user-images.githubusercontent.com/43075884/146448150-50f35297-4c7a-49b4-b6d5-5b680b510c0d.png)

##  <p align="center"> Βήμα 3 </p>

Υπέθεσα ότι όλη η μνήμη L2 κοστίζει όσο όλη η μνήμη L1. Βλέποντας τις cache τιμές ενός Ryzen 3700x επεξεργαστή L1: 512kB L2:4MB, υπολογίζω ότι για κάθε byte πληρώνω 8 φορές περισσότερο την L1 cache σε σχέση με την L2. Υποθέτω ότι το συνολικό κόστος για την αύξηση του associativity είναι το 1/10 από το συνολικό της L2 και L1 καθώς ψάχνοντας βρήκα ότι υπάρχουν και microcontrollers με πολύ μικρότερες cache που χρησιμοποιούν 2 way και 4 way associative.
Καταλήγω επομένως στον τύπο:

C = 0.01 \* L2_cache_bytes + 0.08 \* L1_cache_bytes + 40 \* (L1_I_Associativity + L1_D_Associativity + L2_Associativity)   
(δεν βρήκα κάπου πως επηρεάζεται το κόστος από το μέγεθος της cacheline)

Για το σύστημα του βήματος 2 έχουμε κόστος: C = 81920 μονάδες κόστους, ενώ με τις default παραμέτρους έχουμε C = 12960 μονάδες κόστους. Με βάση το βήμα 2 και υπ'όψιν το κόστος επέλεξα τις εξής παραμέτρους:

* L1 Associativity:2
* L2 Associativity:4
* L1 Instruction cache: 32kB
* L1 Data cache: 8kB
* L2 Cache: 64kB
* Cache line: 128 bytes

Κόστος C =  4160 μονάδες κόστους

![CPI_part3](https://user-images.githubusercontent.com/43075884/146561452-f3232d8e-0113-452f-b6f3-10cba4dc351e.png)

Βλέπουμε ότι με τις νέες παραμέτρους το σύστημα στα benchmarks με μικρό CPI(<2) είναι λίγο πιο αργό απο το default και αυτό του βήματος 2,ενώ στα benchmarks με μεγαλύτερο CPI έχει παραπλήσιες επιδόσεις με αυτό του βήματος 2, ενώ είναι περίπου 3 φορές πιο φθηνό από το σύστημα με τις default παραμέτρους και 20 φορές πιο φθηνό απο το σύστημα του βήματος 2.
