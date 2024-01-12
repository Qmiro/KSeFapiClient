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

Użycie aplikacji - praca z fakturami

$typApi - mooże przyjąć jedną z trzech wartości:
* test - Tryb testowy api KSeF korzysta z adresu https://ksef-test.mf.gov.pl/
* demo - Tryb demo api KSeF korzysta z adresu https://ksef-demo.mf.gov.pl/
* public -Tryb produkcyjny api KSeF korzysta z adresu https://ksef.mf.gov.pl/

$db - połączenie z bazą danych w wersji podstawowej aplikacji (v.1.0.0) wykorzystywane jest zwykłe połączenie z użyciem roższerzenia PDO, które powinno być zainstalowane na twoim serwerze

$nip - numer NIP, który by zakłądany na stronie KSeF

$token - token autoryzacyjny, który był zakładany na stronie KSeF musi być on powiązany z podawanym numerem NIP (muszą mu być nadane zapis/odczytu faktur.

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

// Po wykonanym procesie zostanie zwrócony object, na podstawie zwróconych danyhc w bazie apliakcji zostaną zapisane dwie wartości referenceNumber,elementReferenceNumber.
// Na podstawie elementReferenceNumber sprawdzany będzie status faktury w repozytorium KSeF

object(stdClass)#5 (5) {
  ["timestamp"] => string(24) ""
  ["referenceNumber"] => string(36) ""
  ["processingCode"] => int(100)
  ["processingDescription"] => string(30) "Proces został zarejestrowany."
  ["elementReferenceNumber"] => string(36) ""
}

// Sprawdzenie faktury za pomocą elementReferenceNumber zwróconym po wysłaniu
// faktury do repozytorium KSeF
// Po wykonaniu procesu zostanie dodany wpis wartości ksefReferenceNumber do bazy danych

$checkInvoiceStatus = $objKSeFcommand->checkInvoiceStatus($elementReferenceNumber);

object(stdClass)#5 (6) {
  ["timestamp"] => string(24) ""
  ["referenceNumber"] => string(36) ""
  ["processingCode"] => int(200)
  ["processingDescription"] => string(46) "Zakończenie etapu archiwizacji danych faktury"
  ["elementReferenceNumber"] => string(36) ""
  ["invoiceStatus"]=>
    object(stdClass)#4 (3) {
      ["invoiceNumber"] => string(13) ""
      ["ksefReferenceNumber"] => string(35) ""
      ["acquisitionTimestamp"] => string(24) ""
    }
  }

// Fakture jej treść pobieramy za pomocą numeru ksefReferenceNumber. Po prawidłowym wykonaniu procesu zostanie zwrócona struktura XML faktury

$xmlInvoice = $objKSeFcommand->getInvoice($kSeFReferenceNumber);

```

