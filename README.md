# Tema 4 - Client Web. Comunicatie cu REST API
## Gorin Natalia Stefania - 333CD

* am inceput prin a-mi defini macro-uri pentru host, port, payload si rutele de acces pentru a putea fi mai usor de citit in cod

### **main()**
* cu ajutorul unei bucle ciclez pana in momentul in care comanda care este data de la tastatura de utilizator este cea de *exit*. 
* in interiorul buclei deschid o conexiunde de retea cu serverul specificat in enuntul temei, urmand apoi gestionarea comenzilor

### **register**
* in momentul in care utilizatorul da comanda de *register* se citesc de la tastatura credentialele cu care utilizatorul doreste sa se inregistreze
* credentialele sunt date ca parametru functiei *register_req()* unde:
    * se verica daca aceste nu contin spatii, iar in cazul in care s-au gasit spatii se intoarce un mesaj de eroare. 
    * in cazul in care nu s-au gasit spatii, se formeaza un JSON String prin apelarea functiei *json_reg_log()* pe care am implementat-o ajutandu-ma  de functiile din *parson.c* (din enunt)
    * construiesc cererea POST, o trimit serverului apoi salvez intr-o variabila mesajul primit inapoi de la server
    * din continutul mesajului primit de la server, extrag exit code-ul cu ajutorul functiei *find_exit_code()* care gaseste primul spatiu si copiaza primele 3 caractere gasite dupa acesta si pune '\0' la final, acesta reprezentand exit code-ul.
    * se verifca daca exit code-ul este unul bun si afiseaza mesaje de succes sau eroare in functie de caz. 

### **login**
* in momentul in care utilizatorul da comanda de *login*, se verifica daca cookie-ul de sesiune este NULL. In cazul in care nu este NULL, acest lucru inseamna ca utilizatorul este dega autentificat si se intoarce un mesaj de eroare. In cazul in care cookie-ul de sesiune este NULL, se citesc credentialele utilizatoruui si se apeleaza functia de *login_req* unde: 
    * verific daca credentialele nu contin spatii, iar daca contin afisez un mesaj de eroare
    * in cazul in care nu exista spatii, se formeaza un JSON string cu ajutorul functiei *json_reg_log()* pe care am implementat-o ajutandu-ma  de functiile din *parson.c* (din enunt)
    * construiesc cererea POST, o trimit serverului apoi salvez intr-o variabila mesajul primit inapoi de la server
    * din continutul mesajului primit de la server, extrag exit code-ul cu ajutorul functiei *find_exit_code()* care gaseste primul spatiu si copiaza primele 3 caractere gasite dupa acesta si pune '\0' la final, acesta reprezentand exit code-ul.
    * daca exit code-ul este unul bun, caut mesajul "Set-Cookie:" si adaug 12 pentru a ajunge la inceputul valorii cookie-ului, caut finalul (;), calculez lungimea cookie-ului si pe ultima pozitie pun '\0' si afisez un mesaj de succes, in caz contrar, afisez mesaj de eroare

### **enter_library**
* in momentul in care utilizatorul introduce de la tastatura comanda *enter_library*, verific daca cookie-ul de sesiune este NULL, in cazul in care este NULL, afisez un mesaj de eroare pentru a anunta ca este necesara autentificarea mai intai, iar in cazul in care acesta nu este NULL, trec mai departe si verific token-ul
* in cazul in care tokenul nu este NULL, afisez un mesaj de eroare, asta insemnand ca utilizatorul a accesat deja biblioteca, iar in cazul in care tokenul este NULL se apeleaza functia *enter_library_req()* unde: 
    * construiesc cererea GET, o trimit serverului apoi salvez intr-o variabila mesajul primit inapoi de la server
    * din continutul mesajului primit de la server, extrag exit code-ul cu ajutorul functiei *find_exit_code()* care gaseste primul spatiu si copiaza primele 3 caractere gasite dupa acesta si pune '\0' la final, acesta reprezentand exit code-ul.
    * daca exit code ul este unul bun, gasesc tokenul in mesajul de la server si pun '\0' in final, apoi afisez un mesaj de succes, iar in caz contrar unul de eroare

### **get_books**
* cand este introdusa comanda de *get_books* se verifica tokenul. In cazul in care acesta este NULL se afiseaza un mesaj de eroare, asta insemnand ca utilizatorul nu a accesat biblioteca, in caz contrar, se apeleaza functia *get_books_req()* unde: 
    * construiesc cererea GET, o trimit serverului apoi salvez intr-o variabila mesajul primit inapoi de la server
    * din continutul mesajului primit de la server, extrag exit code-ul cu ajutorul functiei *find_exit_code()* care gaseste primul spatiu si copiaza primele 3 caractere gasite dupa acesta si pune '\0' la final, acesta reprezentand exit code-ul.
    * daca exit code-ul este bun, extrag cartile din din mesajul de la server prin cautarea simbolului "[", ulterior se afiseaza lista cu carti si un mesaj de succes, in caz contrar se afiseaza un mesaj de eroare. 

### **get_book**
* cand este introdusa comanda de *get_book* se verifica tokenul. In cazul in care acesta este NULL se afiseaza un mesaj de eroare, asta insemnand ca utilizatorul nu a accesat biblioteca, in caz contrar, se apeleaza functia *get_book_req()* unde: 
    * se citeste id-ul cartii dorite
    * contruiesc cererea GET, o trimit serverului apoi salvez intr-o variabila mesajul primit inapoi de la server
    * din continutul mesajului primit de la server, extrag exit code-ul cu ajutorul functiei *find_exit_code()* care gaseste primul spatiu si copiaza primele 3 caractere gasite dupa acesta si pune '\0' la final, acesta reprezentand exit code-ul.
    * daca este exit code ul bun, extrag informatiile despre cartea cu id-ul dat si afisez, iar in caz contrar afisez mesaj de eroare

### **add_book**
* cand este introdusa comanda de *add_book* se verifica tokenul. In cazul in care acesta este NULL se afiseaza un mesaj de eroare, asta insemnand ca utilizatorul nu a accesat biblioteca, in caz contrar, se apeleaza functia *add_book_req()* unde: 
    * citesc campurile pentru adaugarea cartii si verific daca acestea au fost completate corespunzator
    * daca s-a trecut de verificari, formez JSON string-ul cu ajutorul functiei *json_book()* implementata cu ajutorul functiilor din *parson.c* 
    * contruiesc cererea POST, o trimit serverului apoi salvez intr-o variabila mesajul primit inapoi de la server
    * din continutul mesajului primit de la server, extrag exit code-ul cu ajutorul functiei *find_exit_code()* care gaseste primul spatiu si copiaza primele 3 caractere gasite dupa acesta si pune '\0' la final, acesta reprezentand exit code-ul.
    * daca exit code-ul e ok afisez mesaj de succes, altfel afisez mesaj de eroare

### **delete_book**
* cand este introdusa comanda de *delete_book* se verifica tokenul. In cazul in care acesta este NULL se afiseaza un mesaj de eroare, asta insemnand ca utilizatorul nu a accesat biblioteca, in caz contrar, se apeleaza functia *delete_book_req()* unde: 
    * citesc id-ul cartii care urmeaza sa fie stearsa 
    * contruiesc cererea DELETE, o trimit serverului apoi salvez intr-o variabila mesajul primit inapoi de la server
    * din continutul mesajului primit de la server, extrag exit code-ul cu ajutorul functiei *find_exit_code()* care gaseste primul spatiu si copiaza primele 3 caractere gasite dupa acesta si pune '\0' la final, acesta reprezentand exit code-ul.
    * daca exit code-ul e ok afisez mesaj de succes, altfel afisez mesaj de eroare

### **logout**
* cand este data comanda de *logout* se verifica daca cookieul este null, iar in cazul in care este se afiseaza o eroare pentru a anunta utilizatorul ca trebuie sa se autentifice mai inta. In caz contrar, se apeleaza functia *logout_req()* unde: 
    * contruiesc cererea GET, o trimit serverului apoi salvez intr-o variabila mesajul primit inapoi de la server
    * din continutul mesajului primit de la server, extrag exit code-ul cu ajutorul functiei *find_exit_code()* care gaseste primul spatiu si copiaza primele 3 caractere gasite dupa acesta si pune '\0' la final, acesta reprezentand exit code-ul.
    * daca exit code-ul e ok afisez mesaj de succes, altfel afisez mesaj de eroare

### **exit**
* in final, cand se da comanda *exit*, se iese din bucla principala

    


