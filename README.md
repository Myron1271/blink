
# Blink Update Process Selfhosted

Blink Thema Updater (Selfhosted)

## Beschrijving

In deze POC wordt het Blink Thema up to date gehouden via de plugin: [plugin-update-checker](https://github.com/YahnisElsts/plugin-update-checker).

Via het bestand [theme.json](https://github.com/Myron1271/blink/blob/master/theme.json) wordt de versie bijgehouden. Als de "version" in style.css van het Blink Thema in het lokale project lager is dan de "version" in theme.json, zal Wordpress in het dashboard aangeven dat er een update beschikbaar is.

***Let op!**: deze installatie gaat ervan uit dat er al een Lokale Wordpress Environment is opgezet en dat het Thema al is toegevoegd.*

* Link voor het opzetten van WordPress: [Local WordPress Setup](https://jetpack.com/resources/wordpress-localhost/).

## Getting Started

### Dependencies

Hoewel de plugin-update-checker niet geïnstalleerd zal worden als Dependency is deze wel nodig voor dit project:

[plugin-update-checker](https://github.com/YahnisElsts/plugin-update-checker).

### Voor Installatie

Zoals eerder beschreven maakt deze POC gebruik van een theme.json. In dit JSON bestand wordt de versie bijgehouden en wordt verwezen naar de download locatie van het thema.

Deze theme.json moet geplaatst worden in de root van een git repository, met daarin de volgende code:

    {  
      "version": "1.0.0", //Huidige versie (moet gelijk zijn aan de tag van de release in Github)  
      "details_url": "https://example.com/version-2.0-details.html", // Geeft de pagina op die de gebruiker te zien krijgt wanneer hij op de link "Bekijk details van versie 1.0.0" klikt in een update-melding in Wordpress.   
      "download_url": "https://example.com/example-theme-1.0.0.zip" // De link van de laatste versie van het Wordpress thema gehost wordt. 
    }

De .zip in de "download_url" kan overal gehost worden, zolang update-checker er maar toegang tot heeft.

In deze demo gebruik ik Github voor het Selfhost gedeelte. Dit betekent dat in deze repository niet alleen het bestand theme.json staat, maar ook de map /Blink met daarin alle bestanden van dat thema.

### Installatie

- Begin met het verkrijgen van de laatste versie van de [plugin-update-checker releases](https://github.com/YahnisElsts/plugin-update-checker/releases).
- Kopieer de uitgepakte .zip naar de plugins folder in Wordpress.
- Voor deze POC zal de volgende code in de functions.php van het Blink thema gezet worden, deze code hoeft niet perse in de functions.php, maar raad ik wel aan.

```
<?php  
        /**  
         * Plugin Name: Blink Theme Updater * Description: Automatische updates voor het Blink-theme via GitHub. * Version: 1.0.0 */  
        require plugin_dir_path(__FILE__) . 'plugin-update-checker/plugin-update-checker.php';  
          
        use YahnisElsts\PluginUpdateChecker\v5\PucFactory;  
          
        // Theme update via GitHub  
        $updateChecker = PucFactory::buildUpdateChecker(  
            'https://example.com/path/to/details.json',  
            get_theme_root() . '/Blink', 'Blink'// Het pad naar het Thema met de naam "Blink".
        );
```

De installatie van de plugin is nu afgerond. Om het Wordpress thema klaar te maken voor toekomstige updates,
moeten we het bestand style.css aanpassen.
Dit doen we door in de style.css in de root van het thema het volgende toe te voegen:
```
/*!  
Version: 1.0.0 // Moet gelijk zijn met de versie in de theme.json  
*/
```

De versie in style.css wordt nu vergeleken met de versie in theme.json. Als deze lager is, zal Wordpress aangeven dat er een update beschikbaar is.

**Met dit is de installatie klaar.**

### Updates uitbrengen

Voor het updaten van het thema wordt in deze POC gebruikgemaakt van Github om het thema te hosten. 
Daarbij worden tags en releases gebruikt om aan te geven wat de nieuwste versie is. 
Het aanmaken van tags en releases is niet nodig als het thema op een andere locatie wordt gehost.

**Voor het updaten van dit Thema zijn dan de volgende stappen nodig.**

- Na het maken van aanpassingen aan het thema moet het versienummer in theme.json worden verhoogd, bijvoorbeeld van `1.0.0` naar `1.0.1`.
- Dit zelfde moet ook gebeuren in de style.css
- Zodra dit is gedaan, moet er een .zip bestand worden gemaakt van het (geupdaten) Wordpress thema en geüpload naar de locatie waar het wordt gehost. In deze demo is dat Github.
Voor deze demo moeten we een nieuwe tag en release aanmaken. Bij het aanmaken van een nieuwe release uploaden we het .zip bestand van het thema onder de sectie "Attach binaries by dropping them here or selecting them."

Als iemand nu een oudere versie van het thema heeft, zal er in Wordpress een melding verschijnen met de mogelijkheid om het thema te updaten. Zie de afbeelding als voorbeeld:
<img width="1007" alt="Blink-ReadMe-Selfhosted" src="https://github.com/user-attachments/assets/491a2c66-da70-44db-8023-18800d12e7dd" />

## Auteurs

[Myron Seelen LinkedIn](https://www.linkedin.com/in/myron-seelen/)

## Versies

*Check releases tab voor version history:*
[Versies](https://github.com/Myron1271/blink/releases).



