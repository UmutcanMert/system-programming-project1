# system-programming-project-threads
## Project-1: A Communicating Multi-shell (Part-1: intro, shared file)
### Bilinmesi gerekenler
C system calls
Exec family functions
Fork
Wait and similar system calls
File I/O

### Öğrenme Hedefleri
Practicing fork/exec relationships. Understanding the multi-process environment and how they can communicate through a shared object, understanding the details of a shell program. Understanding the need for inter process communication mechanisms which will be taught in the coming weeks.

### Önbilgi
Bu projede birbirleriyle memory paylasan multi-shell implementasyonu yapmaniz istenmektedir. Bunun icin size singleshell.c ve multishell.c dosyalari verilmistir.

### Detaylar ve yapılması istenilenler
#### singleshell.c
/*initmem() fonksiyonu icerigi*/
fd = shm_open(MY_SHARED_FILE_NAME, O_RDWR, 0);
/*Map shared memory*/
addr = mmap(NULL, 1024, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);

Burada shm_open paylasimli bir file obje oluştururken (kullanim sekli open ile birebir ayni), mmap bu objeyi memoryye map etmektedir. 
Burada size lazim olan bilgi, bunu aynen malloc seklinde dusunebilirsiniz. Yani addr değişkenini array variable olarak algılayabilirsiniz:
addr[0], addr[1] 

seklinde kullanabilirsiniz, yada buraya 
strncpy(&addr[0]), memcpy(addr,) 

kullanarak data kopyalayabilirsiniz. 
Tabi burada kopyalama işlemi yaparken daha once yazdiklarinizin üzerine yazmamak var olan stringin her zaman sonuna ekleme yapmaniz gerekiyor. Yine strcat yada strncpy() su şekilde kullanabilirsiniz:
strncpy(&addr[strlen(addr)], data, max_length);

Yine fd kullanarak bu alana direk yazip buradan direk okuma islemide yapabilirsiniz.
##### singleshell.c yi asagidaki sekilde tamamlamaniz istenmektedir:
-system(), kullanmayiniz!!!! exec fonksiyonları ve fork kullanmaniz istenmektedir.

-Program tum shelldeki stdout ve stderrleri ayrica addr'e yazmalıdır (sonlarina \n koyarak yazin).

-Ekleme: Bu kisimda sadece shelle yazilan komutlar, yani execle calistirilacak komut, execin outputunu addr’e yazmaniza gerek yok.
yani hem shellde gorulecek, hem dosyaya yazılacak (log gibi düşünebilirsiniz)
-Dosyaya yazarken başlarına kendi process id sini eklemelidir.

-Program calismaya basladiginda kendi process id sini ve parent process id sini ve calismaya baslama tarih, ay, gun, ve saatini dosyaya yazdirmalidir.

-exit verildiğinde calismayi sonlandirmali, sonlandirma isleminden once yine bitis tarih, ay, gun, saatini yazdırmalı.

-ls vb komutlar verildiğinde bunlari normal shelldeki gibi calistirmali. shell programın bitmesini beklemeli, sonra calismasina devam etmeli.

-current directorydeki ve baska bir directoryde program ismi verildiğinde onlarida calistirabilmeli
mesela, ./directory1/directory2/programname

-Yine verilen argümanları da uygun sekilde yönlendirmeli.

-Cikarken tum file descriptorlari vesaire kapamalidir. Yine kullandiginiz tum systemcallarinda hata kontrolu yapmanız istenmektedir.
#### multishell
Verilen sayida singleshell program instance'i olusturmaktadir. Mesela


multishell 4

4 adet singleshell programini xterm uygulamasi ile calistirmaktadir (xterm ./singleshell: 2.arguman ./singleshell).
