# Raport Program do Obliczania Różnic Kursowych

## Wprowadzenie
Program ma za zadanie komunikowanie się z API Narodowego Banku Polskiego i na podstawie wprowadzonych danych o fakturze oraz płatności obliczyć kurs oraz prawidłowość przeprowadzonej płatności.

## Raport z przebiegu zadania
### Uruchomienie programu i analiza wyników:
Program po uruchomieniu zarządca od użytkownika wprowadzenia 3 podstawowych danych o fakturze a są to:
- wartość faktury,
- jedna z 3 obsługiwanych walut,
- data wystawienia faktury w formacie RRRR-MM-DD.
Następnie podajemy te same 3 dane dla płatności za fakturę.
Program w tym momencie porówuje płatność z fakturą i wyświetla nam informację na podstawie kursów o tym czy płatność została zrobiona w całości czy mamy nadpłatę lub niedopłatę.
Wszystkie wprowadzone oraz pobrane dane są zapisywane w pliku .txt w celu tworzenia archiwum płatności.

### Kod źródłowy
```
wrzucic gotowy kod zrodlowy
```

#### Połaczenie z API NBP
Aby połączyć się z API NBP użyłem filmiku na YouTUbe https://www.youtube.com/watch?v=C3jaMFh9Ut0.
Pierwszym krokiem było zainstalowanie/aktualizacja pakieru PiP komendą Pip install requests w konsoli.
PiP jest oficjalnym oraz domyślnym system zarządzania pakietami dla środowiska języka Python.
Następnie użyłem linku https://api.nbp.pl/api/exchangerates/tables/a/?format=json aby sprawdzić dane z API NBP w fomacie json.
Aby API mogło działać musimy zaimportować biblioteki ```python import requests```.

#### Dane faktur
```python
invoice_value = input("Podaj wartość faktury:\n")
invoice_currency = input("Wybierz walutę z podanych:\nEUR\nUSD\nCZK\n")
invoice_date = input("Podaj datę wystawienia (RRRR-MM-DD\n)")
invoice = [invoice_value, invoice_currency, invoice_date]
```
W tym fragmencie kody pobieramy od użytkownika wartość faktury, jej walutę oraz datę wystawienia.
Następnie wszystkie 3 wartości przypisujemy do odpowiadających im zmiennych.
Ostatnim krokiem jest przypisanie listy do której przypisujemy wszystkie 3 zmienne.

#### Dane płatności
```python
payment_value = input("Podaj wartość płatności:\n")
payment_currency = input("Wybierz walutę z podanych:\nEUR\nUSD\nCZK\n")
payment_date = input("Podaj datę płatności (RRRR-MM-DD\n)")
payment = [payment_value, payment_currency, payment_date]
```
Pobieranie danych płatności jest działa tak samo jak pobieranie danych faktury.

#### Zapis faktur do pliku
```python
import csv

with open('invoices.txt', 'a', encoding='UTF8') as invoices_file:
    writer = csv.writer(invoices_file)
    writer.writerow(invoice)
    invoices_file.close()
```
Aby móc zapisywać do pliku najpierw importujemy biblioteki csv.
Następnie otwieramy plik 'invoices.txt' w trybie dodawania danych używając kodowania UTF8.
Później tworzymy obiekt pisarza (writer) klasy csv, który będzie odpowiedzialny za zapisanie danych w formacie CSV.
Kolejna linia kodu zapisuje jedną linijkę danych zmienną 'invoice' do pliku CSV.
Na końcu, plik zostaje zamknięty.

#### Zapis metody płatności do pliku
```python
import csv

with open('payments.txt', 'a', encoding='UTF8') as payment_file:
    writer = csv.writer(payment_file)
    writer.writerow(payment)
    payment_file.close()
```
Zapisywanie danych płatności jest identyczne jak przy zapisywaniu danych faktury.

#### Komunikacja z API
```python
communication = requests.get(f'http://api.nbp.pl/api/exchangerates/rates/c/{currency}/{invoice_date}/?format=json')
response = communication.json()
result = response['rates'][0]['bid'] * quantity
```
W tym miejscu pobieramy dane na temat konkretnej waluty w zadanym dniu.
Aby to zrobić w odpowiednim miejscu w linku do API podstawiamy dwie waluty oraz daty które dokładnie specyfikują nam wymagane przez nas dane.
Następnie, pobrana odpowiedź z serwera jest przetwarzana jako dane w formacie JSON, za pomocą metody .json().
Na końcu, wartość kursu kupna bid dla danej waluty jest pobierana z odpowiedzi i mnożona przez ilość quantity w celu obliczenia końcowej kwoty result.

#### Zapis wyniku operacji do pliku
```python
with open('payment_results.txt', 'a', encoding='UTF8') as results_file:
            writer = csv.writer(results_file)
            results_file.write(str(result))
            results_file.close
```
Fragment który zapisuje wyniki operacji pobierania danych walutowych do pliku, działa identycznie jak zapisywanie danych faktury oraz zapisywanie danych płatności.

#### Wyświetlenie w terminalu informacji o statusie faktury
```python
if invoice_value == payment_value:
    print(f"wartośc faktury wynosi {invoice_value}, czyli {result} w {invoice_currency} - stan na dzień {invoice_date} - Faktura została opłacona w całośći")
elif invoice_value < payment_value:
    print(f"wartośc faktury wynosi {invoice_value}, czyli {result} w {invoice_currency} - stan na dzień {invoice_date} - zapłacone zostało {payment_value} z {invoice_value} wymaganych - nastąpiła nadpłata (wymagany zwrot w kwocie", int(payment_value) - int(invoice_value)(invoice_currency),")")
else:
    print(f"wartośc faktury wynosi {invoice_value}, czyli {result} w {invoice_currency} - stan na dzień {invoice_date} - zapłacone zostało {payment_value} z {invoice_value} wymaganych - nastąpiła niedopłata (wymagana dopłata w kwocie", int(invoice_value) - int(invoice_value)(invoice_currency),")")
```
Porównanie danych faktury oraz płatności i wyświetlenie informacji do użytkownika czy faktura została opłacona w całości, czy mamy nadpłatę lub czy mamy niedopłatę.

#### BŁĘDY
```python
wprowadzić fragmenty które odpowidają za walidacje bledow
```

## Załączniki
- **Zrzuty ekranu**:

---

> Raport przygotowany przez: Norbert Gotfryd
> Data: XX.01.2024
