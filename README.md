# Projekt serwisu z gotowym modelem wykorzystujący uczenie głębokie do klasyfikacji gatunków utworów muzycznych

## Streszczenie

Projekt zawiera zaadaptowany model z [Choi et al.](https://github.com/keunwoochoi/music-auto_tagging-keras) do stworzenia własnego systemu klasyfikacji utworów muzycznych względem ich gatunków. Model został wygenerowany przy użyciu Konwolucyjnej Sieci Neuronowej (Convolutional Neural Network) oraz Rekurencyjnej Sieć Neuronowej (Recurrent Neural Network). Jako rezultat system udostępnia wektor z przypisanymi procentowym przynależeniem danego pliku audio do klasyfikowanych gatunków muzycznych . Projekt zapewnia również mikroserwis do uruchomienia modelu zdalnie.

## Krótki opis użytej sieci neuronowej
Projekt korzysta z wcześniej wytrenowanego modelu. Dzięki czemu możliwe jest szybkie sklasyfikowanie gatunku utworu z zadowalającą dokładnością (powyżej 80% trafności). Model może zostać wygenerowany za pomocą konwolucyjnej sieci nauronowej (CNN) lub konwolucyjnej rekurencyjnej sieci neuronowej (CRNN) - w zależności, która wersja biblioteki Keras została użyta.
W obecnym projekcie została wykorzystana wersja z CRNN.
#### Pierwotnie sieć była skonstruowana według schematu:
Po lewej CNN, po prawej CRNN
![alt text](https://github.com/keunwoochoi/music-auto_tagging-keras/blob/master/imgs/diagrams.png?raw=true)

**CRNN** zawiera 4 warstwy konwolucyjne 2D oraz 2 Gated Recurrent Units (GRU). GRU został opracowany przez Kyunghyun Cho w 2014r., ma za zadanie m.in. przeciwdziałać występowaniu problemu zanikania gradientu. Jest to uproszczony mechanizm Long short-term memory (LSTM), dla małych zbiorów danych.
Ilość neuronów w kolejnych warstwach konwolucyjnych: 64 x 128 x 128 x 128.
Sieć zawiera batch normalization i dropout.
#### Zbiór danych treningowych
Do wytrenowania sieci i stworzenia modelu wykorzystano zbiór plików muzycznych  [kolekcja GTZAN](http://marsyas.info/downloads/datasets.html) . Zawiera on 1000 plików muzycznych w formacie .au, podzielony po 100 dla każdego z 10 gatunków:
- Blues
- Classical
- Country
- Disco
- HipHop
- Jazz
- Metal
- Pop
- Reggae
- Rock

Każdy z plików jest długości 30 sekund.
### Działanie
Predykowany plik .mp3 zostaje przekonwertowany do odpowiedniego formatu i podzielony na ramki długości 30 sekund. Następnie dla każdej z ramek zostaje wygenerowany spektrogram w formie zakodowanego obrazu. Sieć neuronowa przetwarza otrzymane spektrogramy dając na wyjściu wartości oznaczające procentową przynależność, utworu muzycznego, od każdego z gatunków.

## Kod 
Repozytorium zawiera kowolucyjną rekurencyjną sieć neuronową zaimplementowaną w języku Python, wraz z wytrenowanym wcześniej modelem. Zawiera również aplikację WWW napisaną z użyciem mikroframeworka Flask w języku Python. Upraszcza on projektowanie zapewniając przejrzysty schemat łączenia adresów URL oraz źródła danych. Dzięki aplikacji webowej, możliwe jest dokonanie klasyfikacji i wysłanie jej wyników w formie pliku JSON.

### Warunki wstępne
Sieć działa na CPU, więc nie są potrzebne dodatkowe biblioteki oraz sterowniki karty graficznej.
Uruchomienie i poprawne działanie sieci wymaga użycia biblioteki Keras wraz z Tensorflow (możliwe jest również użycie Theano przy niewielkich zmianach w kodzie).
W naszym przypadku sieć działa najlepiej dla:
- Keras 1.2.2
- Tensorflow 1.12.0

- Managera pakietów [pip](https://pip.pypa.io/en/stable/installing/) 

Pozostałe wymagane biblioteki znajdują się w pliku *requirements.txt*

```
 # Chcąc uruchomić model należy aktywować virtualne środowisko
 # Następnie zainstalować wymagane biblioteki
 
 pip install -r requirements.txt
 
```

### Uruchomienie

Aby uruchomić aplikację należy uruchomić w konsoli lub w dowolnym środowisku programistycznym plik quick_test.py.
```
 python quick_test.py
 
```
Lub jeśli aplikacja znajduje się na serwerze, należy umieścić klasyfikowany plik na serwerze, następnie wysłać odpowiednie zapytanie RESTowe:
```
<adres_serwera>/predict
```
które zwróci wynik w postaci pliku JSON.

## Wyniki

### Antonio Vivaldi - Storm

![fig_sea](https://github.com/kirjava113/recoGenre-server/blob/master/genres_prediction.png?raw=true) 
