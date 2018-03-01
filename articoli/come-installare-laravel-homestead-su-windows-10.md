# Come installare Laravel+Homestead su Windows 10

Le informazioni per questa guida sono tratte da [qui](https://medium.com/@eaimanshoshi/i-am-going-to-write-down-step-by-step-procedure-to-setup-homestead-for-laravel-5-2-17491a423aa).

<p align="center" style="margin-top: 50px">
    <img src="https://2.bp.blogspot.com/-OsR2fZEw8QE/WbySFz-4I_I/AAAAAAAAF2M/S_7Fp6-Jblk372DAKn11VisA6Hb2D4ZVgCLcBGAs/s320/laravel-logo.png" alt="Logo Laravel">
</p>

## Prerequisiti

* Virtualizzazione attiva nel bios del computer
* [Download Git Bash](https://git-scm.com/download/win)
* [Download Virtualbox](https://www.virtualbox.org/wiki/Downloads)
* [Download Vagrant](https://www.vagrantup.com/downloads.html)
* [Download Visual Studio Code](https://code.visualstudio.com) _(opzionale)_

## Installazione

Le nostre operazioni prevedono di installare tutto in `C:\Progetti`. Sotto la cartella suddetta, Homestead sarà installato in `_homestead`, mentre le chiavi crittografiche necessarie per l'accesso allo stesso, si troveranno in `__rsa_keys`. 

Installare in quest'ordine: _Git Bash_, _Virtualbox_, _Vagrant_ ed eventualmente _Visual Studio Code_.

### a) Creare la Vagrant VM

Aprire _git bash_ come amministratore e lanciare:

```bash
vagrant box add laravel/homestead
```

Il programma richiederà quale versione installare in base al programma di virtualizzazione da usare.

In caso di errore, installare [MS Visual C++ 2010 x86 Redistributables](https://www.microsoft.com/en-us/download/confirmation.aspx?id=5555) e ripetere il comando in git bash.

### b) Clonare Homestead sul computer

Clonare homestead su `C:\Progetti` su _git bash_:

```bash
cd /c/progetti
git clone https://github.com/laravel/homestead.git _homestead
```

### c) Generare le chiavi crittografiche

Ora bisogna generare le chiavi crittografiche per accedere ad _Homestead_. Da _git bash_ lanciare:
```bash
cd /c/progetti
mkdir __rsa_keys
cd __rsa_keys
ssh-keygen -t rsa -C “your_email@example.com”
```

Alla domanda "Enter file in which to save the key (/c/Users/Nomeutente/.ssh/id_rsa):" rispondere `/c/Progetti/__rsa_keys/id_rsa`.
Alla domanda sulla passphrase si può non rispondere nulla.

Premere sempre INVIO per confermare. Non è necessario inserire una passphrase per generare le chiavi.

### d) Generare e modificare il file di configurazione di Homestead

Ora bisogna creare il file di configurazione `Homestead.yaml`:

```bash
cd /c/progetti
cd _homestead
bash init.sh
```

Una volta creato, aprirlo con il proprio editor di testi preferito, per farlo in _Visual Studio Code_, digitare:

```bash
cd /c/progetti
cd _homestead
code Homestead.yaml
```

Modificare le seguenti righe:

```
authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/code
      to: /home/vagrant/code

sites:
    - map: homestead.test
      to: /home/vagrant/code/public
```

con il testo seguente:

```
authorize: c:/Progetti/__rsa_keys/id_rsa.pub

keys:
    - c:/Progetti/__rsa_keys/id_rsa

folders:
    - map: c:/Progetti
      to: /home/vagrant/code

sites:
    - map: homestead.test
      to: /home/vagrant/code/Laravel1/public      
```

>_**Nota**: ricordarsi di usare la minuscola per indicare la lettera dell'unità e di separare il percorso con forward slash `/` invece del solito back slash `\`. Quindi il percorso che nella sintassi di Windows è `C:\Progetti`, diventa `c:/Progetti` in quella di `Homestead.yaml`._

### e) Modificare il file hosts di Windows

A questo punto dobbiamo fare in modo che il nostro _Homestead_ sia in grado di servire i nostri siti di prova nel browser. Per far ciò dobbiamo modificare il file `hosts` che si trova nella cartella `C:\Windows\System32\drivers\etc\`.

In _git bash_ scriviamo:

```bash
cd /c/Windows/System32/drivers/etc/
code hosts
```

Nel file che si apre, alla prima riga vuota in basso, scrivere:

```
192.168.10.10 homestead.test
```

In questo modo, ogni volta che nel browser digiteremo l'url `homestead.test`, automaticamente il browser servirà il nostro progetto.

### f) Aggiornare il profilo di Git Bash per accedere facilmente ad Homestead

Per facilitarci l'utilizzo di _git bash_, possiamo personalizzare le impostazioni contenute nel file `.bash_profile` che è salvato in `C:\Users\%userprofile%`:

```
cd ~
code .bash_profile
```

In `.bash_profile` possiamo inserire il seguente contenuto:

```bash
# Scorciatoie per navigazione facilitata
alias ..="cd .."
alias vagr="ssh vagrant@127.0.0.1 -p 2222"
alias dir="ls -l"
# Scorciatoia avvio di Homestead
function homestead() {
 ( cd c/Progetti/_homestead && vagrant $* )
}
```

> _**Spiegazione**: I comandi `alias` definiscono delle scorciatoie (sinonimi ai comandi di bash), ad esempio ho scelto che digitando `dir` come nel prompt di Windows, eseguiremo il comando bash `ls -l`, per ottenere la lista ordinata dei file presenti in una cartella (azione simile a `dir /o` di Windows). La funzione `homestead()` consente, invece, di lanciare più comandi sulla stessa linea._

Al termine delle modifiche, in _git bash_ lanciamo il seguente comando per aggiornare le impostazioni della shell:

```bash
source ~/.bash_profile
```

Ora lanciamo il seguente comando per avviare _Homestead_:
```bash
homestead up
```

Per creare un nuovo progetto di ```Laravel``` nella cartella `Laravel1`:
```
homestead ssh
composer create-project --prefer-dist laravel/laravel Laravel1
```

Per chiuderlo lanciamo:
```bash
homestead halt
```

_ _ _
## Alternative

In alternativa ad _Homestead_ è possibile usare _Homestead Improved_, una versione che semplifica alcune operazioni, [qui](https://www.sitepoint.com/quick-tip-get-homestead-vagrant-vm-running/) trovate un tutorial.

---

## Risoluzione problemi

Se si verifica il problema:

```
$ homestead up
Bringing machine 'homestead-7' up with 'virtualbox' provider...
==> homestead-7: Checking if box 'laravel/homestead' is up to date...
A VirtualBox machine with the name 'homestead-7' already exists.
Please use another name or delete the machine with the existing
name, and try again.
```

La soluzione è [questa](https://stackoverflow.com/a/30910282).

Dal prompt di Windows, lanciare:
```
cd "C:\Program Files\Oracle\VirtualBox"
vboxmanage list vms
```

Comparirà una lista di id macchina:

```
"Zorin OS" {02c48f8e-e46d-44eb-bb54-22cf1d83a0ae}
"homestead-7" {d041b7b8-7a8f-446a-8eab-b13f43066d65}
"ubuntu-16.04-amd64_1518144829485_92547" {1ee54cf9-9c9c-4760-a9b5-7b43a42f4532}
```

Il dato da copiare è `d041b7b8-7a8f-446a-8eab-b13f43066d65`.

---

Se si verifica il problema di autenticazione non riuscita:

```
default: SSH address: 127.0.0.1:2222
default: SSH username: vagrant
default: SSH auth method: private key
default: Warning: Authentication failure. Retrying...     
default: Warning: Authentication failure. Retrying...
default: Warning: Authentication failure. Retrying...
default: Warning: Authentication failure. Retrying...
default: Warning: Authentication failure. Retrying...
```

La soluzione è [questa](https://stackoverflow.com/a/39590210).
