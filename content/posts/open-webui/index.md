+++
title = "Open WebUI"
date = 2026-10-11
type = "post"
category = "Selfhost"
description = "Rezygnacja z subskrypcji LLM: OpenRouter + własne Open WebUI na Proxmox. Kolejkowanie, internet w LLM, sandbox – jeden interfejs do wszystkiego."
tags = ["AI", "Selfhost"]
rank = 0
draft = false
+++

## TL;DR

Zrezygnowałem z subskrypcji na rzecz API – **OpenRouter** jako API do modeli – i postawiłem własne środowisko do promptowania: **Open WebUI** na LXC (Proxmox). Jedno miejsce dla wszystkich LLM i funkcje, których webowe platformy nie dają: kolejkowanie promptów, notatki, archiwum rozmów, dostęp do internetu (SearXNG + Firecrawl), RAG, sandbox terminalowy i YouTube API. Efekt: pełna kontrola, wyraźny wzrost efektywności pracy z modelami oraz obniżenie kosztów o 71%.

## CEL

Wszystko kręciło się wokół subskrypcji: jeden dostawca, jeden interfejs, miesięczny cykl rozliczenia niezależny od zużycia tokenów – przy zamknięciu się na jeden model LLM, bez możliwości testowania innych, więc skakanie między platformami było czasochłonne i mało efektywne, a każdy kolejny interfejs to kolejna warstwa trudności w postaci trzymania promptów i materiałów na zewnątrz. Do tego dochodziło to, że część dostawców w ogóle nie ma własnej strony do promptowania – udostępniają tylko API – więc duża część LLM była dla mnie niedostępna.

Szukałem rozwiązania czterech problemów:

- odejścia od subskrypcji na rzecz API,
- obniżenia kosztów,
- jednego miejsca do pracy z różnymi LLM,
- wycisnięcia tyle ile się da z LLM bez ograniczeń dostarczanych interfejsów webowych.

## ROZWIĄZANIE

### Platforma API – OpenRouter

Pierwszy krok: jedno miejsce z dostępem do wszystkich LLM, bez przepinania kart między dostawcami. Rozwiązał to w 100% **OpenRouter**. Ważna funkcja: lista LLM z informacją o popularności w danym temacie – szybko oceniam, czy dany model ma sens, filtrując np. po Learn. Dzięki temu mocno zwiększyłem rotację modeli i dużo częściej korzystam z LLM.

Na dodatek śledzę wydatki i wyszukuję modele znacząco tańsze od wiodących dostawców – nie zderzam się z limitami czasowymi przy zachowaniu niskich kosztów.

Efektem ubocznym była nauka większej rozważności w wydawaniu tokenów: nauczyłem się myśleć i planować rozmowę z LLM, co w przypadku subskrypcji określam jako niemożliwe.

### Dlaczego Open WebUI

Platforma OpenRouter od razu mnie nie przekonała, z jednego prostego powodu: brak sensownego GUI. Po co mam zamieniać wygodny interfejs od takiego Claude na rzecz... tego czegoś?

Ale bilans wypadł jasno: jak tylko rozwiążę problem z brakiem GUI, zyski z API przewyższają to, co daje jakakolwiek gotowa platforma.

Padło na **Open WebUI**. Dwa powody: self-host i bardzo dobrze rozpisany roadmap rozwoju projektu – wersja 0.7 na tyle mnie zainteresowała i przekonała, że postanowiłem zaryzykować. Jak się okazało, rozwój tej platformy jest tak potężny, że przy każdej aktualizacji pojawia się coś nowego, wartego uwagi.

### Kolejkowanie promptów

Zabawne jest to, że kiedy opowiadam ludziom, pierwszą reakcją jest lekki uśmieszek w stylu „głupie głupoty". Wcale się nie dziwię, bo brzmi to absurdalnie. Jaki wyróżnik? Kiedy AI mi odpowiada, mogę w kolejce wysłać kolejne zapytanie – zostanie automatycznie i natychmiast wysłane, jak tylko skończy odpowiadać. Odświeżenie przeglądarki nic nie zmienia, nie trzymam z tyłu głowy myśli „muszę to wysłać", a gdy sygnał dźwiękowy informuje mnie o gotowej odpowiedzi, czekam na drugą, bo moje drugie pytanie już leci w kolejce.

Dla mnie to poziom wygody na tyle wysoki, że kiedy zdarza mi się odpalić Gemini, Claude czy ChatGPT w przeglądarce, jestem natychmiast poirytowany, że muszę czekać z wysłaniem promptu, który już mam gotowy.

### Notatki

Drobna rzecz, która wprowadza niesamowitą wygodę. Idea banalna: miejsce, gdzie trzymam swoje notatki – tytuł i tekst. Nic więcej. Magia w dwóch rzeczach: notatkę jednym poleceniem podpinam do rozmowy (AI sobie ją przeczyta) oraz mogę powiedzieć AI, żeby zapisał coś do notatek. Niesamowicie przydatna rzecz.

### Biblioteka promptów

Kolejna funkcja, która nie robi żadnej rewolucji, ale jest zrobiona tak, że po prostu dobrze działa. Ktoś tutaj naprawdę zrobił solidny research, czego potrzebują użytkownicy. Chodzi o miejsce, gdzie przygotowuję prompty i dzielę je na foldery. Nie brzmi niezwykle – takie prompty mogę trzymać w Obsidianie i kopiować. Ale właśnie w tym jest mały szczegół: na telefonie jednym poleceniem podpinam prompta. Proste i genialne.

Totalnym game changerem jest placeholder w promptcie: po wklejeniu (przypominam – jednym skrótem) od razu otwiera się okno graficzne, w którym wpisuję swoje zapytanie. Efekt: gotowy prompt wraz z moim pytaniem.

### Archiwum

Bardzo niepozorna funkcja, którą doceniłem długo później i zacząłem czerpać z niej korzyści przypadkiem. Rozmów z LLM jest bardzo dużo (zwłaszcza przy API), więc naturalne usuwanie starych wątków – z niejasnego, nieprzemyślanego powodu – zamieniłem na archiwizowanie. Ku mojemu zdziwieniu: kazałem LLM poszukać informacji o RAG, o który pytałem kiedyś, on zaczął szukać w archiwum i znalazł! Nagle okazało się, że mam ogromną bazę danych, do której mogę w każdej chwili sięgnąć, a buduję ją nieświadomie – jedyny filtr: do archiwum wrzucam to, co sensowne, a nie 100 zapytań o pogodę. Nie dość, że wszystko jest u mnie lokalnie, to mogę to w każdej chwili eksportować albo poprosić AI o przeszukanie archiwum w poszukiwaniu konkretnej informacji.

### Dostęp do internetu i wyszukiwarka

Kolejny kamień milowy: dowolny (tak, dowolny) LLM zyskuje wzrost inteligencji o około 260% (wiem, bo liczyłem). Konfiguracja: przeczytałem 2 strony dokumentacji i w 30 minut poskładałem całość. Podpinamy dwie rzeczy: silnik do wyszukiwania – lokalnego „google" (SearXNG) oraz silnik do scrapingu stron (Firecrawl). Efekt: LLM nie dość, że może przeszukiwać internet, to jeszcze każdą stronę może wyciągnąć od początku do końca i ją przeczytać – wrzucam linka, on go odpala, analizuje i ściąga wszelkie informacje. W 100% rozwiązuje to problem przeterminowanych informacji.

Przykład z życia: pytałem o rewizję do programu, na początku podał mi złe informacje, więc poprosiłem o weryfikację – dzięki temu ściągnął dokumentację, przeczytał ją i okazało się, że za pierwszym razem czytał ją zbyt pobieżnie. Po zapoznaniu poprawił swój błąd.

Nagle skończyły się problemy z kłótnią kto ma rację, bo AI ma dostęp do całego internetu. Poziom skoku jakości był tak duży, że przy rozmowach ze znajomymi, którzy korzystają z tych samych LLM, mamy zupełnie inne doświadczenia – u mnie nie występowały problemy z „głupim modelem".

### RAG

Funkcja, z której – przyznaję – obecnie, w dobie LightRAG i agentów AI, korzystam praktycznie wcale. Mimo to o niej wspominam, bo dała mi bardzo fajny wstęp do RAG, który określam jako ważną lekcję. Krótko mówiąc: baza danych dla LLM – wrzucam swoje notatki albo dokumentację i chat ma dostęp do dodatkowej wiedzy, szybko i tanio. Ciekawostka: podawanie tekstu jest na tyle lekkie, że można skorzystać z lokalnego LLM bez karty graficznej. Czym jest RAG – odsyłam do mojego artykułu o [LightRAG](/posts/lightrag/), gdzie omawiam wektorową podstawę działania.

### Terminal, czyli jak z chatu zrobić agenta

Obecnie dla mnie coś obowiązkowego: po co LLM ma się męczyć, jak można dać mu piaskownicę do zabawy. Twórcy Open WebUI przygotowali specjalnego dockera, co podpinamy do naszego OWUI – kiedy mówię mu „wygeneruj kod", „sprawdź", „zmień" albo zwyczajnie chcę mu któryś plik wrzucić, ma swoją piaskownicę. Potrafi szybko i sensownie zaprojektować prostą stronę internetową czy podmienić pliki, bo może zainstalować dowolne narzędzie, którego potrzebuje. Nie jest to może jeszcze poziom agenta AI, ale jesteśmy na bardzo mocnym styku. Wisienka: do tego jest interfejs, więc przerzucanie plików, ściąganie czy prosta komenda w dockerze bez wychodzenia z czatu. Główny zysk: kolejne podbicie efektywności pracy z LLM.

### YouTube API – miły dodatek

Nie będę się rozpisywać: po wrzuceniu linka z YouTube wbudowany silnik API wyciąga napisy i od razu podaje do LLM. Ma to swoje humory – zdarza mu się zawiesić i trwać w nieprzerwanej pętli – ale jest to dosłownie wbudowane w Open WebUI, więc jako funkcja zero-konfiguracyjna działa dobrze.

## WNIOSKI

Opisałem wybrane funkcje, które zrobiły na mnie największe wrażenie. Zdaję sobie sprawę, że nie opisałem sporo innych dodatków – zapoznałem się z nimi za mało albo zrezygnowałem, bo (jeszcze) nie widziałem zastosowania. Jeżeli pojawi się coś istotnego, może powstanie vol. 2 jako duży update.

Uwagi techniczne i wnioski:

- mam zainstalowane Open WebUI na LXC w Proxmox (brak gotowego skryptu Proxmox),
- nie korzystałem z aplikacji na PC,
- wszystko, co poruszyłem, łatwo znaleźć na stronie projektu – dokumentacja Open WebUI jest bardzo dobrze opisana,
- obecnie korzystam z DeepSeek Flash & Pro v4.x oraz GLM 5.x,
- coraz częściej korzystam z agentów AI (pi.dev), mimo to przewiduję, że z Open WebUI nie zrezygnuję nigdy.

Niewielkim nakładem sił – wystarczy umieć czytać dokumentację – mamy darmowe środowisko do promptowania z LLM na tak wysokim poziomie, że nasuwa się tylko jedna myśl: „Stop, we have many!".

*Artykuł napisałem w trzecim kwartale 2026 – dane mogą (ale nie muszą) być już przeterminowane. Zdaję sobie sprawę, jak szybki jest rozwój LLM, mimo to podjąłem ryzyko i opisałem moje doświadczenia.*
