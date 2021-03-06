# Αναβάθμιση έκδοσης υπάρχοντος λογισμικού για το Ubuntu

Όπως αναφέρθηκε νωρίτερα, σχετικά με την αναβάθμιση υπάρχοντος για το Ubuntu πακέτου, πρόκειται για την απλούστερη περίπτωση στο packaging. Επειδή η διαδικασία θα απαιτήσει την λήψη και επεξεργασία αρχείων, καλό θα είναι να γίνει σε έναν φάκελο αποκλειστικά για αυτό το σκοπό, για να έχουμε τα αρχεία μας συμμαζεμένα. Δημιουργήστε, λοιπόν, ένα φάκελο όπου σας διευκολύνει (μέσα στο «**Home**» ή στους υποφακέλους του), ανοίξτε το τερματικό σας και κάντε «**cd**» σε αυτόν τον φάκελο.


> Την στιγμή που συντάσσεται το παρόν κείμενο, η τρέχουσα έκδοση του Ubuntu είναι η **16.04 LTS (Xenial Xerus)**, η οποία περιέχει στα επίσημα αποθετήρια της τα συστατικά του *περιβάλλοντος εργασίας GNOME*, *έκδοσης 3.18 ή παλαιότερης*. Το **GNOME Project**, όμως, την ίδια στιγμή, έχει τα συστατικά του στην *έκδοση 3.20*. Επειδή οι οδηγίες που ακολουθούν για την διαδικασία του packaging είναι γενικές και εμπλέκονται πολλά αρχεία, παράλληλα, σε κάθε βήμα θα υπάρχουν *screenshots* με την εφαρμογή της διαδικασίας στο *πακέτο-πειραματόζωο* «**gnome-disk-utility**», το οποίο είναι ένα από τα συστατικά του GNOME και αυτή τη στιγμή βρίσκεται στην έκδοση **3.18.1**. Θα χρησιμοποιηθεί επίσης και η εντολή «**ls**» για να βλέπουμε τα αρχεία που δημιουργούνται σε κάθε βήμα. Ας δοκιμάσουμε, λοιπόν, να αναβαθμίσουμε το «**gnome-disk-utility**» στην τελευταία του upstream έκδοση.
![](https://cerebrux.files.wordpress.com/2016/04/gdu3-18.png?w=680)


## • Βήμα 1:Λήψη πακέτου

Για να αναβαθμίσουμε ένα λογισμικό το οποίο υπάρχει ήδη στο Ubuntu, πρέπει πρώτα να κατεβάσουμε την υπάρχουσα έκδοση. Ο λόγος δεν είναι άλλος από την διατήρηση του φακέλου «**debian**», ο οποίος περιέχει ήδη *Ubuntu-specific* παραμέτρους στο λογισμικό (patches για διορθωμένα bugs και άλλα), οι οποίες πρέπει να διατηρηθούν ώστε να μην χρειαστεί να ξανακάνουμε δουλειά ή οποία έχει ήδη γίνει από άλλους packagers. Για να κατεβάσουμε την υπάρχουσα έκδοση ενός λογισμικού, εκτελούμε στο τερματικό:
```bash
pull-lp-source ΟΝΟΜΑ_ΠΑΚΕΤΟΥ```
Με την εκτέλεση της παραπάνω εντολής, στον φάκελό μας θα κατέβουν τα εξής στοιχεία:

* Ο φάκελος «**όνομα_πακέτου-'έκδοση(Upstream)'**», ο οποίος περιέχει τον πηγαίο κώδικα του λογισμικού της υπάρχουσας έκδοσης, μαζί με τον κατάλογο «**debian**».
* Το tarball «**όνομα_πακέτου-'έκδοση(Upstream)'.orig.tar**», το οποίο περιέχει τον πηγαίο κώδικα του λογισμικού της υπάρχουσας έκδοσης όπως αυτός είναι upstream.
* Το tarball «**όνομα_πακέτου-'έκδοση(Upstream)-έκδοση(Ubuntu)'.tar**», το οποίο περιέχει τον πηγαίο κώδικα του λογισμικού της υπάρχουσας έκδοσης, μαζί με τον κατάλογο «**debian**».
* Η εικόνα «**όνομα_πακέτου-'έκδοση(Upstream)-έκδοση(Ubuntu)'.dsc**».

Και με τη λήψη του φακέλου «**όνομα_πακέτου-'έκδοση(Upstream)'**» αυτόματα εφαρμόζονται στον πηγαίο κώδικα τα τυχόντα υπάρχοντα *patches* και οι *Ubuntu-specific* αλλαγές, όπως αυτές ισχύουν στο υπάρχον πακέτο.
![](https://cerebrux.files.wordpress.com/2016/04/pulllpsource.png?w=680)


## • Βήμα 2: Έλεγχος και λήψη τελευταίας έκδοσης

Στην συνέχεια, πρέπει να ελέγξουμε αν έχει βγει νεότερη έκδοση του λογισμικού από την ομάδα ανάπτυξής του (*upstream*) και να την κατεβάσουμε. Μπαίνουμε πρώτα στον φάκελο με τον πηγαίο κώδικα που κατεβάσαμε πριν:
```bash
cd όνομα_πακέτου-'έκδοση(Upstream)'```
Και ελέγχουμε αν υπάρχει νεότερη έκδοση εκτελώντας την ακόλουθη εντολή, η οποία εφόσον υπάρχει νεότερη έκδοση, θα την κατεβάσει κιόλας:
```bash
uscan```
Όπου μετά την εκτέλεσή της, στον αρχικό φάκελό μας, εκτός των 4 στοιχείων που κατέβηκαν στο **βήμα 1**, θα έχουμε επιπλέον δύο αρχεία:

* Το tarball «**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)'.orig.tar**», το οποίο περιέχει τον πηγαίο κώδικα της τελευταίας upstream έκδοσης, και αμέσως μετά, θα του προσθέσουμε και τον κατάλογο «**debian**».
* Το tarball «**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)'.tar**».

> Πληροφοριακά, ο τρόπος που η παραπάνω εντολή βρίσκει την τελευταία έκδοση του λογισμικού, είναι μέσω του αρχείου «**debian/watch**».

![](https://cerebrux.files.wordpress.com/2016/04/uscan.png?w=680)

## • Βήμα 3: Ενημέρωση της τελευταίας έκδοσης με τον κατάλογο «debian»

Αυτό που θέλουμε να κάνουμε τώρα, είναι στην τελευταία έκδοση του λογισμικού που κατέβηκε εκτελώντας την εντολή στο προηγούμενο βήμα, να προσθέσουμε τον κατάλογο «**debian**» από την υπάρχουσα έκδοση, ώστε να περάσουμε τις Ubuntu-specific αλλαγές στην νεότερη έκδοση. Αυτό γίνεται εκτελώντας:
```bash
uupdate ../ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)'.orig.tar```
Έτσι, ο κατάλογος «**debian**» από την προηγούμενη έκδοση θα περάσει στην νεότερη. Εδώ, προσοχή, θα πρέπει να βάλετε **ακριβώς** το όνομα του αρχείο μαζί με την κατάληξή του (*tar.xz* ή οτιδήποτε). Μετά από αυτή την ενέργεια θα έχουν δημιουργηθεί:

* Ο φάκελος «**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)'**» με την νεότερη έκδοση του λογισμικού, όπου θα περιέχει τον κατάλογο «debian»
* Ο φάκελος «**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)'.orig**» με την νεότερη έκδοση του λογισμικού, όπως αυτή είναι upstream.

![](https://cerebrux.files.wordpress.com/2016/04/uupdate.png?w=680)

## • Βήμα 4: Έλεγχος εξαρτήσεων

Συνεχίζουμε ελέγχοντας τις **εξαρτήσεις** του λογισμικού. Οι *εξαρτήσεις* είναι άλλα λογισμικά στα οποία βασίζεται το λογισμικό που αναβαθμίζουμε, ώστε να λειτουργεί σωστά. Συνήθως, όταν δημιουργηθεί μία νέα έκδοση ενός λογισμικού, οι εξαρτήσεις αυτού (άλλα λογισμικά) έχουν αλλάξει. Συγκεκριμένα, ή προστέθηκαν νέες εξαρτήσεις, ή αφαιρέθηκαν, ή όπως πιο συχνά συμβαίνει, οι υπάρχουσες εξαρτήσεις χρειάζεται να είναι σε νεότερη έκδοσή τους. Γενικότερα, για να δούμε τι αλλαγές έχουν γίνει στις εξαρτήσεις του πακέτου που αναβαθμίζουμε θα πρέπει να ελέγξουμε το αρχείο «**configure.ac**», το οποίο βρίσκεται στον βασικό φάκελο του λογισμικού upstream. Συνεπώς, προκειμένου να το κάνουμε αυτό, πρέπει να κατεβάσουμε τον φάκελο του λογισμικού στον υπολογιστή. Αυτό το κάνουμε με το εργαλείο «**git**» (αφού πρώτα πλοηγηθούμε ξανά, πίσω στον αρχικό φάκελο όπου δουλεύουμε), εκτελώντας:
```bash
cd .. && git clone ΣΥΝΔΕΣΜΟΣ_ΛΟΓΙΣΜΙΚΟΥ```
Για παράδειγμα, τα εργαλεία του **GNOME Project** βρίσκονται σε σύνδεσμο του τύπου:
```bash
http://git.gnome.org/browse/PACKAGE```

Αφού εκτελέσουμε την εντολή, στον αρχικό φάκελο θα έχει προστεθεί ακόμα ένας φάκελος:

* Ο φάκελος «**ΟΝΟΜΑ_ΛΟΓΙΣΜΙΚΟΥ**», ο οποίος θα έχει το όνομα του λογισμικού για το οποίο δουλεύουμε (χωρίς κάποιον αριθμό έκδοσης).

Μπαίνουμε, λοιπόν, σε αυτό το φάκελο και ανοίγουμε το αρχείο «**configure.ac**», εκτελώντας:
```bash
cd ΦΑΚΕΛΟΣ_ΛΟΓΙΣΜΙΚΟΥ(ΧΩΡΙΣ ΕΚΔΟΣΗ)
git log -p configure.ac```
Εδώ, στο τερματικό μας θα δούμε το **log**, δηλαδή τις αλλαγές που έχουν γίνει στο λογισμικό, κυρίως αλλαγές στις εξαρτήσεις, που έχουν γίνει από έκδοση σε έκδοση. Θα υπάρχουν γραμμές τονισμένες με τον εξής τρόπο:
```bash
-ΚΑΤΙ ΠΟΥ ΑΦΑΙΡΕΘΗΚΕ
+ΚΑΤΙ ΠΟΥ ΠΡΟΣΤΕΘΗΚΕ```
Οπότε, όταν έχει γίνει προσθήκη κάποιας εξάρτησης, θα δούμε στο log του αρχείου κάτι τέτοιο:
```bash
+ΟΝΟΜΑ-ΕΞΑΡΤΗΣΗΣ_REQUIRED="όνομα_πακέτου >= αριθμός_έκδοσης_εξάρτησης"```
Αντίστοιχα, αν κάποια εξάρτηση έχει αφαιρεθεί:
```bash
-ΟΝΟΜΑ-ΕΞΑΡΤΗΣΗΣ_REQUIRED="όνομα_πακέτου >= αριθμός_έκδοσης_εξάρτησης"```
Ενώ, αν απλά έχει αναβαθμιστεί η έκδοση υπάρχουσας εξάρτησης (η συνηθέστερη περίπτωση):
```bash
-ΟΝΟΜΑ-ΕΞΑΡΤΗΣΗΣ_REQUIRED="όνομα_πακέτου >= αριθμός_έκδοσης_εξάρτησης"
+ΟΝΟΜΑ-ΕΞΑΡΤΗΣΗΣ_REQUIRED="όνομα_πακέτου >= αριθμός_νέας_έκδοσης_εξάρτησης"```
Οπτικά, αυτή η δομή της επισήμανσης των αλλαγών αποτελεί διευκόλυνση για έναν packager, διότι βλέπουμε με **κόκκινο χρώμα και το σύμβολο «-»** στην αρχή, τις γραμμές που έχουν αφαιρεθεί, ενώ με **πράσινο χρώμα και το σύμβολο «+»**, τις γραμμές που προστέθηκαν. Αφού δούμε τι αλλαγές έχουν γίνει (scrollάρουμε στο log στο τερματικό με τα βελάκια του πληκτρολογίου), αν έχουν γίνει (διότι στο παράδειγμα δεν έχει γίνει καμία αλλαγή στις εξαρτήσεις. Τυχαίνει.), κλείνουμε το log (με το πλήκτρο **q**).
![](https://cerebrux.files.wordpress.com/2016/04/git.png?w=735)
![](https://cerebrux.files.wordpress.com/2016/04/log.png?w=735)


## • Βήμα 5: Προσθήκη εξαρτήσεων



Αφού, λοιπόν, δούμε τις εν λόγω αλλαγές στις εξαρτήσεις του λογισμικού, πρέπει να πάμε να τις ορίσουμε στο πακέτο που θα δημιουργηθεί για το Ubuntu. Αυτό, γίνεται με επεξεργασία του αρχείου «**debian/control.in**», το οποίο βρίσκεται στον φάκελο της νεότερης έκδοσης του λογισμικού (**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)'**). Είτε μέσω κάποιου κειμενογράφου στο τερματικό, είτε με οποιονδήποτε κειμενογράφο, ανοίγουμε το αρχείο «**debian/control.in**» και εκτός διαφόρων πληροφοριών που υπάρχουν σε αυτό, θα δούμε και την κατηγορία «**Build-Depends:**», στην οποία θα υπάρχει η λίστα των εξαρτήσεων του λογισμικού και η έκδοση αυτών. Η λίστα αυτή θα έχει τη μορφή:
```bash
Build-Depends: εξάρτηση_1 (>= έκδοση),
εξάρτηση_2 (>= έκδοση),
εξάρτηση_3 (>= έκδοση),
εξάρτηση_4 (>= έκδοση),
...
εξάρτηση_ν (>= έκδοση)```
Σε αυτή τη λίστα θα κάνουμε τις κατάλληλες αλλαγές στις εξαρτήσεις του λογισμικού, είτε πρόκειται για αλλαγή στον αριθμό έκδοσης της εξάρτησης, είτε προσθήκη νέας εξάρτησης, είτε αφαίρεση παλιάς. Απλώς, προσθαφαιρούμε στη λίστα τις αλλαγές που παρατηρήσαμε στο προηγούμενο βήμα με την ανάγνωση του αρχείου «*configure.ac*». Αποθηκεύουμε το αρχείο, αφού κάνουμε όλες τις απαραίτητες αλλαγές.


> **Σε αυτό το σημείο, να τονιστεί κάτι σημαντικό που αφορά τον εκάστοτε packager:** Οι εξαρτήσεις ενός πακέτου εγκαθίστανται αυτόματα αφού πρώτα ληφθούν από τα αποθετήρια της διανομής. Το γεγονός πως η νέα έκδοση του λογισμικού που θέλουμε να αναβαθμίσουμε μπορεί να χρησιμοποιεί νεότερες εκδόσεις εξαρτήσεων, δεν σημαίνει πως αυτές οι εκδόσεις των εξαρτήσεων υπάρχουν στα αποθετήρια (μπορεί αυτές στα αποθετήρια να είναι παλιότερες), πράγμα που αν ισχύει, θα αφήσει τις εν λόγω εξαρτήσεις ανικανοποίητες και δεν θα γίνει η εγκατάσταση της νέας έκδοσης του λογισμικού. Σε αυτή την περίπτωση, πρέπει να πακετάρουμε και την ίδια την εξάρτηση, με τον ίδιο ακριβώς τρόπο που αναγράφεται στην παρούσα ενότητα. Όπως καταλαβαίνετε, η ίδια η εξάρτηση μπορεί να έχει δικές τις εξαρτήσεις, και η νέα έκδοσή της να απαιτεί νεότερες εκδόσεις των εξαρτήσεων της οι οποίες μπορεί να μην βρίσκονται στα αποθετήρια. Βλέπουμε πως μία απλή αναβάθμιση ενός πακέτου, μπορεί να δημιουργήσει μία αλυσίδα ελέγχου και αναβαθμίσεων άλλων πακέτων-εξαρτήσεων προκειμένου να επιτευχθεί, πράγμα που μας καλωσορίζει στην σκοτεινή πλευρά του packaging. Ελέγχουμε ποια πακέτα (και ποιες εκδόσεις αυτών) υπάρχουν στα επίσημα αποθετήρια του Ubuntu είτε μέσω του τερματικού μας, είτε [μέσω αυτού του συνδέσμου](http://packages.ubuntu.com/).

> 
Επίσης, ενδέχεται να υπάρχουν δύο αρχεία, το «**control**» και το «**control.in**», ή μόνο ένα από τα δύο. Αν υπάρχουν και τα δύο, τότε αυτό που επεξεργαζόμαστε για να κάνουμε τις αναγκαίες αλλαγές, είναι το «**control.in**».

![](https://cerebrux.files.wordpress.com/2016/04/control.png?w=680)


## • Βήμα 6: Εφαρμογή patches



Στο **βήμα 3**, περάσαμε τον κατάλογο «**debian**» από την προηγούμενη έκδοση στην νεότερη. Θα πρέπει όμως να εφαρμόσουμε όποια **patches** υπάρχουν από την παλιότερη έκδοση, στη νέα. Έτσι, γυρνάμε στον αρχικό μας φάκελο (όπου δουλεύουμε) και στην συνέχεια μπαίνουμε σε αυτόν της νεότερης έκδοσης:
```bash 
cd ../ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)'```
Και εφαρμόζουμε τα patches εκτελώντας:
```bash
quilt push -a```
Το «**quilt**» είναι ένα εργαλείο διαχείρισης και διόρθωσης patches. Με την παραπάνω εντολή, ουσιαστικά, εφαρμόζουμε στην νεότερη έκδοση όλα τα patches που βρίσκονται στον κατάλογο «**debian/patches»**. Εδώ, αν όλα πάνε καλά δεν θα μας εμφανιστεί κάποιο σφάλμα. Αν εμφανιστεί σφάλμα, τότε, αυτό σημαίνει πως κάποιο patch δεν εφαρμόστηκε σωστά στο λογισμικό. Αυτό, συνήθως, σημαίνει πως ο κώδικας του λογισμικού έχει αλλάξει στο σημείο όπου κάνει αλλαγές το εν λόγω patch και θα πρέπει να το επεξεργαστούμε κατάλληλα ώστε να εφαρμόζεται σωστά. Δεν θα ελέγξουμε αυτή την περίπτωση προς το παρόν, διότι θα χρειαστεί να επεκταθούμε σε πιο προχωρημένες διαδικασίες που αφορούν το packaging, δηλαδή την επεξεργασία ή την διαγραφή patches, αναλόγως με τις ανάγκες του πηγαίου κώδικα του λογισμικού. Αρκετές φορές, ειδικότερα σε αναβαθμίσεις που δεν απέχουν πολύ μεταξύ τους οι εκδόσεις, δεν εμφανίζονται τέτοια σφάλματα. Άλλες φορές εμφανίζονται.
![](https://cerebrux.files.wordpress.com/2016/04/quilt.png?w=680)


## • Βήμα 7: Καταγραφή αλλαγών



Προτού προχωρήσουμε με τη δημιουργία και τον έλεγχο του νέου πακέτου, πρέπει να καταγράψουμε τις αλλαγές που κάναμε. Όπως αναφέρθηκε σε προηγούμενη ενότητα, αυτό γίνεται στο αρχείο «**debian/changelog**».  Τώρα, όμως, το αρχείο «**debian/changelog**» αυτή τη φορά δεν θα ανοίξουμε με κάποιο κειμενογράφο χειροκίνητα, αλλά με την εξής εντολή:
```bash
dch -r```
Βλέπουμε πως θα ανοίξει το αρχείο **changelog** με το **nano** και πως ήδη σε αυτό έχουν εισαχθεί τα στοιχεία μας (επειδή προηγουμένως τα είχαμε ορίσει στο αρχείο **.bashrc**) και οι **πληροφορίες** για το πακέτο, την **έκδοσή** του και την **έκδοση του Ubuntu** για την οποία θα δημιουργηθεί. Δηλαδή, θα δούμε το εξής:
```bash
πακέτο (έκδοση.upstream-έκδοση.στο.ubuntu) διανομή; urgency=low/medium/high

* New upstream release

-- Όνομα Επίθετο <e-mail> Ημερομηνία```

Όπως είπαμε, πρόκειται για αρχείο καταγραφής των αλλαγών, όπως αυτές που είδαμε στο προηγούμενο βήμα που αφορούν την ενημέρωση των εξαρτήσεων για τη νέα έκδοση. Βλέπουμε στο περιεχόμενο του αρχείου πως αρχικά αναγράφεται το **όνομα του πακέτου**, ή **έκδοση Upstream** (πχ. 3.20.0) του λογισμικού και η **έκδοση του στο Ubuntu** (πχ. 0ubuntu1) και η **προτεραιότητά** του. Ακριβώς από κάτω, με αστερίσκους βάζουμε σχόλια (εν συντομία) που αφορούν τις αλλαγές που κάναμε (που προς το παρόν αφορούν μόνο αλλαγές στις εξαρτήσεις του λογισμικού). Τέλος, φαίνεται η υπογραφή μας, δηλαδή τα στοιχεία μας τα οποία μπαίνουν αυτόματα σε αυτό το σημείο (δεν τα πειράζουμε χειροκίνητα), επειδή τα ορίσαμε στο **.bashrc** προηγουμένως. Κάτω από όλα αυτά, θα υπάρχει στο ίδιο μοτίβο η καταγραφή αλλαγών σε παλιότερες εκδόσεις του πακέτου από άλλους packagers (εννοείται πως δεν τα πειράζουμε). Σε αυτό το σημείο, εμείς θα χρειαστεί να κάνουμε δύο αλλαγές.

1. Προσθήκη **σχολίου** για καταγραφή των αλλαγών που κάναμε στις εξαρτήσεις.
2. Προσθήκη κάποιου **tag** (ό,τι θέλουμε) στην έκδοση του πακέτου για να γνωρίζουμε όταν το δούμε στο σύστημα πως είναι δικό μας (προαιρετικά για προσωπική χρήση, αλλά ακολουθούμε τους κανόνες του εκάστοτε αποθετηρίου αν λειτουργούμε εντός ομάδας packaging).

Για παράδειγμα, μετά τις αλλαγές, η αρχική καταχώρηση στο αρχείο θα είναι κάπως έτσι:
```bash
πακέτο (έκδοση.upstream-έκδοση.στο.ubuntu~myusername) xenial; urgency=high/medium/low

* New upstream release

* debian/control.in: Bump build-dep on εξάρτηση_χ

-- Όνομα Επίθετο <e-mail> Ημερομηνία```

Έγινε δηλαδή προσθήκη σχολίου πως στο αρχείο «**debian/control.in**» ενημερώσαμε την *εξάρτηση_χ* (πράγμα που έγινε στο προηγούμενο βήμα) και η προσθήκη ενός **tag** (πχ. το *username* μας) στην έκδοση του πακέτου. Αποθηκεύουμε, στην συνέχεια, το αρχείο ως «**changelog**» και όχι ως *~~changelog.dch~~* όπως θα προτείνει το nano (διότι εκτελέσαμε την εντολή dch) και κλείνουμε τον nano.
![](https://cerebrux.files.wordpress.com/2016/04/dch.png?w=735)
![](https://cerebrux.files.wordpress.com/2016/04/changeloga.png?w=735)
![](https://cerebrux.files.wordpress.com/2016/04/changelogb.png?w=735)
 


## • Βήμα 8: Δημιουργία πακέτου



Με την περαίωση όλων των παραπάνω, ότι αλλαγές χρειάζονταν να γίνουν για την σωστή αναβάθμιση του λογισμικού, ολοκληρώθηκαν. Τώρα μπορούμε να χτίσουμε το πακέτο μας.

Βρισκόμαστε στον φάκελο με την νέα έκδοση του λογισμικού, δηλαδή αυτόν που εδώ αναγράφουμε ως «**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)'**». Προκειμένου να χτίσουμε το πακέτο έτσι όπως το διαμορφώσαμε, απλά εκτελούμε στο τερματικό:
```bash
debuild -S```
Και το πρόγραμμα θα ξεκινήσει και θα ολοκληρώσει την διαδικασία. Αν δεν έχουμε παραλείψει κάτι και έγιναν όλα σωστά, μετά από μερικά δευτερόλεπτα θα δούμε αυτό:
```bash
Successfully signed dsc and change files```
Ίσως μας ζητηθεί να πληκτρολογήσουμε και τον κωδικό του κλειδιού GPG που έχουμε δημιουργήσει προηγουμένως.

Αφού ολοκληρωθεί το χτίσιμο του πακέτου, πίσω στον αρχικό μας φάκελο (αυτόν που έχουμε δημιουργήσει για να δουλεύουμε) θα έχουν δημιουργηθεί τα εξής αρχεία:

* Tο tarball «**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)-ΕΚΔΟΣΗ_UBUNTU'~tag.debian.tar**», το οποίο περιέχει την νεότερη έκδοση του λογισμικού, μαζί με τον κατάλογο «**debian**» ενημερωμένο, δηλαδή, με όλες τις αλλαγές που εφαρμόσαμε στα προηγούμενα βήματα.
* Η εικόνα «**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)-ΕΚΔΟΣΗ_UBUNTU'~tag.dsc**», δηλαδή η εικόνα του νέου πακέτου για το Ubuntu από το παραπάνω tarball πάνω στην οποία θα γίνει έλεγχος.
* Το αρχείο «**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)-ΕΚΔΟΣΗ_UBUNTU'~tag_source.build**».
* Το αρχείο «**ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)-ΕΚΔΟΣΗ_UBUNTU'~tag_source.changes**».

![](https://cerebrux.files.wordpress.com/2016/04/debuilda.png?w=735)
![](https://cerebrux.files.wordpress.com/2016/04/debuildb.png?w=735)


## • Βήμα 9: Έλεγχος πακέτου



Το πακέτο μας ουσιαστικά έχει δημιουργηθεί. Θεωρητικά (και πρακτικά) μπορούμε να το ανεβάσουμε στο Launchpad (θα δούμε σε επόμενη ενότητα πως), όμως, πριν γίνει αυτό, πρέπει πρώτα να κάνουμε έναν έλεγχο για να δούμε αν το πακέτο μπορεί να χτιστεί σωστά σε ένα σύστημα Ubuntu. Αυτό είναι το τελευταίο βήμα, ίσως το πιο σημαντικό, καθώς αν έχει γίνει κάποιο λάθος, θα φανεί εδώ.

Αφού επιστρέψουμε στον αρχικό μας φάκελο:
```bash
cd ..```

Eκτελούμε στο τερματικό:
```bash
sbuild --dist=xenial --arch=amd64 -A -c xenial-amd64 ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)-ΕΚΔΟΣΗ_UBUNTU'~tag.dsc```


> **Παρατήρηση**: Όπως και προηγουμένως, στην ενότητα για την κατάλληλη ρύθμιση του *sbuild* χρειάστηκε προσοχή στο όνομα που θα δώσουμε στο περιβάλλον ελέγχου καθώς και στον ορισμό της έκδοσης του Ubuntu για την οποία δημιουργούμε το πακέτο. Βλέπουμε πως η ονομασία του περιβάλλοντος και η κωδική ονομασία της έκδοσης χρησιμοποιούνται στην παραπάνω εντολή. Αναλόγως, λοιπόν, τις ονομασίες που ορίσαμε τότε και την διανομή για την οποία πακετάρουμε το λογισμικό, κάνουμε τις κατάλληλες αλλαγές στην εντολή παραπάνω.
> 
**Επεξήγηση**: Η διαδικασία που εφαρμόζεται με την εκτέλεση της παραπάνω εντολής, δεν είναι άλλη από την δημιουργία και την ενημέρωση ενός συστήματος Ubuntu (αυτή άλλωστε είναι η δουλειά του *sbuild*), στην default κατάστασή του, προκειμένου να εκτελεστεί εκεί η προσπάθεια εγκατάστασης του πακέτου που δημιουργήσαμε και να γίνει ένας εκτεταμένος έλεγχος για την σωστή πρόοδο της διαδικασίας. Η διαδικασία αυτή θα αργήσει, σχετικά, να ολοκληρωθεί. Οπότε, περιμένουμε.


Αφού ολοκληρωθεί η διαδικασία, στο τέλος θα δούμε μία περίληψη (**Summary**) του ελέγχου που πραγματοποιήθηκε με την εκτέλεση της τελευταίας εντολής. Στο summary, λοιπόν, θα υπάρχει η γραμμή όπου θα αναγράφεται το αποτέλεσμα του ελέγχου (**Status**), το οποίο θα είναι είτε «**Successful**» εφόσον ο έλεγχος ολοκληρώθηκε επιτυχώς, είτε «**Failed**» αν υπήρξαν σφάλματα.

Αν ο έλεγχος ολοκληρωθεί με επιτυχία, στον αρχικό μας φάκελο θα έχουμε επιπλέον τα εξής αρχεία:

* **ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)-ΕΚΔΟΣΗ_UBUNTU'~tag_amd64.build**
* **ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)-ΕΚΔΟΣΗ_UBUNTU'~tag_amd64.changes**
* **ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)-ΕΚΔΟΣΗ_UBUNTU'~tag_amd64.deb**
* **ΟΝΟΜΑ_ΠΑΚΕΤΟΥ-dbgsym_'ΤΕΛΕΥΤΑΙΑ_ΕΚΔΟΣΗ(Upstream)-ΕΚΔΟΣΗ_UBUNTU'~tag_amd64.ddeb**

![](https://cerebrux.files.wordpress.com/2016/04/sbuilda.png?w=735)
![](https://cerebrux.files.wordpress.com/2016/04/sbuildb.png?w=735)
![](https://cerebrux.files.wordpress.com/2016/04/sbuildc.png?w=735)
![](https://cerebrux.files.wordpress.com/2016/04/sbuildd.png?w=735)


## Η διαδικασία ολοκληρώθηκε. Αλλά, λειτουργεί το λογισμικό;

![](https://cerebrux.files.wordpress.com/2016/04/package.png?w=735)
![](https://cerebrux.files.wordpress.com/2016/04/software.png?w=752)


Ναι, λειτουργεί. Μάλιστα, στην συγκεκριμένη περίπτωση του παραδείγματος, αν δεν είχαμε χρησιμοποιήσει το tag «**~cerebrux**» για το πακέτο, αλλά το «**~xenial0**», θα μπορούσαμε να στείλουμε το πακέτο μας στην ομάδα «[GNOME3 Team»](https://launchpad.net/~gnome3-team) η οποία συντηρεί το αποθετήριο «[gnome3-staging](https://launchpad.net/~gnome3-team/+archive/ubuntu/gnome3-staging)», το οποίο περιέχει πάντα την τελευταία έκδοση των συστατικών του GNOME, ώστε να το προσθέσουν στο αποθετήριο.

 




