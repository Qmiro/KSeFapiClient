# KSeFapiClient

Krajowy Systemu e-Faktur

Celem projektu jest stworzenie klienta API KSeF w technologi PHP, z wykorzystaniem wywołań http i serializacji JSON. Obecna wersja klienta wspierwa wyłącznie scheme FA (2)

#### Bazza danych

W bazie danych będziesz potrzebować następujących tabel

| tabela   	       | opis   	                                                           |
|-----------------|---------------------------------------------------------------------------|
| `ksef_api_communication_errors` 	     | 	logowanie błędów procesów zadrzeń  wykonywanych w repozytorium KSeF z wykorzystaniem procesów API                  | 
| `ksef_api_contracts`	 | 	dane dostępu do API KSeF |
| `ksef_invoices`	     | 	wysłane / pobrane faktury z repozytorium KSeF                                          |
| `ksef_invoice_processing`	     | 	procesy wysyłania faktur do repozytorium KSeF      
