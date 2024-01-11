# KSeFapiClient

Krajowy Systemu e-Faktur

* Wersja projektu 1.0.0
* Wymagana wersja PHP >= 7.0.0
* Baza danych MySQL
* Do tworzeniu dokumentów XML Projekt używa funkcji xmlwriter.
* Do walidacji poprawnie utworzonego dokumentów XML Projekt używa DOMDocument.

Celem projektu jest stworzenie klienta API KSeF w technologi PHP, z wykorzystaniem wywołań http i serializacji JSON. Obecna wersja klienta wspierwa wyłącznie scheme FA (2)

#### Baza danych

W bazie danych będziesz potrzebować następujących tabel

| tabela   	       | opis   	                                                           |
|-----------------|---------------------------------------------------------------------------|
| `ksef_api_communication_errors` 	     | 	logowanie błędów procesów zadrzeń  wykonywanych w repozytorium KSeF z wykorzystaniem procesów API                  | 
| `ksef_api_contracts`	 | 	dane dostępu do API KSeF |
| `ksef_invoices`	     | 	wysłane / pobrane faktury z repozytorium KSeF                                          |
| `ksef_invoice_processing`	     | 	procesy wysyłania faktur do repozytorium KSeF                                          |

Aktualna wersja projektu pozwala na następujące działania:

* Tworzenie dokumentu faktury w formie pliku XML zgodnie ze schemą KSeF FA (2). Obsługiwane dokumenty faktur: zakupowa, sprzedaży oraz krekty
* Walidacja struktury faktury pod kątem synematycznym
* Wysłanie faktury do repozytorium KSeF
* Sprawdzenie statusu procesowania faktury w repozytorium KSeF
* Pobranie jednej lub wielu faktur z repozytorium KSeF
* Pobranie UPO faktur z repozytorium KSeF

Wszystkie procesy odbywają się w sesji interaktywnej. Wymagany jest token autoryzacyjny dodany w panelu KSeF.  

Użycie aplikacji - wysłanie faktury do repozytorium KSeF

Użycie walidatora dla pełneo mumeru

$typApi - mooże przyjąć jedną z trzech wartości:
* test - Tryb testowy api KSeF korzysta z adresu https://ksef-test.mf.gov.pl/
* demo - Tryb demo api KSeF korzysta z adresu https://ksef-demo.mf.gov.pl/
* public -Tryb produkcyjny api KSeF korzysta z adresu https://ksef.mf.gov.pl/

```php
<?php

declare(strict_types = 1);

chdir(dirname(__DIR__));
chdir(__DIR__);
define('BASE_PATH', realpath(__DIR__));

include 'KSeFcommand.php';

objKSeFcommand = new KSeFcommand();

$objKSeFcommand->addConnectionDb($db);
$objKSeFcommand->addBasePath(BASE_PATH);

$objKSeFcommand->connectionApi($nip, $token, $typApi);
$xml = 'twojafaktura.xml';
$sendInvoice = $objKSeFcommand->sendInvoice($xml);
```

