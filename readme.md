# Envoyer des mails avec WAMP
Envoyer des mails en php depuis wamp est impossible de base.
Il faut pour cela créer un (faux) serveur de mail, pour que la fonction [`mail()`](https://www.php.net/manual/fr/function.mail.php) native de php puisse marcher.

## Installer Sendmail  

1. Installer un émulateur d'envoi, qui permet d'envoyer les mails grâce à une adresse existante :

	- Il faut télécharger ce zip : https://github.com/sendmail-tls1-2/main

	- Je mettrai le zip directement dans ce repo.
	
	ensuite, faire comme la doc le dit :

	1. Dézipper le dossier dans c://wamp/sendmail/
	2. Modifier le fichier sendmail.ini comme suit :

    ```
		[sendmail] 
		smtp_server=Adresse.serveur.fr
		smtp_port=587
		smtp_ssl=tls
		default_domain=Domaine.fr
		error_logfile=error.log
		auth_username=adresse@mail.fr
		auth_password=MotdePasse
		pop3_server= 
		pop3_username= 
		pop3_password= 
		force_sender=adresse@mail.fr
		force_recipient= 
		hostname=
    ```
	3. Ensuite, cliquer sur l'icone de wamp, -> PHP -> php.ini.\
    *Vous pouvez voir les différentes versions de PHP que vous utilisez en cliquant sur* wamp -> PHP -> Afficher l'utilisation des versions PHP :
    ![alt text](img/image.png)
    Vous modifierez les différents `php.ini` comme suit :
    
    ```
                [mail function]
                ; For Win32 only.
                ; http://php.net/smtp
    commenter	; SMTP = localhost
                ; http://php.net/smtp-port
    commenter	; smtp_port = 25

                ; For Win32 only.
                ; http://php.net/sendmail-from
    commenter	; sendmail_from ="admin@wampserver.invalid"

                ; For Unix only.  You may supply arguments as well (default: "sendmail -t -i").
                ; http://php.net/sendmail-path
    décommenter	sendmail_path = "C:\wamp64\sendmail\sendmail.exe -t -i"

                ; Force the addition of the specified parameters to be passed as extra parameters
                ; to the sendmail binary. These parameters will always replace the value of
                ; the 5th parameter to mail().
                ; mail.force_extra_parameters =

                ; Add X-PHP-Originating-Script: that will include uid of the script followed by the filename
    mettre à On	mail.add_x_header = On

                ; The path to a log file that will log all mail() calls. Log entries include
                ; the full path of the script, line number, To address and headers.
                ; mail.log =
                ; Log mail to syslog (Event Log on Windows).
                ; mail.log = syslog
    ```

	4. Relancer ensuite wamp.

## tester un envoi mail 
```php
<?php
$to      = 'Email@destinataire.fr';
$subject = 'le sujet';
$message = 'Bonjour ! ça fonctionne !';
$headers = 'From: email@envoi.fr' . "\r\n" .
'Reply-To: email@envoi.fr' . "\r\n" .
'X-Mailer: PHP/' . phpversion();

$test = mail($to, $subject, $message, $headers);

if ($test) {
  echo "le mail a bien été envoyé.";
} else{
  var_dump($test); // reverra la valeur de la fonction mail, probablement false. Aller voir dans ce cas le fichier error.log dans C://wamp/sendmail/
}
?>
```

**SI ÇA NE MARCHE TOUJOURS PAS :** \
Aller dans `C:\wamp64\bin\php\php__LA_VERSION_DE_PHP_QU'ON_UTILISE__`
Et faire les mêmes modifs que ci-dessus dans le php.ini.

## AUTRES MÉTHODES 
il doit pouvoir exister d'autres méthodes, mais je ne les ai pas testées :
- https://github.com/rnwood/smtp4dev // https://mailosaur.com/blog/a-guide-to-smtp4dev/
- https://github.com/ChangemakerStudios/Papercut-SMTP

