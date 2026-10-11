+++
title = "LightRAG"
date = 2026-07-27
type = "post"
category = "Selfhost"
description = "Kiedy potrzebujesz nauczyciela, ale chcesz dać mu książkę, o której chcesz z nim porozmawiać."
tags = ["AI", "Selfhost"]
rank = 40
draft = false
+++

## TL;DR

Chciałem dać AI pełny dostęp do wiedzy z książek, które dostarczę. Potrzebowałem tego do nauki i pomocy przy analizie dokumentacji technicznych. Wybrałem **LightRAG** z dodatkiem **RAG-Everything**, a jako LLM – **DeepSeek v4** przez API. Efekt bardzo zadowalający, koszt to **7 dolarów za 1200 stron**. Korzystałem z tego przez długi czas – do momentu, w którym wszedłem w agentów AI.

## CEL

Szukałem sposobu na wykorzystanie pełnej mocy AI przy czytaniu książki. Na Reddicie trafiłem na opis _„How to Build Karpathy's LLM Wiki"_ – czyli pomysł, żeby połączyć AI ze swoją zgromadzoną wiedzą. Po głębszej analizie odpadło: rozwiązanie drogie, a przepalanie tokenów przy każdym pytaniu do notatek nie podobało mi się. Tak trafiłem na koncepcję **RAG**, co zaprowadziło mnie do **GraphRAG**. Szukając rozwiązania pod ten temat wybrałem **LightRAG** wraz z dodatkiem **RAG-Everything**.

## ROZWIĄZANIE

### Czym różni się RAG od GraphRAG

Zaczeło się od przeczytania świetnego artykułu od swistak.codes ([Podobieństwo wektorów](https://swistak.codes/post/podobienstwo-wektorow/)) – to on mi wytłumaczył, czym w ogóle jest RAG. Jest to super prosta sprawa w swojej niesamowitej skomplikowalności:

RAG zamienia cały tekst na tablice z numerkami (i to takimi naprawdę wielowymiarowymi). Gdy rozmawiamy z AI, a ono zapyta RAG: _„Co to jest VLAN?"_, RAG dostaje numerek `101`, szuka dopasowania i widzi, że: **definicja VLAN → 100**, **definicja inter-VLAN → 105** – i te dwie dane podrzuca AI.

**W teorii super.** W praktyce był dla mnie problem: takie tematy nigdy nie są takie proste. Przecież VLAN łączy się z inter-VLAN, potem z SVI, ROAS, routingiem – i tak można bez końca. A RAG nie potrafi podać tych połączeń.

I tu dochodzimy do **GraphRAG** – różni się od RAG tym, że tworzy właśnie te połączenia jako jedną wielką siatkę danych.

### Uruchomienie

Super prosto: postawienie LightRAG w dokerze, dokładamy dodatek RAG-Everything, odpalamy web GUI, wrzucamy pliki, czekamy 2-3 dni i gotowe.

### Praca na co dzień

Rozmawiam z AI (w moim przypadku **DeepSeek v4 Pro** przez API) – model cały czas odwołuje się do książki. Do tego stopnia, że jeśli czegoś w niej nie znajduje, to mówi mi to wprost.

### Koszty

7 dolarów za 1200 stron. Nie jest źle – a można jeszcze taniej, korzystając z darmowych API. Wtedy co prawda trwa to ~3 razy dłużej, ale zawsze parę dolarów zostaje w kieszeni. 😊

### Gdzie to zastosowałem w praktyce

- **PI Historian (OSIsoft)** – wrzucenie ~300 plików dokumentacji, aby pomogła mi ogarnąć skomplikowane koncepcje, struktury itp.
- **Książka Cisco** – mój asystent do nauki, który w 98% wyciąga wiedzę z książki, nie halucynując.
- **Książka _The Great Mental Models_** – zamiast czytać trudne idee, podawałem mu przykłady z mojego życia, aby odniósł się do treści książki.

## WNIOSKI

Dla mnie jest to wyższy poziom wiedzy, który jeszcze niedawno był nie do pomyślenia. Teraz nie umiem sobie wyobrazić nauki bez tego – moja produktywność rośnie do sześcianu.

- **Warto korzystać z darmowego API** – czas przetwarzania jest długi, często pojawiają się timeouty, ale finalnie wszystko dochodzi do skutku. Czas budowania jest ~4× dłuższy niż przy płatnym API, ale bardzo mocno obniża to koszty.
- **DeepSeek v4 Pro** – najlepiej sprawdza się przy rozmowach.
- **Przy cięciu i obróbce tekstu nie warto oszczędzać** – tu zostawiam ChatGPT. Natomiast do samego przetwarzania dokumentów spokojnie wystarcza darmowe API.
- **Przy zadawaniu pytań warto używać słów kluczowych** – model lepiej rozumie zależności. Mniej ogólnych opisów, więcej konkretnych terminów.
- **Bazy wektorowe:** świetny artykuł, który wyjaśnił mi wektory na prostym przykładzie: [Podobieństwo wektorów](https://swistak.codes/post/podobienstwo-wektorow/) (swistak.codes – linkowany wyżej przy RAG).
- **Polecam do nauki książek mocno technicznych i bardzo szczegółowych** – oraz książek rozwijających kompetencje zarządcze. Można wtedy omówić własny przykład życiowy i bardzo szybko uzyskać indywidualną interpretację dla siebie.
- **LightRAG (GraphRAG) jest tym, czego potrzebuję.** RAG, MCP, agenci AI – nie spełniają moich oczekiwań. GraphRAG – tak.
