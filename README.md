# 			<div style="text-align:center">Αρχιτεκτονική Υπολογιστών</div>

### 							<div style="text-align:center">Χειμερινό εξάμηνο 2019/2020</div>

### <div style="text-align:center">Τμήμα Ηλεκτρολόγων Μηχανικών και Μηχανικών Υπολογιστών</div>

### 				<div style="text-align:center">Αριστοτέλειο Πανεπιστήμιο *Θεσσαλονίκης*</div>





### 							<u><div style="text-align:center">1η Εργαστητριακή άσκηση</u></div>

##### 													<u><div style="text-align:center">Εισαγωγή στον εξομοιωτή Gem5</u>  </div>





##### Mόρσυ Χρήστος Αδάμ ΑΕΜ:8363

##### Πέννας Παναγιώτης Γεώργιος ΑΕΜ: 9016







Έχοντας εγκατασήσει το λειτουργικό σύστημα Ubuntu  18.04 (Linux Distro) προχωρήσαμε στην εγκατάσταση του εξομοιωτή Gem5 όπως ακριβώς περιγράφηκε στις οδηγίες που μας δόθηκαν. Ακολούθησε η διαδικασία του build για τον επεξεργαστή ARM. 

Τρέξαμε την εντολή που μας δόθηκε ώστε να εκτελεσθεί το precompiled πρόγραμμα "hello" με παραμέτρους από το αρχείο starter_se.py για μοντέλο επεξεργαστή "minor".

```
$ ./build/ARM/gem5.opt configs/example/arm/starter_se.py –cpu=”minor”
“tests/test-progs/hello/bin/arm/linux/hello”
```



Παρακάτω παραθέτουμε τα στοιχεία που συλλέξαμε από την διαδικασία αυτή:



### Ερωτήματα Πρώτου Μέρους:



1. Μελετώντας το αρχείο starter_se.py  εντοπίσαμε και συγκεντρώσαμε τις βασικές παραμέτρους και στατιστικά που πέρασε στον εξομοιωτή gem5 κατά τη διαδικασία της εκτέλεσης του προγράμματος "hello"

   - **<u>CPU</u>:** By default έχει οριστεί ένας πυρήνας, ενώ ο τύπος του επεξεργαστή είναι Minor model CPU.  Σύμφωνα με τα ορίσματα της Minor έχουμε δυο Level-1 cache (μια instruction και μια data), μια Level-2 cache και μια walk cache. 
   - **<u>Συχνότητα</u> <u>Λειτουργίας**</u> : 1GHz
   - **<u>Μήκος γραμμής των cache</u>** : 64 bytes
   - <u>**Μνήμη**</u> : Το μέγεθός της είναι 2GB όπως ορίζεται από το starter_se.py , ο τύπος της SDRAM είναι DDR3_1600_8x8 και  ο αριθμός διαύλων επικοινωνίας είναι δύο. 

2. Για την επαλήθευση των  παραπάνω βασικών χαρακτηριστικών του συστήματος προσομοίωσης μας ζητήθηκε η παράθεση σχετικών πεδίων κώδικα από τα αρχεία config.ini και config.json που παράγει ο Gem5. 

   - <u>**CPU**</u> 

     

     ​		 **config.ini** :

     >   [system.cpu_cluster.cpus]
     >
     > ​	type=MinorCPU

     ​			

     ​		**config.json:**

     > "type": "MinorCPU"

   - <u>**Συχνότητα Λειτουργίας**</u> 

     

     ​		 **config.ini** :

     >  [system.clk_domain]
     >
     > ​	clock=1000

     ​			

     ​		**config.json:**

     > "clock": [
     >                 1000
     >             ]

   - <u>**Μήκος γραμμής των cache**</u> 

     

     ​		 **config.ini** :

     > [system]
     >
     > cache_line_size=64			

     ​		**config.json:**

     > "cache_line_size": 64

   - <u>**Μνήμη**</u> 

     

     ​		 **config.ini** :

     >  [system]
     >
     > mem_ranges=0:2147483647

     

     ​	**config.ini** 

     >[system.mem_ctrlranks_per_channel=2
     
     

     ​			

     ​		**config.json:**

     > "mem_ranges": [
     >
     > ​		"0:2147483647"]



​						**config.json:** 



> ​						 "ranks_per_channel": 2

   3. Στη συνέχεια μας ζητήθηκε να παραθέσουμε μια συνοπτική περιγραφή για κάθε in-order μοντέλο CPU που χρησιμοποιεί ο προσωμοιωτής Gem5. 

   

   

   **SimpleCPU** : είναι λειτουργικά μοντέλα για γρήγορη προσομοίωση χωρίς να εστιάζουν σε όλες τις λεπτομέρειες. Χρησιμοποιούνται κυρίως για απλές δοκιμές, εκτελώντας σειριακά τις εντολές χωρίς instruction pipelining. Δύο μοντέλα αυτών είναι τα Atomic Simple CPU και τα Timing Simple CPU. 

   

   - *Atomic Simple CPU:*  χρησιμοποιεί atomic memory accesses, δηλαδή πιο γρήγορες "επισκέψεις" στη μνήμη από ότι ένα λεπτομερές access. Στο gem5 το AtomicSimpleCPU εκτελεί όλες τις λειτουργίες μιας εντολής σε κάθε tick της CPU και μπορεί να επιστρέψει μια εκτίμηση του συνολικού χρόνου πρόσβασης στην cache χρησιμοποιώντας τα latency από τις atomic accesses. Τέλος παρέχει την ταχύτερη λειτουργική προσωμοίωση.
   - *TimingSimpleCPU* : Σε αντίθεση με το AtomicSimpleCPU χρησιμοποιεί memory access αντί για single atomic. Αυτό σημαίνει ότι περιμένει να γυρίσει το memory access πριν την επεξεργασία και κατά συνέπεια πρόκειται για ένα πιο λεπτομερές μοντέλο.  Για αυτό λέγεται ότι χρησιμοποεί κάποιο επίπεδο χρονισμού. 

   

   **MinorCPU** : Για να προσομοιώσουμε πιο ρεαλιστικά συστήματα χρειαζόμαστε ένα πιο λεπτομερές και ολοκληρωμένο μοντέλο. Το MinorCPU είναι ένα ευέλικτο μοντέλο επεξεργαστή το οποίο χρησιμοποιεί pipeline τεσσάρων σταδίων (fetch1, fetch2, decode και execute). Μπορεί να διαμορφώσει σε επίπεδο μικροαρχιτεκτονικής μοντέλο ενός συγκεκριμένου επεξεργαστή. 

   

   **High performance in-order CPU** : Πρόκειται για ένα ακριβές μοντέλο προσομοίωσης που διαθέτει την ίδια δομή pipeline τεσσάρων σταδίων που έχει και ο minor CPU κι είναι αντιπροσωπευτικό μια σύγχρονης υλοποίησης ARMv8-A. 



​					3a. Στο ερώτημα αυτό μας ζητήθηκε η εκτέλεση ενός απλού προγράμματος σε C και η καταγραφή των       							αποτελεσμάτων που αφορούν τους χρόνους εκτέλεσης.  

Εκτελώντας τον εξής απλό κώδικα :



```c
#include <stdio.h>

int main(){
		for(int i=0; i<6; i++)
	{
		printf("Number:%d\n",i);
   }

return 0;
}
```







Τα αποτελέσματα σε μοντέλο MinorCPU:

| Name of the Parameter         | Corresponding Value | Brief Description                                |
| :---------------------------- | :------------------ | ------------------------------------------------ |
| sim_seconds                   | 0.000038            | # Number of seconds                              |
| sim_ticks                     | 37781000            | # Number of ticks                                |
| sim_freq                      | 1000000000000       | # Frequency of simulated ticks                   |
| host_inst_rate                | 175902              | # Simulator instruction rate (inst/s)            |
| host_op_rate                  | 203764              | # Simulator op (including micro ops) rate (op/s) |
| host_tick_rate                | 508567267           | # Simulator tick rate (ticks/s)                  |
| host_mem_usage                | 669540              | # Number of bytes of host memory used            |
| host_seconds                  | 0.07                | # Real time elapsed on the host                  |
| sim_insts                     | 13050               | # Number of instructions simulated               |
| sim_ops                       | 15134               | # Number of ops (including micro ops) simulated  |
| system.voltage.domain.voltage | 1                   | # Voltage in Volts                               |
| system.clk_domain.clock       | 1000                | # Clock period in ticks                          |



Kαι τα αποτελέσματα σε TimingSimpleCPU:

| Name of the Parameter         | Corresponding Value | Brief Description                                |
| :---------------------------- | :------------------ | ------------------------------------------------ |
| sim_seconds                   | 0.000045            | # Number of seconds                              |
| sim_ticks                     | 45301000            | # Number of ticks                                |
| sim_freq                      | 1000000000000       | # Frequency of simulated ticks                   |
| host_inst_rate                | 363932              | # Simulator instruction rate (inst/s)            |
| host_op_rate                  | 418598              | # Simulator op (including micro ops) rate (op/s) |
| host_tick_rate                | 1266292995          | # Simulator tick rate (ticks/s)                  |
| host_mem_usage                | 664936              | # Number of bytes of host memory used            |
| host_seconds                  | 0.04                | # Real time elapsed on the host                  |
| sim_insts                     | 12977               | # Number of instructions simulated               |
| sim_ops                       | 14967               | # Number of ops (including micro ops) simulated  |
| system.voltage.domain.voltage | 1                   | # Voltage in Volts                               |
| system.clk_domain.clock       | 1000                | # Clock period in ticks                          |





3b. <u>**Συμπέρασμα**</u> :

Όπως ήταν αναμενόμενο  οι χρόνοι εκτέλεσης για κάθε μοντέλο προσομοίωσης της CPU διαφέρουν. Μάλιστα ο MinorCPU ο οποίος  χρησιμοποιεί pipeline 4 σταδίων εκτελεί το απλό πρόγραμμα σε C σε μικρότερο χρόνο καθώς μπορεί να κάνει προσομοίωση multithreading, σε αντίθεση με έναν SimpleCPU προσομοιωτή  όπως αυτός της TimingSimpleCPU που εκτελεί σειριακά εντολές. Η εκτέλεση πιο σύνθετων προγραμμάτων αδηγεί σε μεγαλύτερη διαφορά των χρόνων εκτέλεσης. Οι τιμές που δεν αλλάζουν είναι αυτές που είναι ορισμένες by default.  









3c. Αλλάζοντας τη συχνότητα επεξεργασίας με την εντολή

```
--sys-clock=500000000
```

που κάνει προσομοίωση και για τα δυο μοντέλα της CPU έχουμε τα εξής αποτελέσματα:



Για MinorCPU:

| Name of the Parameter         | Corresponding Value | Brief Description                                |
| :---------------------------- | :------------------ | ------------------------------------------------ |
| sim_seconds                   | 0.000044            | # Number of seconds                              |
| sim_ticks                     | 44184000            | # Number of ticks                                |
| sim_freq                      | 1000000000000       | # Frequency of simulated ticks                   |
| host_inst_rate                | 140637              | # Simulator instruction rate (inst/s)            |
| host_op_rate                  | 162948              | # Simulator op (including micro ops) rate (op/s) |
| host_tick_rate                | 475643765           | # Simulator tick rate (ticks/s)                  |
| host_mem_usage                | 669540              | # Number of bytes of host memory used            |
| host_seconds                  | 0.09                | # Real time elapsed on the host                  |
| sim_insts                     | 13050               | # Number of instructions simulated               |
| sim_ops                       | 15134               | # Number of ops (including micro ops) simulated  |
| system.voltage.domain.voltage | 1                   | # Voltage in Volts                               |
| system.clk_domain.clock       | 2000                | # Clock period in ticks                          |



Όσον αφορά το TimingSimpleCPU:



| Name of the Parameter         | Corresponding Value | Brief Description                                |
| :---------------------------- | :------------------ | ------------------------------------------------ |
| sim_seconds                   | 0.000052            | # Number of seconds                              |
| sim_ticks                     | 51977000            | # Number of ticks                                |
| sim_freq                      | 1000000000000       | # Frequency of simulated ticks                   |
| host_inst_rate                | 575438              | # Simulator instruction rate (inst/s)            |
| host_op_rate                  | 660745              | # Simulator op (including micro ops) rate (op/s) |
| host_tick_rate                | 2292890377          | # Simulator tick rate (ticks/s)                  |
| host_mem_usage                | 664936              | # Number of bytes of host memory used            |
| host_seconds                  | 0.02                | # Real time elapsed on the host                  |
| sim_insts                     | 12977               | # Number of instructions simulated               |
| sim_ops                       | 14967               | # Number of ops (including micro ops) simulated  |
| system.voltage.domain.voltage | 1                   | # Voltage in Volts                               |
| system.clk_domain.clock       | 2000                | # Clock period in ticks                          |



**<u>Συμπέρασμα</u>** :

Η συχνότητα του επεξαργαστή  προσομοίωσης είναι (1.000.000.000.000/2.000)=0,5GHz. Όπως βλέπουμε και για τα δυο μοντέλα επεξεργαστών η λειτουργία σε χαμηλότερη συχνότητα έχει ως αποτέλεσμα  οι δυο χρόνοι εκτέλεσης να είναι μεγαλύτεροι.







Τέλος αλλάζοντα την τεχνολογία της μνήμης με την ακόλουθη εντολή

```
--mem-type='DDR4_2400_8x8' 
```

 παραθέτουμε τα αντίστοιχα αποτελέσματα:



Για MinorCPU:

| Name of the Parameter         | Corresponding Value | Brief Description                                |
| :---------------------------- | :------------------ | ------------------------------------------------ |
| sim_seconds                   | 0.000037            | # Number of seconds                              |
| sim_ticks                     | 36752000            | # Number of ticks                                |
| sim_freq                      | 1000000000000       | # Frequency of simulated ticks                   |
| host_inst_rate                | 165865              | # Simulator instruction rate (inst/s)            |
| host_op_rate                  | 192141              | # Simulator op (including micro ops) rate (op/s) |
| host_tick_rate                | 508567267           | # Simulator tick rate (ticks/s)                  |
| host_mem_usage                | 669540              | # Number of bytes of host memory used            |
| host_seconds                  | 0.08                | # Real time elapsed on the host                  |
| sim_insts                     | 13050               | # Number of instructions simulated               |
| sim_ops                       | 15134               | # Number of ops (including micro ops) simulated  |
| system.voltage.domain.voltage | 1                   | # Voltage in Volts                               |
| system.clk_domain.clock       | 1000                | # Clock period in ticks                          |







Για TimingSimpleCPU:

| Name of the Parameter         | Corresponding Value | Brief Description                                |
| :---------------------------- | :------------------ | ------------------------------------------------ |
| sim_seconds                   | 0.000045            | # Number of seconds                              |
| sim_ticks                     | 44906000            | # Number of ticks                                |
| sim_freq                      | 1000000000000       | # Frequency of simulated ticks                   |
| host_inst_rate                | 591765              | # Simulator instruction rate (inst/s)            |
| host_op_rate                  | 680024              | # Simulator op (including micro ops) rate (op/s) |
| host_tick_rate                | 2038799851          | # Simulator tick rate (ticks/s)                  |
| host_mem_usage                | 664936              | # Number of bytes of host memory used            |
| host_seconds                  | 0.02                | # Real time elapsed on the host                  |
| sim_insts                     | 12977               | # Number of instructions simulated               |
| sim_ops                       | 14967               | # Number of ops (including micro ops) simulated  |
| system.voltage.domain.voltage | 1                   | # Voltage in Volts                               |
| system.clk_domain.clock       | 1000                | # Clock period in ticks                          |





Χρησιμοποιώντας ένα πιο σύγχρονο μοντέλο μνήμης βλέπουμε ότι οι χρόνοι εκτέλεσης μειώνονται.





#### Κριτική Εργασίας:

Η πρώτη μας επαφή με ένα πρόγραμμα προσομοίωσης επεξεργαστή μας φάνηκε ενδιαφέρουσα. Με την εκτέλεση της gem5 είχαμε την ευκαιρία εκτός απο την ενασχόλησή μας με στοιχεία της αρχιτεκτονικής υπολογιστών να γνωρίσουμε τα χαρακτηριστικά που μελετάμε σε κάθε εκτέλεση προγράμματος. Παράλληλα ήρθαμε σε επαφή με το λειτουργικό σύστημα Ubuntu και την εκτέλεση εντολών μέσω Terminal. Η προσωπική ενασχόληση και αναζήτηση αρκετών πληροφοριών μας βοήθησε να βρούμε ενάν μεγάλο όγκο πληροφοριών σχετικά με το κομμάτι hardware των υπολογιστών. Τέλος, η επαφή με τη γλώσσα Markdown και το github είναι αρκετά σημαντική τόσο για τους σκοπούς του μαθήματος όσο και για μελλοντική χρήση τους.



#### Βιβλιογραφία:

- Arm Research Starter Kit: System Modeling  using gem5  -- Ashkan Tousi and Chuan Zhu
  July 2017

- http://www.gem5.org
- https://www.freecodecamp.org/news/the-essential-git-handbook-a1cf77ed11b5/
- https://www.markdowntutorial.com/
- https://stackoverflow.com/



