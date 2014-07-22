**Splashscreen**
L'ambiente SDK di architech boards utilizza come front-end il programma Splashscreen residente in *~/architech_sdk/splashscreen/*. Questa applicazione-browser basata su html permette di accedere alle boards di architech e dei partner. Attraverso i menu l'utente può lanciare le applicazioni dell'SDK (eclipse, qtcreator,...).

[immagini di esempio]

Questa guida illustra cosa deve fare un partner per creare il proprio menu' all'interno dello splashscreen. E' richiesta una conoscenza di base in Linux che non verrà fornita in questo tutorial.

Il partner è tenuto a utilizzare lo splashscreen come front-end. Se non si vuole utilizzare la grafica standard dei menu è possibile modificarla, si veda il paragrafo **Personalizzare i menu**.

La guida è suddivida nei sequenti paragragfi
- **Aggiungere un nuovo Partner**
- **Aggiungere una nuova scheda al Partner**
- **La fase di Installazione**
- **La fase di Aggiornamento**
- **Personalizzare i menu**

**Aggiungere un nuovo Partner**
In questo paragrafo creeremo per esempio un nuovo partner chiamato "tecnoware".
Lo Splashscreen per visualizzare i menu del partner e delle sue schede legge le informazioni a partire dalla cartella *~/architech_sdk/partners/*.
Le cartelle presenti in *~/architech_sdk/partners/* identificano i partner presenti nell'SDK. Per prima cosa procediamo a creare la directory del nuovo partner, è consigliabile usare nomi in minuscolo.

	mkdir -p ~/architech_sdk/partners
	mkdir tecnoware

All'interno della nuova cartella andremo a creare la cartella "splashscreen" che conterrà tutte le informazioni del partner. Le informazioni sono:
- il nome del partner
- l'immagine del logo
- una breve descrizione del partner
- opzionalmente il file index.html, per dettagli si veda il paragrafo **Personalizzare i menu**
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
A questo punto ora che il partner "tecnoware" è stato creato andremo a creare la sua prima scheda. Le cartelle presenti in **~/architech_sdk/partners/tecnoware** esclusa lo splashscreen identificano le schede del partner. Se ad esempio si vuole inserire la scheda "tecno" si dovra' creare la cartella:

	mkdir -p ~/architech_sdk/partners/tecnoware/tecno/splashscreen/

in cui all'interno verranno creati i file con le informazione della scheda e gli script per avviare i programmi dell'SDK. L'immagine sequente mostra il menu standard per la scheda tecno, ogni singola voce del menu lancia uno specifico script.

[Immagine di esempio]

Come per la creazione del partner anche i sequenti file devono essere creati con il nome esatto:
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
- opzionalmente il file index.html, per dettagli si veda il paragrafo **Personalizzare i menu**

Gli script devono essere scritti dal partner per ottemperare i compiti descritti sora. Se una applicazione non e' disponibile, ad esempio QtCreator, inserire nello script di "run_qt-creator" un message box che avvisa l'utente.

Architechboard per convenzione ha installato i programmi e il workspace all'interno della directory della scheda. Per fare un esempio, nella dirctory *~/architech_sdk/architech/tibidabo/* oltre alla cartella dello splashscreen sono presenti i file:
- conf		cartella utilizzata per la gestione delle versioni dei programmi
- docs		utilizzata per la documentazione offline
- eclipse	
- java		java virtual machine per eclipse
- qtcreator	
- sysroot	directory immagine di quello che viene installata nella rootfs della board, usata da qtcreator ed eclipse
- toolchain	cross-toolchain
- workspace	directory di lavoro di qt ed eclipse
- yocto		

A questo punto abbiamo creato sia il partner che la scheda. Concettualmente l'albero dei partners sarà costruito nel sequente modo:

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
    |       |                   |-- index.html (optional)   - alternative display of the board page in the splashscreen
    |       |-- board2
    |       |-- ...
    |       |-- boardN
    |-- ...
    |-- partnerN

A questo punto come ultima operazione prima di passare alla fase di installazione è necessario salvare ogni splashscreen creato in un repository git.
I due repository da creare saranno in
* ~/architech_sdk/partners/tecnoware/splashscreen (es. tecnoware-partner.git)
* ~/architech_sdk/partners/tecnoware/tecno/splashscreen (es. tecnoware-splashscreen.git)

La spiegazione dei comandi di git sono fuori dallo scopo della guida.
Questa procedura è essenziale per permettere al programma di installazione dell'SDK di scaricare e installare/aggiornare i dati del partner.

**La fase di Installazione**
Lo script "machine_installer" permette all'utente di installare da zero l'intero SDK sulla propria macchina Linux. Lo script non fa altro che scaricare dei repository git ed eseguire determinati script all'interno di essi.

Durante l'intallazione per prima cosa viene scaricato il manifesto da *https://github.com/architech-boards/architech-manifest.git* in questo repository cè il file "partners". In questo file ogni riga definisce le informazioni per installare l'SDK dei partners con le sue schede.

[partner alias] | [git repository with splashscreen directory of the partner] | [version of yocto] | [git repository with manifest of the boards partner] | [version of yocto]

- The partner alias accept the following syntax rule: [a-zA-Z][a-zA-Z_0-9]*
- git repository with splashscreen directory of the partner is the folder with the files: logo.txt, long_description.txt, name.txt, partner.jpg,...
- The current version of yocto is dora
- git repository with manifest of the boards partner is the metafile with listed all the board of the partner
- The current version of yocto is dora

Continuando l'esempio del paragrafo precedente, avremo per la "tecnoware" la seguente riga nel manifesto:

tecnoware|https://github.com/tecnoware/tecnoware-partner.git|dora|https://github.com/tecnoware/tecnoware-manifest.git|dota

Lo script di installazione interpretando la linea eseguirà i sequenti comandi:
1. crea la directory "~/architech_sdk/partners/tecnoware/"
2. in questa directory scaricherà il ramo "dora" del repository "tecnoware-partner.git"
3. e anche il ramo "dora" del repository "architech-manifest.git"

Il repository "architech-manifest" serve per installare le schede appartenenti al partner, in every line of the manifest file of the board is showed where download the repository of the splashscreen of the board. The syntax is:

[board alias] | [git repository with splashscreen directory of the board] | [version of yocto]

- board alias is the name of the board
- git repository the url where download the splashscreen
- The current version of yocto is dora

Continuando con l'esempio del paragrafo precedente, avremo per la "tecnoware" la seguente riga:

tecno|https://github.com/tecnoware/tecno-splashscreen.git|dora

Con questa riga lo script di installazione creerà
1. la directory "~/architech_sdk/partners/tecnoware/tecno/"
2. in questa directory scaricherà il ramo "dora" del repository "tecno-splashscreen.git"
3. lancia lo script *~/architech_sdk/partners/tecnoware/tecno/splashscreen/run_install* per far installare tutti i pacchetti necessari all'ambiente SDK (yocto, la toolchain, eclipse, qtcreator e le librerie, java vm,...).

A questo punto sarà lo script "run_install" programmato dal partner ad installare tutti i programmi necessari alla scheda. Come linea guida suggeriamo di seguire i sequenti punti

1. Il repository git viene utilizzato esclusivamente per i file dell'interfaccia dello Splashscreen, i file che non fanno parte di quelli descritti nei paragrafi precedenti (eclipse o qtcreator,...) non vengono inclusi nei repository git ma vengono da un server dedicato per il download dei binari. 
2. Architechboard per mantenere in ogni scheda i metalayer di yocto, utilizza il programma "repo", questo tool svolge il compito di mantenere più repository git contemporaneamente con un unico comando. Questa guida non ha come presupposto l'illustrare il funzionamento di questo tool, quanto ha fornire un suggerimento al partner qualora volesse mantenere più repository git col minor sforzo possibile.
3. Lo script "run_install" deve permettere di essere eseguito più volte e se necessario aggiornare i file installati.
4. Durante l'intallazione delle schede per convenzione tutte le applicazioni e le directory usate dall'utente hanno come path base *~/architech_sdk/partners/nomepartner/nomescheda/* ad eccezione delle librerie Qt in cui vengono installate in */usr/local/Trolltech/NomeScheda*

**La fase di Aggiornamento**
L'utente tramite lo Splashscreen ha la possibilità di aggiornare l'SDK. Vi sono due tipi di aggiornamenti possibili:
- aggiornamento dello splashscreen

[ immagine di esempio ]
Si occupa di aggiornare con l'operazione di "pull" il repository git dello splashscreen, aggiornando anche il repository "architech-manifest.git".

- aggiornamento della board

[ immagine di esempio ]
Questo aggiornamento provvede ad aggiornare tutti i repository splashscreen delle schede installate nell'SDK.

Ribadiamo che lo script "run_install" è importante perchè è quello che si deve occupare dell'installazione da zero dell'ambiente di sviluppo delle schede ed è quello che viene chiamato quando l'utente decide di aggiornare la board tramite il menu. Questo tipo di aggiornamento deve prevedere la possibilità di upgradare il software qualora ve ne fosse il bisogno.

**Personalizzare i menu**
Come descitto all'inizio della guida, architech-board vincola il partner ad utilizzare lo Splashscreen come front-end per l'utente, ma lascia la possibilità di personalizzare l'interfaccia grafica qualora decidesse di non usare quella di default.
Si possono personalizzare due tipi di schermate:
1. Schermata del partner, residente in */architech_sdk/partners/nome_partner/splashscreen/*
2. La schermata della scheda, residente in */architech_sdk/partners/nome_partner/splashscreen/*
Per personalizzare l'interfaccia si deve creare il file "index.html" nella directory dello splashscreen il quale rimpiazzerà la schermata di default.
Lo Splashscreen è sostanzialmente un browser che esegue pagine in html e javascript, con la possibilità di utilizzare delle funzioni IPA che permettono di accedere alle directory dell'SDK. Queste pagine html si trovano nella directory *~/architech_sdk/splashscreen_interface/*. 

Le funzioni di supporto per accedere alle directory sono in index.html:
- function get_absolute_path( relative_path )
- function get_partner_directory( partner_alias )
- function get_partner_board_directory( partner_alias, board_alias )
- function get_splashscreen_interface_directory()
per chiamarle si usa "top.nomefunzione( parametro )

Le funzioni di supporto a loro volta chiamano le funzioni implementate nell'applicazione dello splashscreen. I sorgenti dello splashscreen si trovano nella cartella *~/architech_sdk/splashscreen*. Ad esempio la funzione "get_absolute_path" richiama a sua volta la funzione "process_start_detached" presente nel modulo "launcer.cpp" dei sorgenti dello Splashscreen.

**Contatti**

Per problemi di sviluppo contattare la RSR, tremite il contatto aratti@rsr.it










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

