## VII. Πρόσδεση θυρών
### Εξαγωγή υπηρεσιών μέσω πρόσδεσης θυρών

Οι εφαρμογές (παγκόσμιου) ιστού (web apps) μερικές φορές εκτελούνται μέσα σε ένα εξυπηρετητή ιστού (webserver container).  Για παράδειγμα, εφαρμογές PHP μπορεί να τρέχουν ως ένα κομμάτι μέσα στο [Apache HTTPD](http://httpd.apache.org/), ή εφαρμογές Java μπορεί να τρέχουν μέσα στο [Tomcat](http://tomcat.apache.org/).

**Η εφαρμογή δώδεκα παραγόντων είναι εντελώς αυτοτελής (self-contained)** και δεν βασίζεται στην εισδοχή ενός εξυπηρετηή ιστού (webserver) κατά την εκτέλεση από το περιβάλλον εκτέλεσης για να δημιουργήσει μια υπηρεσία ιστού (web-facing service).  Η εφαρμογή ιστου (web app) **εξάγει το HTTP ως υπηρεσία μέσω πρόσδεσης σε μια θύρα (binding to a port)**, και αναμένοντας αιτήσεις εξυπηρέτησης που έρχονται από αυτή τη θύρα.

Σε ένα τοπικό περιβάλλον υλοποίησης, ο προγραμματιστής επισκέπτεται ένα URL υπηρεσίας όπως το `http://localhost:5000/` για να έχει πρόσβαση στην υπηρεσία που εξάγεται απο την εφαρμογή του.  Στην παραγωγή, ένα επίπεδο δρομολόγησης χειρίζεται τις αιτήσεις δρομολόγησης από ένα δημόσιο φιλοξενητή πρός τις διεργασίες ιστού προσδεδεμένες σε θύρα (port-bound web processes).

Αυτό τυπικά υλοποιείται χρησιμοποιώντας [δήλωση εξαρτήσεων](./dependencies) για να προστεθεί μια βιβλιοθήκη εξυπηρετητή ιστου (webserver library) στην εφαρμογή, όπως το [Tornado](http://www.tornadoweb.org/) για τη Python, το [Thin](http://code.macournoyer.com/thin/) για τη Ruby, ή το [Jetty](http://www.eclipse.org/jetty/) για τη Java και άλλες γλώσσες που βασίζονται στη JVM.  Αυτό συμβαίνει εξ' ολοκλήρου στο *χώρο χρήστη* (*user space*), δηλαδή, μέσα στον κώδικα της εφαρμογής.  Το συμβόλαιο (contract) με το περιβάλλον εκτέλεσης είναι η πρόσδεση σε μια θύρα για να εξυπηρετηθούν αιτήσεις.

Το HTTP δεν είναι η μόνη υπηρεσία που μπορεί να εξαχθεί μέσω πρόσδεσης θυρών (port binding).  Σχεδόν κάθε είδος λογισμικού εξυπηρετητή (server software) μπορέι να τρέξει μέσω μιας διεργασίας που προσδένεται σε θύρα και αναμένει εισερχόμενα αιτήματα εξυπηρέτησης.  Παραδείγματα περιλαμβάνουν το [ejabberd](http://www.ejabberd.im/) (που μιλάει [XMPP](http://xmpp.org/)), και το [Redis](http://redis.io/) (που μιλάει το [πρωτόκολλο Redis](http://redis.io/topics/protocol)).

Σημειώστε επίσης ότι η προσέγγιση της πρόσδεσης θυρών σημαίνει ότι μία εφαρμογή μπορεί να γίνει [υπηρεσία υποστήριξης](./backing-services) για μια άλλη εφαρμογή, παρέχοντας το URL της εφαρμογής υποστήριξης ως ένα αναγνωριστικό πόρου (resource handle) στις [παραμέτρους (config)](./config) της εφαρμογής που θα την καταναλώσει.