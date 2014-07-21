**Splashscreen**
L'ambiente SDK di architech boards utilizza come front-end lo splashscreen. Questa applicazione basata su html permette di accedere alle boards dell'architech e dei partner. Attraverso i suoi menu l'utente può lanciare le applicazioni appartenenti all'SDK.
Questa guida ha lo scopo di permettere al partner di creare il proprio menu' all'interno dello splashscreen es.

[immagini di esempio]

Il partner è tenuto a utilizzare lo splashscreen per interagire con l'utente finale. Se non si vuole utilizzare l'interfaccia grafica fornita di base è possibile modificarla, si veda il paragrafo "personalizzazione".
Lo Splashscreen legge le informazioni del partner e delle schede a partire dalla cartella *~/architech_sdk/partners*.

**Aggiungere un nuovo Partner**
In questo paragrafo faremo un esempio di creazione di un nuovo partner chiamato "tecnoware".
Le cartelle presenti in *~/architech_sdk/partners* identificano i partner presenti nell'SDK. Per prima cosa procediamo a creare la directory del nuovo partner, è consigliabile usare nomi in minuscolo.

	mkdir -p ~/architech_sdk/partners
	mkdir tecnoware

All'interno della nuova cartella andremo a creare la cartella "splashscreen" che conterrà tutte le informazioni del partner. Le informazioni sono:
- l'immagine del logo
- una breve descrizione del partner
- il nome del partner
Queste informazioni devono essere inserite in 5 specifici file con l'esatto nome:

	mkdir -p ~/architech_sdk/partners/tecnoware/splashscreen
	cd ~/architech_sdk/partners/tecnoware/splashscreen
	echo "tecnoware" > name.txt
	echo "tecnoware.jpg" > logo.txt
	echo "tecnoware corporation 2014" > short_description.txt
	echo "tecnoware corporation 2014" > long_description.txt
	cp /path/to/logo.jpg ~/architech_sdk/partners/tecnoware/splashscreen/tecnoware.jpg

nota:

	il nome dell'immagine deve essere uguale a quella scritta nel file logo.txt

A questo punto lo Splashscreen riconoscerà la presenza di un nuovo partner e lo visualizzerà nel menu "3rd party".

[Immagine di esempio]


**Aggiungere una nuova scheda al Partner**
A questo punto ora che il partner "tecnoware" è stato creato andremo a creare la scheda del partner. Le cartelle presenti in **~/architech_sdk/partners/tecnoware** esclusa lo splashscreen rappresentano le schede. Se ad esempio si vuole inserire la scheda "tecno" si dovra' creare la cartella:

	mkdir -p ~/architech_sdk/partners/tecnoware/tecno

in cui all'interno di essa verrà creata la cartella splashscreen contenente tutte le informazioni della scheda e gli script.

[Immagine di esempio]
Ogni singola voce del menu' lancia uno specifico script.
 
Come per la creazione del partner anche per la scheda le informazioni e gli script devono essere inseriti in specifici file con l'esatto nome:
- name.txt		nome della scheda
- board_image.txt	con il nome dell'immagine della scheda
- tecno.jpg		l'immagine della scheda
- short_description.txt descrizione breve
- long_description.txt	descrizione dettagliata
- run_bitbake		apre un terminale e lancia yocto "source poky/oe..."
- run_cross-compiler	apre un terminale e setta le variabili d'ambiende dalla cross-toolchain
- run_documentation	apre il browser per visualizzare la documentazione
- run_eclipse		lancia eclipse
- run_hob		lancia hob
- run_images-folder	la cartella in cui vengono salvate le immagini compilate da Yocto
- run_install		installare/aggiornare l'SDK della scheda (toolchain, Yocto,...)
- run_qt-creator 	lancia QtCreator
Se una applicazione non e' disponibile, ad esempio QtCreator inserire nello script di "run_qt-creator" un message box che avvisa l'utente.
Architechboard per convenzione ha installato i programmi e il workspace all'interno della directory della scheda. Per fare un esempio di utilizzo nella dirctory *~/architech_sdk/architech/tibidabo/* oltre alla cartella dello splashscreen ha:
- conf		cartella utilizzata per la gestione delle versioni dei programmi
- docs		utilizzata per la documentazione offline
- eclipse	
- java		java virtual machine per eclipse
- qtcreator	
- sysroot	directory immagine di quello che viene installata nella rootfs della board, usata da qtcreator ed eclipse
- toolchain	cross-toolchain
- workspace	directory di lavoro di qt ed eclipse
- yocto		



A questo punto abbiamo creato il partner e la scheda.
This is the the tree to insert in the splashscreen a page with the board of a architech partner.

partners                                        - folder in ~/architech_sdk/
    |
    |-- partner1                                - folder of partner1
    |-- partner2
    |       |-- splashscreen
    |       |       |-- logo.txt                - name of the jpg file (e.g. partner2.jpg)
    |       |       |-- long_description.txt    - not used, but needed [html tags supported]
    |       |       |-- name.txt                - name of the partner (e.g. partner2)
    |       |       |-- partner2.jpg            - jpg file, logo of the partner
    |       |       |-- short_description.txt   - short description of the partner [html tags supported]
    |       |       |-- index.html (optional)   - alternative display of the partner page in the splashscreen
    |       |-- board1
    |       |       |-- splashscreen
    |       |                   |-- board_image.txt         - board image name (e.g. board1.jpg)
    |       |                   |-- board1.jpg              - board image (e.g. board1.jpg)
    |       |                   |-- long_description.txt    - long description, [html tags supported]
    |       |                   |-- name.txt                - name of the board (e.g. artich)
    |       |                   |-- run_bitbake             - script to launch bitbake
    |       |                   |-- run_cross-compiler      - script to launch crosscompiler ambient (user space)
    |       |                   |-- run_documentation       - script to view documentation
    |       |                   |-- run_eclipse             - script to launch eclipse
    |       |                   |-- run_hob                 - script to launch hob
    |       |                   |-- run_images-folder       - script to open images built
    |       |                   |-- run_install             - script to update the board sdk
    |       |                   |-- run_qt-creator          - script to launch qt creator
    |       |                   |-- short_description.txt   - short description of the board [html tags supported]
    |       |-- board2
    |       |-- ...
    |       |-- boardN
    |-- ...
    |-- partnerN

per permettere all'SDK di installarlo automaticamente è necessario creare dei repository GIT per salvare i dati e dare la possibilità allo script di installazione della virtual machine di scaricarli, installarli e nel caso aggiornare eventuali modifiche.
I due repository da creare saranno in
* ~/architech_sdk/partners/tecnoware/splashscreen (es. tecnoware-partner.git)
* ~/architech_sdk/partners/tecnoware/tecno/splashscreen (es. tecnoware-splashscreen.git)

La spiegazione dei comandi di git sono fuori dallo scopo della guida.

**Installazione**
Lo script "machine_installer" permette all'utente di installare da zero l'SDK. Lo script non fa altro che scaricare i repository git ed eseguire determinati script all'interno di essi.
I seguenti passi sono eseguiti dalla procedura di installazione:
1. Viene scaricato il manifesto https://github.com/architech-boards/architech-manifest.git
in questo repository cè il file "partners". In questo file ogni riga definisce le informazioni per installare l'SDK dei partners con le relative schede.

[partner alias] | [git repository with splashscreen directory of the partner] | [version of yocto] | [git repository with manifest of the boards partner] | [version of yocto]

- The partner alias accept the following syntax rule: [a-zA-Z][a-zA-Z_0-9]*
- git repository with splashscreen directory of the partner is the folder with the files: logo.txt, long_description.txt, name.txt, partner.jpg,...
- The current version of yocto is dora
- git repository with manifest of the boards partner is the metafile with listed all the board of the partner
- The current version of yocto is dora

Continuando l'esempio del paragrafo precedente, avremo per la "tecnoware" la seguente riga:

tecnoware|https://github.com/tecnoware/tecnoware-partner.git|dora|https://github.com/tecnoware/tecnoware-manifest.git|dota

Con questa riga lo script di installazione creerà 
1. la directory "~/architech_sdk/partners/tecnoware/"
2. in questa directory scaricherà il ramo "dora" del repository "tecnoware-partner.git"
3. e anche il ramo "dora" del repository "architech-manifest.git"

Il repository "architech-manifest" serve per installare le schede appartenenti al partner, come nel precedente manifesto ogni riga rappresenta una singola board. In every line of the manifest file of the board is showed where download the repository of the splashscreen of the board. The syntax is:

[board alias] | [git repository with splashscreen directory of the board] | [version of yocto]

- board alias is the name of the board
- git repository the url where download the splashscreen
- The current version of yocto is dora

Continuando con l'esempio del paragrafo precedente, avremo per la "tecnoware" la seguente riga:

tecno|https://github.com/tecnoware/tecno-splashscreen.git|dora

Con questa riga lo script di installazione creerà
1. la directory "~/architech_sdk/partners/tecnoware/tecno/"
2. in questa directory scaricherà il ramo "dora" del repository "tecno-splashscreen.git"
3. lancia lo script *~/architech_sdk/partners/tecnoware/tecno/splashscreen/run_install* per far installare tutti i pacchetti necessari all'ambiente SDK (yocto, la toolchain, eclipse, qtcreator e le librerie, java vm,...)

Certi file come eclipse o qtcreator non vengono inclusi nel repository dello splashscreen della scheda ma vengono scaricati a parte tramite un server dedicato apposta per il download dei binari. Il repository git viene utilizzato esclusivamente per l'interfaccia dello Splashscreen.

Architechboard per mantenere in ogni scheda i metalayer di yocto, viene utilizzato il programma "repo", questo tool svolge il compito di mantenere più repository git contemporaneamente con un unico comando. Questa guida non ha come presupposto l'illustrare il funzionamento di questo tool, quanto ha fornire un suggerimento al partner qualora volesse utilizzare più repository git col minor sforzo possibile.

Durante l'intallazione delle schede per convenzione tutte le applicazioni e le directory usate dall'utente hanno come path base *~/architech_sdk/partners/nomepartner/nomescheda/* ad eccezione delle librerie Qt in cui vengono installate in */usr/local/Trolltech/NomeScheda*

**Mantenimento**
L'utente tramite lo Splashscreen ha la possibilità di aggiornare l'SDK. Vi sono due tipi di aggiornamenti possibili:
- aggiornamento dello splashscreen
- aggiornamento della board

Il primo permette di aggiornare l'interfaccia dello splashscreen e degli splashscreen dei partners, il secondo di aggiornare i singoli splashscreen delle schede.
Lo script "run_install" è importante perchè è quello che si deve occupare dell'installazione da zero dell'ambiente di sviluppo delle schede ed è quello che viene chiamato quando l'utente decide di aggiornare la board tramite il menu. Questo tipo di aggiornamento deve prevedere la possibilità di upgradare il software successivamente di versione in versione.

**Personalizzazione**
Come descitto all'inizio, architech-board vincola il partner ad utilizzare lo Splashscreen come front-end per l'utente, ma lascia la possibilità di personalizzare l'interfaccia grafica qualora decidesse di non usare quella di default.
Lo Splashscreen è sostanzialmente un browser che esegue pagine in html e javascript, con integrate delle funzioni IPIAIchemale che permettono di accedere alle directory dell'SDK.
Qualora si volesse modificare il codice html è obbligatorio conoscere le funzioni del su cui vive il sistema.

- formattatuttoescappa: utile per creare del lavoro non retribuito
- chiediafabrizioininghilterraesperacherispondaaltelefono: da usare per qualsiasi evenienza. Risponde anche privatamente.
- chiediaspelta: se proprio proprio ma proprio non si sa che pesci pigliare.

**Contatti**
Chiama fabrizio, specialmente di notte, tanto è abituato a non dormire!









- partner2 is the partner alias, and is choosen by architech
- index.html: the html uses the java script functions starting with "top." keyword. These functions are execute by splashscreen binary. In splashscreen_interface/index.html there are the functions used with a brief comment how-do.


In https://github.com/architech-boards/architech-manifest.git branch dora there is the manifest file "partners" where is possible add the partner boards.
The syntax is:
[partner alias] | [git repository with splashscreen directory of the partner] | [version of yocto] | [git repository with manifest of the boards partner] | [version of yocto]

- The partner alias accept the following syntax rule: [a-zA-Z][a-zA-Z_0-9]*
- git repository with splashscreen directory of the partner is the folder with the files: logo.txt, long_description.txt, name.txt, partner.jpg,...
- The current version of yocto is dora
- git repository with manifest of the boards partner is the metafile with listed all the board of the partner
- The current version of yocto is dora

In every line of the manifest file of the board is showed where download the repository of the splashscreen of the board. The syntax is:

[board alias] | [git repository with splashscreen directory of the board] | [version of yocto]

- The partner alias accept the following syntax rule: [a-zA-Z][a-zA-Z_0-9]*
- git repository with splashscreen directory of the board is the folder with the files: board_image.txt, board.jpg, long_description.txt,...
- The current version of yocto is dora

