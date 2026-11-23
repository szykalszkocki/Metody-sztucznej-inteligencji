# Sprawozdanie z laboratorium 1

- **Repozytorium** — [Metody-sztucznej-inteligencji](https://github.com/szykalszkocki/Metody-sztucznej-inteligencji)
- **Aplikacja** — Codex (środowisko Codex Cloud)
- **Model** — GPT-6.2 Sol 
- **Poziom rozumowania** — Wysoki

## 1. Zlecenie, zakres i odbiór

Zlecono wygenerowanie raportu dla `data/exercise.csv` i zapisanie go w `results/exercise-summary.json`. W etapie 1 kod, CSV i testy miały pozostać bez zmian. W etapie 2 dopuszczono zmianę jednej etykiety i ponowne wygenerowanie raportu. Do odbioru poproszono o wykonane polecenia, wyniki procesów, pełną treść raportu i wynik gotowego testu.

## 2. Wygenerowanie i odczyt raportu

Agent użył narzędzia terminalowego `exec_command` w zdalnym środowisku Codex Cloud. Polecenie wykonano z katalogu `lab1/project` przy użyciu Pythona 3.12.14.

```bash
python3 describe_data.py --data data/exercise.csv --out results/exercise-summary.json
```

Skrypt zapisał raport i zakończył się kodem `0` w obu etapach. Po każdym uruchomieniu raport odczytano poleceniem `cat results/exercise-summary.json`. Końcowy wynik to 8 rekordów CSV, w tym 4 z etykietą `ham` i 4 z etykietą `spam`. Ścieżkę wejścia, sumę SHA-256 i liczebności zweryfikowano względem aktualnego CSV. [Końcowy raport JSON](https://github.com/szykalszkocki/Metody-sztucznej-inteligencji/blob/5e0d544af9c4f706fb284f5f2f6996f7ffd03be1/lab1/project/results/exercise-summary.json)

## 3. Wyjaśniony fragment i przewidywanie

W sekcji **Environment and first run** wyjaśniono polecenie z opcjami `--data` i `--out`. Pierwsza wskazuje CSV do odczytu, a druga plik JSON do zapisu. Obie ścieżki są względne wobec katalogu projektu. Przewidywanie było takie, że podanie `--data data/demo.csv --out results/demo-summary.json` użyje danych demonstracyjnych i zapisze osobny raport. README podaje dokładnie tę komendę jako opcjonalną próbę, co potwierdziło przewidywanie bez uruchamiania programu. [README projektu](project/README.md#environment-and-first-run)

## 4. Gotowy test

Z katalogu projektu uruchomiono test poleceniem `python3 -m unittest discover -s tests -v`. Wynik to `Ran 1 test`, `OK` i kod wyjścia `0`. Test uruchomił skrypt na trzech syntetycznych wiadomościach z `tests/fixtures/messages.csv`. Sprawdził kod zakończenia, utworzenie i odczyt raportu JSON, ścieżkę pliku wejściowego oraz oczekiwane liczebności: 3 wiadomości, w tym 2 `ham` i 1 `spam`. Użył osobnego katalogu tymczasowego, więc wynik testu był niezależny od raportu ćwiczeniowego. [Kod testu](project/tests/test_report.py)

## 5. Zmiana etykiety i porównanie raportów

W `data/exercise.csv` zmieniono etykietę `exercise-008` z `ham` na `spam`. Przewidywanie zakładało o jedną wiadomość mniej w `ham`, o jedną więcej w `spam` i tę samą liczbę wszystkich wiadomości. Bezpośrednio po edycji suma kontrolna pliku raportu potwierdziła, że raport wciąż był w wersji sprzed zmiany. Następnie ponownie uruchomiono skrypt, aby zapisać aktualny wynik. Odczytano JSON i porównano go z niezależnym zliczeniem etykiet w CSV oraz z sumą SHA-256 danych wejściowych.

| Raport | `ham` | `spam` | Razem |
|---|---:|---:|---:|
| Przed zmianą | 5 | 3 | 8 |
| Po ponownym uruchomieniu | 4 | 4 | 8 |

Wynik zgadza się z przewidywaniem. Końcowy CSV i raport zweryfikowano również na gałęzi `main`. [Końcowy commit na GitHubie](https://github.com/szykalszkocki/Metody-sztucznej-inteligencji/commit/5e0d544af9c4f706fb284f5f2f6996f7ffd03be1)

**Pełny zapis sesji** — do uzupełnienia eksportem z aplikacji w katalogu `lab1/sesje/`.
