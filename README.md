# ODM-PinkRibbonClassify

<img width="3164" height="1894" alt="image" src="https://github.com/user-attachments/assets/dc2069aa-2f8b-4310-912a-a2646103aadd" />

<h2 align="left">Wybór zbioru danych</h2>

###

<p align="left">Wybrany przeze mnie zbiór to Breast Cancer Wisconsin (Diagnostic) Data Set pochodzący z Kaggle (https://www.kaggle.com/datasets/uciml/breast-cancer-wisconsin-data?resource=download). Zawiera on 569 rekordów. Ostatnia aktualizacja miała miejsce 9 lat temu. Został pobrany 512 tyś. razy a jego Usability Score wynosi 8.53. Proporcja klas wynosi B: 357 (63%), M: 212 (37%), gdzie B oznacza benign (nowotwór łagodny) a M malignant (nowotwór złośliwy), co oznacza, że zbiór mieści się w wymaganym przedziale (niezrównoważenie nie większe niż 70:30). W związku z tym nie zaszła konieczność stosowania dodatkowego oversamplingu i mogłem się skupić na próbkowaniu watstwowym. Zbiór posiada łącznie 30 cech, a targetem jest kolumna diagnosis.</p>

<img width="1582" height="947" alt="image" src="https://github.com/user-attachments/assets/0b70f59b-0815-4465-8b76-efe8220e11d1" />

###

<h2 align="left">Precision czy Recall - uzasadnienie wyboru</h2>

###

<p align="left">W diagnostyce onkologicznej bład typu False Negative - czyli uznanie osoby chorej za zdrową - jest krytyczny, ponieważ prowadzi do zaniechania lezenia i zagrożenia życia. Błąd typu False Positive skutkuje jedynie dodatkowym stresem i badaniami. Naszym celem jest maksymalizacja wykrywalności realnych przypadków nowotworów złośliwych dlatego najbardziej logicznym wyborem jest metryka Recall dla klasy M.</p>

###

<h2 align="left">Podział zbioru danych</h2>

###

<p align="left">Przy zastosowaniu widżetu Data Sampler, dane zostały podzielone zgodnie z zaleceniami w stosunku 80/20, gdzie 80%, czyli 456 rekordów, wykorzystałem do uczenia modeli i 5-krotnej walidacji krzyżowej, a 20%, czyli 113 rekordów, całkowicie odesparowałem i wykorzystałem wyłącznie do końcowej oceny w punkcie piątym (Predykcja i Ewaluacja). Dodatkowo zaznaczyłem opcję Stratify sample (when possible), aby stworzyć bardziej reprezentatywną próbkę testową.</p>

<img width="734" height="1140" alt="image" src="https://github.com/user-attachments/assets/61e2ed5d-cdf5-45eb-8c8e-6a901ebd0507" />

###

<h2 align="left">Wybór algorytmu uczenia maszynowego</h2>

###

<p align="left">Przetestowałem pięć algorytmów (SVM, Logistic Regression, kNN, Random Forest i Neural Network) za pomocą widżetu Test and Score z wykorzystaniem 5-fold Stratified Cross-validation. Oznacza to, że każdy z algorytmów był trenowany i testowany pięciokrotnie na różnych podzbiorach danych treningowych (stanowiących 80% całego zbioru). Takie podejście pozwoliło na uzyskanie wiarygodnej, uśrednionej oceny wydajności modeli i wyeliminowanie wpływu przypadkowego podziału danych. Dla każdego modelu dokonywano zmian parametrów wewnątrz widżetów i na bieżąco obserwowano wpływ tych zmian na wyniki w tabeli walidacji. Poniżej zamieściłem zrzuty ekranu przedstawiające ustawienia każdego z testowanych algorytmów.</p>

<img width="445" height="668" alt="image" src="https://github.com/user-attachments/assets/fac946ef-aa17-443a-b45f-225d22365168" />

<img width="380" height="442" alt="image" src="https://github.com/user-attachments/assets/86992976-c5f2-4669-8915-4e086d998556" />

<img width="357" height="413" alt="image" src="https://github.com/user-attachments/assets/1a74a3a8-de85-41c8-9c42-77ebdcf0d31b" />

<img width="522" height="531" alt="image" src="https://github.com/user-attachments/assets/3c992b8c-0e35-42e7-8409-e23989a3c148" />

<img width="448" height="486" alt="image" src="https://github.com/user-attachments/assets/f71cf2a3-45a6-4618-b570-1e50601d631d" />

<p align="left">Dlaczego wybrałem akurat te algorytmy? SVM szuka optymlanej granicy między klasami, a w medycynie jest ceniony ze względu na to, że świetnie radzi sobie z danymi, które mają dużo cech (mój data set ma ich aż 30). Neural Network potrafi wyłapywać skomplikowane, nieliniowe zależności, których inne modele mogą nie zauważyć. Random Forest jest odporny na błędy w danych i świetnie pokazuje, które cechy są najważniejsze. Logistic Regression warto mieć w zestawieniu jako punkt odniesienia. kNN działa na zasadzie podobieństwa - jeśli nowy pacjent ma parametry guza podobne do pięciu innych ze zdiagnozowanym nowotworem, zostanie tak samo przypisany. Dodatkowo, wybierając akurat te algorytmy, przetestowałem jednocześnie modele statystyczne (Logistic Regression), geometryczne (SVM), zespołowe (Random Forest), biologiczne (Neural Network) i oparte na sąsiedztwie (kNN).</p>

###

<h2 align="left">Predykcja i ewaluacja</h2>

###

<img width="996" height="674" alt="image" src="https://github.com/user-attachments/assets/76c7ccb0-2aa1-4ecd-87ec-b3ddab29f159" />

<p align="left">Zwycięskim modelem okazał się SVM, osiągając wynik Recall równy 0.976. Nie było to dużym zaskoczeniem ze względu na jego wcześniej wspomnianą renomę. Dodatkowo sprawdziłem go na odłożonym zbiorze 113 pacjentów, na którym uzyskał wynik 0.956 dla Precision i 0.956 dla Recall. Poniżej znajduje się zrzut ekrany z Confusion Matrix.

<img width="862" height="480" alt="image" src="https://github.com/user-attachments/assets/07fc236c-e5bc-4db5-83c5-17d2bd98c572" />

<p align="left">Można więc stwierdzić, że model spełnia wymagania. Na 42 chorych pacjentów w zbiorze testowym, model przeoczył tylko 2 osoby (FN=2), co przy tak małym zbiorze jest wynikiem bardzo dobrym.</p>

###

<h2 align="left">Wyniki i wnioski</h2>

###

<p align="left">Algorytm SVM okazał się najbardziej efektywny w kontekście postawionego celu (maksymalizacja Recall). Dzięki zastosowaniu jądra RBF oraz optymalizacji parametrów, model osiągnął czułość na poziomie 0.976 w walidacji krzyżowej oraz 0.956 na zbiorze testowym. Oznacza to, że model jest stabilny i nie uległ overfittingowi. Głównym wyzwaniem był dobór parametrów tak, aby podnosząc Recall, nie zdewastować całkowicie metryki Precision. Udało się zachować Precision na poziomie powyżej 0.95, co oznacza, że model nie generuje nadmiernej liczby "fałszywych alarmów". Macierz pomyłek wykazała jedynie 2 przypadki False Negative na 113 testowanych rekordów. W diagnostyce onkologicznej taki wynik jest bardzo satysfakcjonujący, gdyż minimalizuje ryzyko zaniechania leczenia u osoby faktycznie chorej. W przyszłości wyniki można by próbować poprawić poprzez zastosowanie techniki Feature Selection (np. przy wykorzystaniu widżetu Rank) w celu wyeliminowania cech najmniej wnoszących do modelu, co mogłoby jeszcze bardziej uprościć strukturę SVM. Dodatkowo zwiększenie zbioru danych o przypadki bardziej niejednoznaczne mogłoby pozwolić modelowi na jeszcze lepszą generalizację.
