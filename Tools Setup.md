# Ρύθμιση εργαλείων

Ήρθε η στιγμή να ρυθμίσουμε, για τις ανάγκες μας, το βασικό εργαλείο που θα χρησιμοποιούμε για τη δημιουργία και τον έλεγχο πακέτων λογισμικού. Πρόκειται για το «**sbuild**», το οποίο εγκαταστήσαμε προηγουμένως μαζί με άλλα χρήσιμα εργαλεία. Δεν είναι τίποτε άλλο, παρά ένα περιβάλλον προσομοίωσης του συστήματος χτισίματος πακέτων του Launchpad. Ουσιαστικά, ένα Ubuntu μέσα στο Ubuntu μας, όπου τα πακέτα που θα φτιάχνουμε θα ελέγχονται και θα δημιουργούνται μέσα σε αυτό. Προκειμένου να ρυθμίσουμε σωστά το «**sbuild**», ακολουθούμε τα εξής βήματα:

1. Με την εγκατάσταση του *sbuild*, δημιουργήθηκε και αντίστοιχο *group*, στο οποίο προσθέτουμε τον χρήστη μας, εκτελώντας στο τερματικό την εντολή:
```bash
sudo adduser $USER sbuild```
2. Δημιουργούμε ένα κατάλογο ο οποίος θα χρησιμοποιηθεί ως *mount point* για το *sbuild*, εκτελώντας:
```bash
mkdir -p $HOME/ubuntu/scratch
mkdir $HOME/ubuntu/logs```
> Εδώ, δημιουργείται ένας κατάλογος με όνομα «**ubuntu**» και μέσα σε αυτόν, ένας άλλος με όνομα «**scratch**» και ένας με το όνομα «**logs**». Αυτός ο φάκελος (**ubuntu**) είναι ένα ενδεικτικό παράδειγμα. Μπορείτε να δημιουργήσετε τον φάκελο όπου και με όποιο όνομα επιθυμείτε, αρκεί μέσα να φτιάξετε και τους άλλους δύο.

3. Ανοίγουμε το αρχείο **/etc/schroot/sbuild/fstab**, εκτελώντας την εντολή:
```bash
gedit /etc/schroot/sbuild/fstab```
Και τοποθετούμε σε αυτό την γραμμή:
```bash
/home/USERNAME/ubuntu/scratch /scratch none rw,bind 0 0```
Κάντε τις ανάλογες αλλαγές εάν χρησιμοποιήσατε άλλο φάκελο και αποθηκεύστε το αρχείο.
4. Δημιουργούμε το αρχείο «**.sbuildrc**» στον προσωπικό μας φάκελο, εκτελώντας την ακόλουθη εντολή:
```bash
gedit ~/.sbuildrc```
Και μέσα σε αυτό, προσθέτουμε το ακόλουθο κείμενο:
```bash
# Mail address where logs are sent to (mandatory, no default!)
$mailto = $ENV{USER};
# Name to use as override in .changes files for the Maintainer: field
# (mandatory, no default!).
$maintainer_name='ΟΝΟΜΑ ΕΠΙΘΕΤΟ <E-MAIL>;
# Default distribution to build.
$distribution = "xenial";
# Build arch-all by default.
$build_arch_all = 1;
# When to purge the build directory afterwards; possible values are "never",
# "successful", and "always". "always" is the default. It can be helpful
# to preserve failing builds for debugging purposes. Switch these comments
# if you want to preserve even successful builds, and then use
# "schroot -e --all-sessions" to clean them up manually.
$purge_build_directory = 'successful';
$purge_session = 'successful';
$purge_build_deps = 'successful';
# $purge_build_directory = 'never';
# $purge_session = 'never';
# $purge_build_deps = 'never';
# Directory for writing build logs to
$log_dir=$ENV{HOME}."/ubuntu/logs";
# don't remove this, Perl needs it:
1;```
Αποθηκεύουμε το αρχείο.
> Εδώ, παρατηρήστε πως υπάρχει η γραμμή «**$distribution = "xenial";**». Σε αυτό το σημείο, δηλώνουμε για ποια έκδοση του Ubuntu θα δημιουργήσουμε πακέτα. Στην προκειμένη περίπτωση, το **xenial** αντιστοιχεί στην έκδοση **Ubuntu 16.04 LTS**. Ακόμα, στην τελευταία γραμμή βλέπουμε αναφορά στον φάκελο «**ubuntu/logs**», που δημιουργήθηκε σε προηγούμενο βήμα. Αν έχετε φτιάξει φάκελο με άλλο όνομα και σε άλλη θέση, κάντε τις ανάλογες αλλαγές στην τελευταία γραμμή (το **$ENV{HOME}.** αντιστοιχεί στον προσωπικό φάκελο, οπότε κάντε τις αλλαγές μετά από αυτό).
> 
>Ακόμα, προσέξτε τη γραμμή όπου θα πρέπει να προσθέσετε το **ονοματεπώνυμό** σας και το **e-mail** σας, ως maintainer του πακέτου.

5. Δημιουργούμε το αρχείο «**.mk-sbuild.rc**» στον προσωπικό μας φάκελο, εκτελώντας την εντολή:
```bash
gedit ~/.mk-sbuild.rc```
Και μέσα σε αυτό, προσθέτουμε το ακόλουθο κείμενο:
```bash
SCHROOT_CONF_SUFFIX="source-root-users=root,sbuild,admin
source-root-groups=root,sbuild,admin
preserve-environment=true"
# you might want to undo the below for stable releases, read `man mk-sbuild` for details
SKIP_UPDATES="1"
SKIP_PROPOSED="1"
# if you have e.g. apt-cacher-ng around
# DEBOOTSTRAP_PROXY=http://127.0.0.1:3142/```
Αποθηκεύουμε το αρχείο.
6. Μπαίνουμε στο group όπου προσθέσαμε τον χρήστη νωρίτερα, με την εντολή:
```bash
sg sbuild```
7. Παράγουμε ένα κλειδί GPG εντός του group, με την εντολή:
```bash
sudo sbuild-update --keygen```
8. Και τέλος, δημιουργούμε το περιβάλλον χτισίματος, με την εντολή:
```bash
mk-sbuild xenial```
> Οι ονομασίες του περιβάλλοντος καλό θα είναι να έχουν την κωδική ονομασία της έκδοσης του Ubuntu για την οποία θα δημιουργούμε πακέτα, για να μην υπάρξει σύγχυση. Πάλι, εδώ ως παράδειγμα χρησιμοποιείται το *xenial*.
9. Τέλος, για να είναι συγχρονισμένο το περιβάλλον με τις αλλαγές που κάναμε, εκτελούμε στο τερματικό:
```bash
sudo sbuild-launchpad-chroot create -n xenial-amd64-sbuild -s xenial -a amd64
sudo sbuild-update --keygen```
> Εδώ, πάλι, προσέξτε στην πρώτη εντολή τα «**xenial**». Επαναλαμβάνομαι, αλλά είναι σημαντικό να μην γίνουν τέτοια λάθη. Βάζετε την κωδική ονομασία της έκδοσης για την οποία επιθυμείτε να φτιάξετε πακέτα. Επίσης έχω αφήσει εξ' ορισμού την αρχιτεκτονική των **64 bit**, αλλά αν θέλετε το αλλάζετε και αυτό. Ούτως ή άλλως, στο αρχείο *.sbuildrc* που δημιουργήσαμε στο **βήμα 4** ορίσαμε τα πακέτα που χτίζουμε να δημιουργούνται και για τις δύο αρχιτεκτονικές (32 και 64bit).

Στο σημείο αυτό, τελειώνει και η απαραίτητη προετοιμασία, οπότε, είμαστε έτοιμοι να συνεχίσουμε με την εισαγωγή στο πακετάρισμα λογισμικού για το Ubuntu.