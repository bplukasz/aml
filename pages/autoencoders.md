[Strona główna](../README.md)
# Autoenkodery
Autoenkoder jest rodzajem sieci neuronowej, której zadaniem jest wytworzenie reprezentacji danych wejściowych.
Architekturą przypominają standardową sieć neuronową ze sprzężeniem zwrotnym (z tego względu możemy stosować takie same techniki uczenia się jak np. propagacja wsteczna), ale ich cel różni się.
Patrząc głębiej, przekształca on dane wejściowe w reprezentację przejściową, a następnie na podstawie tej reprezentacji przejściowej próbuje odtworzyć wynik, który przypomina dane wejściowe.
Ze względu na to, że w procesie uczenia nie potrzebuje etykiet, to proces ten może przebiegać w sposób nienadzorowany.

![Architektura podstawowego autoenkodera [1]](../resources/autoencoder-model.png)  
*Architektura podstawowego autoenkodera [1]*

Podstawowy autoenkoder składa się z trzech zasadniczych części:
* Koder (ang. encoder) – jego zadaniem jest zakodowanie informacji wejściowych do postaci reprezentacji o zmniejszonym wymiarze.
* Zakodowany wektor – jest to warstwa ukryta w samym środku architektury, nazywana również kodem. Jest wynikiem kodowania.
* Dekoder (ang. decoder) – ma za zadanie przekształcić zakodowany wektor do postaci, która jest jak najbliższa wektorowi wejściowemu sieci.

```
𝑥 − 𝑤𝑒𝑘𝑡𝑜𝑟 𝑤𝑒𝑗ś𝑐𝑖𝑜𝑤𝑦
𝑓(𝑥) − 𝑓𝑢𝑛𝑘𝑐𝑗𝑎 𝑘𝑜𝑑𝑒𝑟𝑎
h = 𝑓(𝑥) − 𝑧𝑎𝑘𝑜𝑑𝑜𝑤𝑎𝑛𝑦 𝑤𝑒𝑘𝑡𝑜𝑟
𝑔(h) = 𝑔(𝑓(𝑥)) − 𝑓𝑢𝑛𝑘𝑐𝑗𝑎 𝑑𝑒𝑘𝑜𝑑𝑒𝑟𝑎
𝑦 = 𝑔(h) − 𝑤𝑒𝑘𝑡𝑜𝑟 𝑤𝑦𝑗ś𝑐𝑖𝑜𝑤𝑦 (𝑧𝑤𝑎𝑛𝑦 𝑟𝑒𝑘𝑜𝑛𝑠𝑡𝑟𝑢𝑘𝑐𝑗ą)
𝑥≈𝑦
```

## Zastosowania autoenkoderów
Architektura autoenkoderów wydaje się być prosta, ale ich zastosowania wbrew pozorom są bardzo szerokie [1]:
* Redukcja wymiarowości – podstawowe zastosowanie autoenkoderów. Jeśli warstwa z zakodowanym wektorem posiada mniej wymiarów niż warstwa wejściowa, to otrzymujemy reprezentację danych wejściowych ze zredukowaną liczbą wymiarów. W takim przypadku możemy mówić o kompresji danych.
* Generowanie nowych wymiarów – jeśli wektor reprezentacji będzie miał więcej wymiarów niż wektor wejściowy możemy uzyskać nowe cechy reprezentujące dane wejściowe.
* Wykrywanie anomalii – anomalie występują rzadko w zbiorze danych, więc autoenkoder powinien mieć problemy z rekonstrukcją próbki, która wykracza poza rozkład danych trenujących. Porównując wartości funkcji straty autoenkodera możemy łatwo wykryć próbkę, której nie udało się dostatecznie dobrze zrekonstruować.
* Generowanie danych przypominających dane wejściowe – możemy to osiągnąć dodając losowe szumy do środkowej warstwy autoenkodera czyli modyfikując zakodowany wektor.
* Redukcja szumów – wykorzystując autoenkodery odszumiające (których architektura jest opisana poniżej) jesteśmy wstanie redukować szumy i różnego rodzaju zakłócenia danych wejściowych. Może to zostać wykorzystane jako jedna z metod obrony przed antagonistycznymi przykładami.

## Rodzaje autoenkoderów

### Głębokie
Składają się z wielu ukrytych warstw (w części kodera i dekodera) w celu uzyskania bardziej złożonych kodowań czy wyłapywania skomplikowanych zależności z danych wejściowych.

### Rzadkie (ang. sparse autoencoders)
Tego rodzaju autoenkodery nakładają ograniczenia na funkcje aktywacji w ukrytych warstwach sieci. W ten sposób unikane jest nadmierne dopasowanie, a także poprawiana jest odporność na ataki antagonistyczne. Pozwalają one na aktywację jedynie niewielkiej liczby neuronów w tym samym czasie (przykładowo 5%). Dzięki takiemu podejściu poziom rekonstrukcji danych obniża się, ale wyłapywane są jedynie najbardziej znaczące cechy.

### Odszumiające (ang. denoising autoencoders)
Architektura samego autoenkodera odszumiającego jest identyczna jak podstawowego autoenkodera, z tą różnicą jednak, że na wejściu do sieci neuronowej wrzucane są zaszumione dane, a funkcja straty liczona jest względem oryginalnego wejścia, bez dodatkowego szumu.

![Architektura autoenkodera odszumiającego [1]](../resources/denoising-autoencoder.png)  
*Architektura autoenkodera odszumiającego [1]*

### Kurczliwe (ang. contractive autoencoders)
W tym przypadku funkcja straty jest skonstruowana w taki sposób, żeby pozbyć się reprezentacji danych, które są zbyt wrażliwe na niewielkie odchylenia w danych wejściowych. Może być użyty jako technika obrony przed atakami unikowymi, ponieważ dwie podobne do siebie próbki danych wejściowych powinny w tym przypadku uzyskać również podobne do siebie kodowania.

### Wariacyjne (ang. variational autoencoders VAE)
[2] Są autoenkoderami generatywnymi. Na podstawie danych ze zbioru uczącego potrafią generować losowe nowe wyniki (podobnie jak przy GAN).
Różnica architektoniczna tego typu autoenkodera leży w budowie kodera. Wynikiem kodera są tutaj dwa wektory o jednakowych rozmiarach.
Pierwszy z nich jest wektorem średnich, a drugi wektorem odchyleń standardowych. Następnie na ich podstawie liczony jest zakodowany wektor reprezentacji danych, który zostanie odkodowany według standardowej procedury.
Dzięki takiemu podejściu autoenkoder stał się tak naprawdę generatorem nowych próbek, które są z zakresu rozkładu danych treningowych.

![Architektura autoenkodera wariacyjnego [2]](../resources/variational-autoencoder.png)  
*Architektura autoenkodera wariacyjnego [2]*

### Antagonistyczne (ang adversarial autoencoders)
[3] Są połączeniem koncepcji autoenkoderów wariacyjnych z generatywnymi sieciami antagonistycznymi. Tak naprawdę są to różne podejścia do problemu generowania danych.
Wektor zakodowany podobnie jak w przypadku VAE składa się z dwóch podwektorów – średniej wartości i odchylenia standardowego. W tym przypadku staramy się wymusić na koderze, by podążał znanym rozkładem.
Architektura tego rozwiązania bazuje na architekturze antagonistycznych sieci generatywnych. Do dyskryminatora, tak jak i do dekodera dostarczamy zakodowany wektor reprezentacji danych wejściowych.
Do dyskryminatora dostarczany jest również rozkład danych treningowych.

![Architektura autoenkodera antagonistycznego [2]](../resources/adversarial-autoencoder.png)  
*Architektura autoenkodera antagonistycznego [2]*

Matematyczny zapis architektury antagonistycznego autoenkodera:

```
𝑥 − 𝑑𝑎𝑛𝑒 𝑤𝑒𝑗ś𝑐𝑖𝑜𝑤𝑒
𝑧 − 𝑤𝑦𝑛𝑖𝑘 𝑘𝑜𝑑𝑒𝑟𝑎
𝑞(𝑧|𝑥) − 𝑟𝑜𝑧𝑘ł𝑎𝑑 𝑘𝑜𝑑𝑜𝑤𝑎𝑛𝑖𝑎
𝑝(𝑥|𝑧) − 𝑟𝑜𝑧𝑘ł𝑎𝑑 𝑑𝑒𝑘𝑜𝑑𝑜𝑤𝑎𝑛𝑖𝑎
𝑝𝑑(𝑥) − 𝑟𝑜𝑧𝑘ł𝑎𝑑 𝑑𝑎𝑛𝑦𝑐h
𝑝(𝑥) − 𝑟𝑜𝑧𝑘ł𝑎𝑑 𝑚𝑜𝑑𝑒𝑙𝑢
𝑝(𝑧) − 𝑟𝑜𝑧𝑘ł𝑎𝑑 𝑗𝑎𝑘𝑖 𝑐h𝑐𝑒𝑚𝑦 𝑛𝑎ł𝑜ż𝑦ć 𝑛𝑎 𝑤𝑒𝑘𝑡𝑜𝑟 𝑧
𝑞(𝑧) = ∫ 𝑞(𝑧|𝑥)𝑝𝑑(𝑥)𝑑𝑥
```

![Realistyczne twarze wygenerowane przy pomocy antagonistycznych autoenkoderów [4]](../resources/adversarial-autoencoder-faces.png)  
*Realistyczne twarze wygenerowane przy pomocy antagonistycznych autoenkoderów[4]*

## Źródła
* [[1] M. Mamczur, „Czym są autoenkodery (autokodery) i jakie maja zastosowanie?,”](www.miroslawmamczur.pl/czym-sa-autoenkodery-autokodery-i-jakie-maja-zastosowanie/)
* [[2] IntroductiontoAdversarialAutoencoders](https://rubikscode.net/2019/01/14/introduction-to-adversarial-autoencoders/)
* [[3] S. Madeline i R. Harang, „Adversarial Autoencoders](https://www.sophos.com/en-us/medialibrary/PDFs/technical-papers/Adversarial-Autoencoders.pdf)
* [[4] Adversarial Autoencoder Achieves GAN-level Results](https://neurohive.io/en/news/adversarial-autoencoder-achieves-gan-level-results/)
