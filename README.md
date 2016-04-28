# Εισαγωγή στο Ubuntu Packaging


### Aυτό το e-book δημιουργήθηκε για τις ανάγκες άρθρου στο [cerebrux.net](https://cerebrux.net/) ~eliasps



![](https://cerebrux.files.wordpress.com/2016/04/introduction-to-ubuntu-pagkaging.png?w=680)

Σε αυτό το e-book γίνεται μία εισαγωγή στην διαδικασία του «*πακεταρίσματος*» λογισμικού (ΕΛ/ΛΑΚ) για το Ubuntu.

Έχει ως στόχο να βοηθήσει τον αναγνώστη, είτε αυτός είναι αρχάριος χρήστης στο Ubuntu, είτε έμπειρος, να κατανοήσει τα βασικά βήματα της διαδικασίας του packaging και μέσω μίας ακολουθίας βημάτων να δοκιμάσει ο ίδιος να δημιουργήσει ένα πακέτο, ώστε να δει στην πράξη τα βήματα και την λογική τους.

Μπορεί να αναρωτηθεί κάποιος που θα του χρησιμεύσει αυτή η διαδικασία. Η απάντηση είναι «*ίσως πουθενά*», όμως, ίσως κάποιος ενδιαφέρεται να μάθει πως γίνεται για διάφορους λόγους. Είτε για την συνεισφορά στο **Ubuntu Project** υπό τη μορφή προσθήκης πακέτων στα επίσημα αποθετήρια (ή σε προσωπικό αποθετήριο τρίτων), είτε για να έχει ο ίδιος στο σύστημά του τελευταίες εκδόσεις εφαρμογών των που χρησιμοποιεί, χωρίς να περιμένει κάποια άλλη ομάδα να τις αναβαθμίσει, αλλά και για άλλους λόγους που ισχύουν για τον εκάστοτε packager.

Η αλήθεια είναι, πως, δεν έχει σημασία ο λόγος, ίσως με την εκμάθηση της διαδικασίας οι λόγοι να δημιουργηθούν αυτόματα. Οπότε, ας αρχίσουμε να εξετάζουμε πως μπορούμε να πακετάρουμε λογισμικό μέσω μίας ακολουθίας βημάτων, να δούμε τι θα χρειαστούμε και να εφαρμόσουμε την διαδικασία σε κάποια εφαρμογή, ώστε να την κατανοήσουμε στην πράξη.
