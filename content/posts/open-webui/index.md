+++
title = "Open WebUI"
date = 2026-10-11
type = "post"
category = "Selfhost"
description = "OpenRouter + Open WebUI na home server. Kolejkowanie, web search dla LLM, sandbox i wiele więcej."
tags = ["AI", "Selfhost"]
rank = 0
draft = false
+++

## TL;DR

Zrezygnowałem z subskrypcji na rzecz API i postawiłem w homelabie własne środowisko do promptowania (**Open WebUI** – LXC – Proxmox). Powód nie był finansowy – był to efekt uboczny. Głównym celem było rozdzielenie trzech warstw – dostawcy, interfejsu i modelu – których subskrypcja nie pozwala dostosować do własnych potrzeb. Efekt: dostęp do wszystkich modeli AI i funkcji, które platformy subskrypcyjne nie dostarczają. Efekt uboczny: koszt niższy o 70%.

## CEL

Wszystko kręciło się wokół subskrypcji: jeden dostawca, jeden interfejs, jeden zestaw modeli i miesięczny cykl rozliczenia niezależny od tego, ile faktycznie zużyłem tokenów. Testowanie różnych modeli oznaczało skakanie między platformami, a każdy kolejny interfejs to inne podejście do pracy – więc prompty i materiały musiałem trzymać na zewnątrz. Do tego część dostawców nie ma własnej strony do promptowania: dostarczają tylko API, więc spora grupa modeli była dla mnie niedostępna.

Chciałem jednego miejsca, w którym mogę pracować z dowolnym modelem bez potrzeby migracji między platformami.

## ROZWIĄZANIE

### Dostęp i koszt

Pierwszym krokiem było rozdzielenie dostępu do reszty. **Openrouter** rozwiązał ten problem poprzez jedno konto i jeden rachunek. Rozwiązało to problem dostępu: każdy model, którego potrzebuję lub chcę przetestować, jest dostępny w tym samym miejscu.

Ważniejsza okazała się inna rzecz, której się nie spodziewałem: ranking modeli w danej kategorii według użytkowników. Zamiast zgadywać, czy nowy model spełni moje oczekiwania, wystarczy, że przejrzę rankingi, aby zobaczyć, na którym miejscu jest dany model pod względem np. kategorii learning albo code. Efekt jest taki, że rotacja modeli wzrosła – testuję znacznie częściej i więcej, bo testowanie przestało być koszmarem.

Drugi skutek jest mniej oczywisty. Przy stałej opłacie tokeny nie mają znaczenia, więc nie ma powodu, aby o nich myśleć. Przy API sprawa wygląda inaczej – każde zapytanie ma swoją cenę, więc trzeba podejmować decyzję: kiedy przerwać rozmowę i rozpocząć nowy wątek, aby nie przepalać niepotrzebnie tokeny. Zacząłem planować rozmowę – co chcę osiągnąć, jaki kontekst jest potrzebny, czy lepiej zapytać raz a porządnie, czy mogę się tym nie przejmować. Brzmi to jak ograniczenie, a okazało się najtrwalszą zmianą w sposobie pracy, którą później wykorzystałem przy pracy z agent AI.

### Interfejs to nie dodatek

**Openrouter** sam w sobie mnie nie przekonał, i to z prostego powodu: nie ma sensownego GUI. Nie miałem powodu zamieniać działającego interfejsu na surową stronę.

Ale bilans był jasny: gdy tylko rozwiążę problem interfejsu, zyski z API przewyższają to, co daje gotowa platforma. Potrzebowałem więc drugiej warstwy – własnego interfejsu.

Padło na **Open WebUI**. Zadecydowały dwa argumenty: self-host i aktywnie rozwijany projekt. Wersja 0.7 była na tyle dobrze rozwinięta, że zdecydowałem się zaryzykować. Okazało się, że był to dobry zakład – każda aktualizacja przynosi na tyle nowości, że w pewnym sensie platforma ma nieograniczone możliwości.

### Efektywność to otoczenie

Najbardziej niedocenianą funkcją jest kolejkowanie. Brzmi jak gadżet – sam się uśmiechałem, kiedy opowiadałem znajomym z pełną powagą, jak ważna jest to funkcja. W praktyce: wysyłam kolejne pytanie, kiedy model jeszcze odpowiada, a on automatycznie tworzy kolejkę i wrzuca to pytanie, gdy zakończy działanie. Nie muszę trzymać w głowie prompta, pamiętać o tym, a przy odświeżeniu przeglądarki nic mi nie zniknie. Przy pracy na platformach od Gemini czy ChatGPT dopiero poczułem, jak brak takiej funkcjonalności jest irytujący.

Notatki i biblioteka promptów robią coś podobnego: trzymają materiały tam, gdzie pracuję. Notatkę podpinam do rozmowy, prompt wywołuję z biblioteki i to z placeholderem, który od razu otwiera okno do uzupełnienia tekstu. Właśnie na takich drobiazgach wszystko się rozstrzyga, bo będzie to narzędzie, którego będę używać każdego dnia.

Osobno wspomnę archiwum, bo okazało się czymś innym, niż myślałem. Zaczynałem od niego jako od sposobu na nieusuwanie rozmów – bez jakiegoś konkretnego powodu. Po czasie, jak zapytałem o narzędzie RAG, a była to rozmowa, którą kiedyś prowadziłem z AI, ku mojemu zaskoczeniu model znalazł taką informację w archiwum. Niezauważenie zbudowałem bazę wiedzy, której nie planowałem.

Na końcu terminal – piaskownica dla AI, w której model może uruchomić kod, podmienić pliki albo zainstalować narzędzie, które potrzebuje do wykonania zadania. To jeszcze nie agent, ale moment, w którym przestajesz działać z kodem na poziomie kopiuj-wklej, a przechodzisz na poziom uruchom-sprawdź. Zmiana jest wyraźnie odczuwalna z powodu dużego spadku błędów i szybkości działania.

### Rozszerzenie wiedzy modelu

Ostatnia zmiana dotyczy podpięcia lokalnej wyszukiwarki (SearXNG) i scrapera, czyli ściąganie całej strony (Firecrawl). Dzięki temu model potrafi znaleźć źródła, przeczytać je i przeanalizować.

Przykład: pytałem o rewizję pewnego programu. Pierwsza odpowiedź była błędna, poprosiłem o weryfikację – model pobrał dokumentację, przeczytał ją i poprawił się. Okazało się, że za pierwszym razem przeczytał ją pobieżnie i dzięki narzędziom mógł się poprawić.

Efekt był taki, że model przestał się mylić, co doprowadziło do efektu, gdzie przestałem się kłócić z modelem „kto ma rację" i przyjemność pracy z modelami znacznie wzrosła.

### Cała reszta

Dwie funkcje wymieniam tylko dlatego, że wypada je wymienić. RAG znam, ale w dobie LightRAG i agentów praktycznie z niego nie korzystam – dał mi jednak wstęp do tematu. YouTube API jest wbudowane i działa bez konfiguracji, czasami zdarza mu się zaciąć, ale i tak jest niezłe.

## PODSUMOWANIE

Opisałem wybrane funkcje, które zrobiły na mnie największe wrażenie. Zdaję sobie sprawę, że nie opisałem sporo innych dodatków – zapoznałem się z nimi za mało albo zrezygnowałem, bo (jeszcze) nie widziałem zastosowania. Jeżeli pojawi się coś istotnego, może powstanie vol. 2 jako duży update.

Uwagi techniczne i wnioski:

- mam zainstalowane **Open WebUI** na LXC w Proxmox (brak gotowego skryptu Proxmox),
- nie korzystałem z aplikacji na PC,
- wszystko, co poruszyłem, łatwo znaleźć na stronie projektu – dokumentacja Open WebUI jest bardzo dobrze opisana,
- obecnie korzystam z DeepSeek Flash & Pro v4.x oraz GLM 5.x,
- coraz częściej korzystam z agentów AI (pi.dev), mimo to przewiduję, że z Open WebUI nie zrezygnuję nigdy.

Niewielkim nakładem sił – wystarczy umieć czytać dokumentację – mamy darmowe środowisko do promptowania z LLM na tak wysokim poziomie, że nasuwa się tylko jedna myśl: „Stop, we have many!".

*Artykuł napisałem w trzecim kwartale 2026 – dane mogą (ale nie muszą) być już przeterminowane. Zdaję sobie sprawę, jak szybki jest rozwój LLM, mimo to podjąłem ryzyko i opisałem moje doświadczenia.*
