+++
title = "LightRAG"
date = 2026-07-27
type = "post"
description = "Kiedy potrzebujesz nauczyciela, ale chcesz dać mu książkę, o której chcesz z nim porozmawiać."
tags = ["self-host", "rag", "ai", "lightrag", "graphrag"]
draft = false
+++

Podczas szukania rozwiązania na efektywniejszą naukę trafiłem na Reddicie na opis _"How to Build Karpathy's LLM Wiki"_ – czyli jak połączyć dwie rzeczy: swoją zgromadzoną wiedzę z AI.

Niestety po głębszej analizie okazało się, że jest to rozwiązanie drogie i nie podobało mi się przepalanie tokenów na każdym pytaniu do moich notatek. W ten sposób trafiłem na koncepcję **RAG**, a później **GraphRAG**, który finalnie zaprowadził mnie do uruchomienia **LightRAG + RAG-Everything**.

Wszystko zaczęło się od przeczytania świetnego artykułu od swistak.codes ([Podobieństwo wektorów](https://swistak.codes/post/podobienstwo-wektorow/)). Zrozumiałem, czym jest RAG – i jest to super prosta sprawa w swojej niesamowitej skomplikowalności.
RAG zamienia cały tekst na tablice z numerkami (i to takimi naprawdę wielowymiarowymi). Gdy rozmawiamy z AI, a ono zapyta RAG: _"Co to jest VLAN?"_, RAG dostaje numerek `101`, szuka dopasowania i widzi, że: **definicja VLAN → 100**, **definicja inter-VLAN → 105** – i te dwie dane podrzuca AI.
**W teorii super.** W praktyce był dla mnie problem: takie tematy nigdy nie są takie proste. Przecież VLAN łączy się z inter-VLAN, potem z SVI, ROAS, routingiem – i tak można bez końca. A RAG nie potrafi podać tych połączeń.
I tu dochodzimy do **GraphRAG** – który różni się od RAG tym, że tworzy właśnie te połączenia jako jedną wielką siatkę danych.

Jak wygląda uruchomienie takiego LightRAG? Super prosto: postawienie lightRAG w dokerze, dodajemy dodatek RAG-Everything, odpalamy web GUI, wrzucamy pliki, czekamy 2-3 dni i gotowe.
Teraz, gdy rozmawiam z AI (w moim przypadku **DeepSeek v4 Pro** przez API), model cały czas odwołuje się do książki – do tego stopnia, że jeśli czegoś w niej nie znajduje, to wprost mi o tym mówi.
A jak kosztowo? **7 dolarów** za 1200 stron książki Cisco. Nie jest źle – ale można obniżyć koszty jeszcze bardziej, korzystając z darmowych API. Wtedy co prawda trwa to ~3 razy dłużej, ale zawsze parę dolarów w kieszeni. 😊

Gdzie to zastosowałem w praktyce?
- **PI Historian (OSIsoft)** – wrzucenie ~300 plików dokumentacji, aby pomogła mi ogarnąć skomplikowane koncepcje, struktury itp.
- **Książka Cisco** – mój asystent do nauki, który w 98% wyciąga wiedzę z książki, nie halucynując.
- **Książka _The Great Mental Models_** – zamiast czytać trudne idee, podawałem mu przykłady z mojego życia, aby odniósł się do treści książki.

Podsumowując:
Dla mnie jest to wyższy poziom wiedzy, który jeszcze niedawno był nie do pomyślenia. Teraz nie umiem sobie wyobrazić nauki bez tego – moja produktywność rośnie do sześcianu.

## Uwagi techniczne i wnioski

- **Warto korzystać z darmowego API** – czas przetwarzania jest długi, często pojawiają się timeouty, ale finalnie wszystko dochodzi do skutku. Czas budowania jest ~4× dłuższy niż przy płatnym API, ale bardzo mocno obniża to koszty.
- **DeepSeek v4 Pro** – najlepiej sprawdza się przy rozmowach.
- **Przy cięciu i obróbce tekstu nie warto oszczędzać** – tu zostawiam ChatGPT. Natomiast do samego przetwarzania dokumentów spokojnie wystarcza darmowe API.
- **Przy zadawaniu pytań warto używać słów kluczowych** – model lepiej rozumie zależności. Mniej ogólnych opisów, więcej konkretnych terminów.
- Świetny artykuł do zrozumienia baz wektorowych: [https://swistak.codes/post/podobienstwo-wektorow/](https://swistak.codes/post/podobienstwo-wektorow/)
- **Polecam do nauki książek mocno technicznych i bardzo szczegółowych** – oraz książek rozwijających kompetencje zarządcze. Można wtedy omówić własny przykład życiowy i bardzo szybko uzyskać indywidualną interpretację dla siebie.
- **LightRAG (GraphRAG) jest tym, czego potrzebuję.** RAG, MCP, agenci AI – nie spełniają moich oczekiwań. GraphRAG – tak.