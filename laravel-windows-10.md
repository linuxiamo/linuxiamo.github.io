# Installare Laravel+Homestead su Windows 10

Informazioni tratte da [qui](https://medium.com/@eaimanshoshi/i-am-going-to-write-down-step-by-step-procedure-to-setup-homestead-for-laravel-5-2-17491a423aa)

## Prerequisiti

* Virtualizzazione attiva nel bios del computer
* [Download Git Bash](https://git-scm.com/download/win)
* [Download Virtualbox](https://www.virtualbox.org/wiki/Downloads)
* [Download Vagrant](https://www.vagrantup.com/downloads.html)
* [Download Visual Studio Code](), opzionale

## Installazione

Le nostre operazioni prevedono di installare tutto in `C:\Progetti`. Homestead sarà installato in `_homestead`. Le chiavi crittografiche in `__rsa_keys`. 
Installare Git Bash, Virtualbox e Vagrant in quest'ordine.

### a) Creare la Vagrant VM

Aprire _git bash_ come amministratore e lanciare:

```bash
vagrant box add laravel/homestead
```

Il programma richiederà quale versione installare in base al programma di virtualizzazione da usare.

In caso di errore, installare [MS Visual C++ 2010 x86 Redistributables](https://www.microsoft.com/en-us/download/confirmation.aspx?id=5555) e ripetere il comando in git bash.

### b) Clonare homestead sul computer

Clonare homestead su `c:\progetti` su _git bash_:

```bash
cd /c/progetti
git clone https://github.com/laravel/homestead.git _homestead
```

### c) Generare le chiavi crittografiche

Ora bisogna generare le chiavi crittografiche per accedere ad homestead. Da _git bash_:
```bash
cd /c/progetti
mkdir _rsa_keys
cd _rsa_keys
ssh-keygen -t rsa -C “your_email@example.com”
```

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
```

Così (ricordarsi la minuscola per la lettera dell'unità):

```
authorize: c:/Progetti/__rsa_keys/id_rsa.pub

keys:
    - c:/Progetti/__rsa_keys/id_rsa

folders:
    - map: c:/Progetti
      to: /home/vagrant/code
```


_ _ _
## Alternative

Tutorial [Homestead improved](https://www.sitepoint.com/quick-tip-get-homestead-vagrant-vm-running/)