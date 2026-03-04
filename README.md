# Theoretical-predictive-model-of-bitcoin-price
theoretical project whose code demonstrates data collection, calculation of advanced metrics, appropriate scaling, and construction of a multi-layer LSTM network with Dropout regularization .



1. Przegląd Projektu
Projekt stanowi studium przypadku z zakresu finansów ilościowych (Quantitative Finance). Celem była budowa potoku danych oraz modelu uczenia głębokiego do prognozowania binarnych zmian kierunkowych ceny Bitcoina (BTC-USD) w horyzoncie jednodniowym. Kluczowym elementem badania była weryfikacja wpływu kosztów transakcyjnych (prowizji i poślizgów cenowych) na skuteczność teoretycznej strategii inwestycyjnej.

2. Architektura Systemu i Uzasadnienie Wyboru Narzędzi
2.1. Przetwarzanie Danych (Polars)
Zdecydowano się na wykorzystanie biblioteki Polars zamiast standardowego pakietu Pandas.

Uzasadnienie: Wykorzystanie silnika napisanego w Rust i leniwej ewaluacji (lazy evaluation) pozwala na znaczną optymalizację pamięciową i czasową przy obliczaniu cech kroczących (rolling features), co jest krytyczne w środowiskach High-Frequency Trading (HFT) lub przy obsłudze dużych zbiorów danych historycznych.

2.2. Normalizacja (RobustScaler)
Do skalowania danych wejściowych wykorzystano RobustScaler.

Uzasadnienie: Szeregi czasowe kryptowalut charakteryzują się ekstremalną zmiennością i występowaniem grubych ogonów rozkładu (fat tails). RobustScaler, operując na medianie i rozstępie międzykwartylnym (IQR), zapobiega dominacji modelu przez wartości odstające (outliery), co w przypadku standardowej normalizacji Z-score prowadziłoby do spłaszczenia sygnału rynkowego.

2.3. Model Predykcyjny (Attention-LSTM)
Zastosowano architekturę hybrydową łączącą warstwy Long Short-Term Memory (LSTM) z mechanizmem Multi-Head Attention.

Decyzja: Problem sformułowano jako klasyfikację binarną (prawdopodobieństwo wzrostu/spadku), odrzucając regresję punktową ceny.

Logika: Regresja w szeregach czasowych o wysokim poziomie szumu prowadzi do efektu "gonienia ceny" (lagging). Mechanizm Attention pozwala modelowi na dynamiczne nadawanie wag istotnym zdarzeniom z 30-dniowego okna czasowego, co teoretycznie umożliwia lepszą identyfikację formacji odwrócenia trendu niż klasyczne sieci rekurencyjne.

3. Metodyka Testowa (Backtesting)
Zaimplementowany silnik backtestowy uwzględnia parametry Fee (prowizja) oraz Slippage (poślizg cenowy).

Analiza kosztów: Wprowadzenie kosztów na poziomie 0.15% (łącznie) na każdą transakcję służy eliminacji błędu przeżywalności strategii. Większość modeli wykazujących wysoką trafność (Win Rate) w warunkach bezkosztowych traci rentowność przy uwzględnieniu mikrostruktury rynku.

4. Wyniki i Analiza Błędów
Na podstawie przeprowadzonych testów na danych out-of-sample (poza próbką treningową), sformułowano następujące wnioski:

Erozja Przewagi (Edge Erosion): Model wykazuje trafność (Accuracy) na poziomie 52-54%. W warunkach idealnych pozwala to na generowanie dodatniej stopy zwrotu. Jednak po nałożeniu realnych kosztów egzekucji, przewaga statystyczna ulega niemal całkowitej redukcji.

Opóźnienie Predykcji: Analiza wykresu predykcji wskazuje na występowanie zjawiska autokorelacji błędów – model poprawnie identyfikuje trend, ale często z opóźnieniem jednej jednostki czasu (t+1), co w tradingu jednodniowym czyni sygnał bezużytecznym pod względem operacyjnym.

Ryzyko Systematyczne (Drawdown): Model nie posiada mechanizmów reagowania na zdarzenia egzogeniczne (np. newsy makroekonomiczne), co prowadzi do gwałtownych obsunięć kapitału (Maximum Drawdown) w momentach zmiany reżimu rynkowego.

5. Ograniczenia i Charakter Projektu
Projekt ma charakter wyłącznie teoretyczny i edukacyjny. Służy on jako platforma do testowania hipotez z zakresu uczenia maszynowego w finansach.

Ograniczenie danych: Model operuje wyłącznie na danych endogenicznych (cena, wolumen), ignorując dane on-chain oraz arkusz zleceń (Order Book), co znacząco ogranicza jego zdolność do przewidywania krótkoterminowych braków płynności.

Interpretowalność: Pomimo zastosowania warstw Attention, model pozostaje strukturą typu "black-box", co utrudnia identyfikację konkretnych czynników decyzyjnych w sytuacjach skrajnej zmienności.

6. Podsumowanie
Projekt dowodzi, że budowa modelu o wysokiej trafności teoretycznej jest wtórna wobec problemu zarządzania kosztami transakcyjnymi i ryzykiem.
