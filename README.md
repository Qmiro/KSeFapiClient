# QMA - Components KSeF

Komponet rozszerzający aplikację o możliowść przesyłanie faktur do **Krajowego System e-Faktur (KSeF)**. Komponent wspiera faktury ze schema **Fa (2)**.

* Wersja komponentu 1.0.0
* Wymagana wersja PHP >= 8.1.0
* Baza danych PostgreSQL
* Do tworzeniu dokumentów XML Projekt używa funkcji xmlwriter.
* Do walidacji poprawnie utworzonego dokumentów XML Projekt używa DOMDocument.
* Wymagana wersja QMA - DPA >= v.2.1.0

Komponent aplikacji jest dodawany z poziomu panelu administracyjnego aplikacji **QMA - DPA Dedykowana Platforma Aplikacji v.2.x.x**. Do automatyzacji procesów przesłanie faktury, pobranie UPO wykorzystywany jest mechanizm kolejkowania oraz zadań cyklicznych **Cron**. Wszystkie procesy wysłki do **KSeF** odbywają się w sesji interaktywnej. Wymagany jest token autoryzacyjny dodany w panelu **KSeF Ministerstwa Finansów** oraz klucze dla wybranego jednego z 3 środowisk. Do tekena w wybranym śrowowisku **KSeF** przypisywany jest numer **NIP** podatnika te dwie wartości tworzą tzw kontrakt, który jest dodawany w naszej **Aplikacji QMA**. Dla dodanego numeru **NIP** muszą mu być nadane zapisu / odczytu faktur.

Ministerstwo Finansów udostępnia następujące środowiska:
* test - Tryb testowy api KSeF korzysta z adresu https://ksef-test.mf.gov.pl/
* demo - Tryb demo api KSeF korzysta z adresu https://ksef-demo.mf.gov.pl/
* public -Tryb produkcyjny api KSeF korzysta z adresu https://ksef.mf.gov.pl/

Po stronie aplikacji wszystkie dane procesów są przechowywane w bazie danych.
| tabela / widok  	       | opis   	                                                           |
|-----------------|---------------------------------------------------------------------------|
| `ksef_api_communication_errors` | logowanie błędów komunikacji dla procesów zadrzeń  wykonywanych w repozytorium KSeF z wykorzystaniem API  |
| `ksef_api_contracts` | dane dostępu do API KSeF (tzw. kontrakty KSeF) |
| `ksef_api_worker_job_invoices` | worker zdań wysyłanai faktur / korekt do repozytoium KSeF|
| `ksef_invoice_processing` | procesy wysyłania faktur do repozytorium KSeF |
| `ksef_invoice_processing_history` | historia procesów wysyłanych faktur do repozytorium KSeF |
| `ksef_invoice_upo` | dane UPO przesłanych faktur do repozytorium KSeF |
| `ksef_invoices` | numery wysłanych faktur / korekt do repozytorium KSeF |
| `ksef_sessions` | zarejestrowane sesje komunikacji z repozytorium KSeF |
| `view_ksef_api_communication_errors` | widok prezentujący dane błędów komunikacji dla procesów zdarzeń  wykonywanych w repozytorium KSeF z wykorzystaniem API |
| `view_ksef_invoice_processing` | widok prezentujący dane procesów wysyłania faktur do repozytorium KSeF | 
| `view_ksef_invoice_processing_history` | widok prezentujący dane historii procesów wysyłanych faktur do repozytorium KSeF |
| `view_ksef_invoice_upo` | widok prezentujący dane pobranych UPO przesłanych faktur do repozytorium KSeF |
| `view_ksef_invoices` | widok prezentujący dane numery wysłanych faktur / korekt do repozytorium KSeF |
| `view_ksef_sessions` | widok prezentujacy dane zarejestrowanych sesje komunikacji z repozytorium KSeF |

Komendy zadań cyklicznych Cron dla procesów KSeF
| nazwa polecenia  	       | opis   	                                                           |
|-----------------|---------------------------------------------------------------------------|
| `package:ksef-invoice-send` | komenda wyysłajacą nową fakturę repozytorium KSeF |
| `package:ksef-invoices-resend` | komenda ponawiająca wyysłkę fakturę repozytorium KSeF (np. niepowodzenie poprzedniej wysyłki) |
| `package:ksef-invoices-upo` | komenda poberajaca UPO wysłanych faktur / korekt do repozytorium KSeF |

Czasy uruchamiania poszczególnych zadadań są ustawiane w panelu administracyjnym **QMA - DPA Dedykowana Platforma Aplikacji v.2.x.x**.

