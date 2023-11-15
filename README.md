# Security By Design - Zadanie 1

## Wymagania
1. Zainstalowana komenda `git` na stacji roboczej
2. Zainstalowany `docker` na stacji roboczej
3. Bezpośredni dostęp do internetu (nie przez proxy)

## Przygotowanie
1. **Założenie konta na GitHub** (jeśli jeszcze nie jesteś zarejestrowany)  
   Aby wykonać ćwiczenie, konieczne jest posiadanie zarejestrowanego użytkownika na portalu github.com.

2. **Wykonanie "fork'a" projektu**  
   Wykonaj "fork'a" projektu [https://github.com/Mixeway-Academy/task2](https://github.com/Mixeway-Academy/task2) - w wyniku tej operacji, w twojej przestrzeni na GitHubie powstanie kopia repozytorium.  
   Zadanie zakłada wykonanie listy operacji na kodzie źródłowym, ale aby nie wprowadzać zmian w przestrzeni, z której korzystają inni użytkownicy, wygodnie jest wykonać kopię w swojej przestrzeni. Więcej informacji znajdziesz [tutaj](https://docs.github.com/en/get-started/quickstart/fork-a-repo).  
   ![img.png](.github/img.png)

3. **Pobranie kopii projektu na swoją stację roboczą** 
   Aby pobrać 'sforkowany' projekt na swoją stację roboczą, wykonaj poniższą komendę:

```shell
git clone https://github.com/{username}/task1

#gdzie {username} to nazwa użytkownika. Wchodząc w swoją kopie repozytoroium przez przeglądarkę można też skorzystać z adresu URL.
```

## Zadanie 1 - Weryfikacja wycieku wrażliwych danych

**Cel:** Celem zadania jest przetestowanie aplikacji w celu weryfikacji, czy w logach znajdują się wrażliwe dane, które niekoniecznie powinny być zawarte w logach aplikacyjnych.

### Instrukcja:
1. **Uruchomienie wybranej aplikacji**
   Zadanie można zrealizować w jednym z dwóch wybranych wariantów: Java lub Python. Obydwa warianty zostały zawarte w katalogach:

```shell
Java/
Python/
```

W repozytorium z zadaniem, pierwszym krokiem, który należy wykonać, jest wybór technologii. O ile wybór nie ma znaczenia przy zadaniu związanym z wyszukiwaniem podatności, o tyle zadanie polegające na zaproponowaniu poprawki wymagać będzie niewielkiej wiedzy dotyczącej programowania w wybranej technologii.

Aby uruchomić aplikację JAVA, należy wykonać operacje:

```shell
cd Java/spring-thymeleaf-crud-example
docker build -t task1-java .
docker run -p 8080:8080 task1-java
```
W wyniku tych operacji na stacji roboczej zostanie uruchomiony kontener dockerowy z aplikacją JAVA, wyeksponowany na porcie 8080. Przez przeglądarkę aplikacja będzie dostępna pod adresem http://localhost:8080.

Aby uruchomić aplikację Python, należy wykonać operacje:

```shell
cd Python/Flask_Book_Library
docker build -t task1-python .
docker run -p 5000:5000 task1-python
```
W wyniku tych operacji na stacji roboczej zostanie uruchomiony kontener dockerowy z aplikacją Python (Flask), wyeksponowany na porcie 5000. Przez przeglądarkę aplikacja będzie dostępna pod adresem http://localhost:5000.

2. **Weryfikacja logów aplikacji**
   Aplikacja Java to aplikacja pozwalająca na rejestrowanie studentów przez GUI.

   Aplikacja Python to aplikacja, która implementuje wybrane możliwości biblioteki - istnieje możliwość dodawania książek itp. 

   W tej cześci zadania należy "przeklikać się" przez aplikacje czyli skorzystać ze wszystkich funkcjonalności, dodania zasobów, edycji przeglądu. W kolejnym kroku należy zweryfikować zawarość logów (w przypadku uruchomienia aplikacji za pośrednictwem docker'a logi powinny być widoczne na konsoli, w której uruchomiono kontener, w innym przypadku należy skorzystać z komendy `docker logs` - https://docs.docker.com/engine/reference/commandline/logs/)

   **Szukamy w logach danych wrażliwych, osobowych takich jak pesel czy adres**
3. **Zaproponowanie poprawki**

   W przypadku identyfikacji ciągu znaków w logach aplikacji należy zaproponować zmianę, która spowoduje usunięcie zagrożenia, jakim jest wyciek wrażliwych danych.

   **O ile to jest możliwe nie ingerujemy w format logów, tylko powodujemy, że zidentyfikowane wrażliwe dane (pesel, adres) będą zamaskowane w logach aplikacji, zamaskowane czyli zamiast wartości `123456890` pojawi się wartość `*********`**
4.  **Przesłanie wyników
    Wynikiem ćwiczenia ma być przygotowany `Pull Request` z zaimplementowaną poprawką. Informacje jak przygotować `Pull Request` znajdują się [tutaj](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request).

    Co musi zawierać `Pull Request`:
    - W opisie musi znaleźć się informacja o znalezionej podatności - co to za podatność, gdzie została znaleziona, sposób jej odtworzenia oraz screen udowadniający jej wystąpienie.
    - Commit (zmianę w kodzie), która zawiera wprowadzoną zmianę.

## Zadanie 2 - weryfikacja wycieku sekretów

**Cel:** Celem zadania jest weryfikacja, czy w repozytorium (i jego historii) znajdują się sekrety i/lub hasła, które zostały `hardcodowane` w kodzie źródłowym lub jednym z plików w przestrzeni dyskowej

### Insrukcja:

1. **Uruchomienie aplikacji gitleaks**
    Będąc w katalogu główym (Task2) należy uruchomić aplikacje gitleaks via docker:
    ```shell
    docker run -v ${path_to_host_folder_to_scan}:/path zricethezav/gitleaks:latest detect --source="/path" -v 
    ```
   gdzie: `path_to_host_folder_to_scan` to ścieżka do pobranego na stację roboczą folderu z repozytorium `Task2`
2. **Weryfikacja działania i wyników**
    Zweryfikowanie wyników, które zostały wykryte przez aplikacje gitleaks. Weryfikacja polega na zweryfikowaniu czy gitleaks nie zasygnalizował wykrycia fałszywie pozytywnego, wykryciem fałszywie pozytywnym jest np wystąpienie w kodzie aplikacji słowa `password`, które jest zmienną, a nie przyjmuje narzuconej z góry wartości.
3. **Przesłanie wyników**
    Uzupełnienie informacji w `Pull Request'cie` z zadania 1, które zawierają:
   - info o tym jak uruchomiono weryfikacje sekretów
   - info o tym, co zostało wykryte
   - propozycje poprawienia problemu (nie robimy zmiany w kodzie, tylko jednym, dwoma zdaniami piszemy jak należy zarządzać hasłami i sekretami w kodzie źródłowym)

## Zadanie 3 - weryfikacja bezpieczeństwa bibliotek OpenSource wykorzystywanych w projekcie 

**Cel:** Uruchomienie weryfikacji bezpieczeństwa bibliotek OpenSource, które są wykorzystywane w projekcie w celu oceny bezpieczeństwa badanej aplikacji.

W przypadku Aplikacji napisanej w JAVA wykorzystywany będzie OWASP Dependency Check - https://owasp.org/www-project-dependency-check/

### Instrukcja:
1. **Uruchomienie weryfikacji pakietów OpenSource**
    W przypadku weryfikacji aplikacji JAVA należy przejść do katalogu `JAVA/spring-thymeleaf-crud-example` a następnie uruchomić:
    ```shell
    docker run --rm \
    --volume path_to_host_folder_to_scan:/src:z \
    owasp/dependency-check:latest \
    --scan /src \
    --format "ALL" \
    --project "dependency-check scan: $(pwd)"
    ```
    gdzie: `path_to_host_folder_to_scan` to ścieżka do pobranego na stacje roboczą folderu `JAVA/spring-thymeleaf-crud-example`
    Możemy zaobserwować kilka ERRROow na końcu jednak w katalogu Java powinny się pojawić takie pliki jak `dependency-check-report.html` który zawiera wynik działania

    W przypadku aplikacji python nalezy wejść do katalogu `Python/Flask_Book_Library` a następnie uruchomić:
    ```shell
    docker run -it -v path_to_host_folder_to_scan:/sources pyupio/safety safety check -r /sources/requirements-task.txt --full-report
    ```
   gdzie: `path_to_host_folder_to_scan` to ścieżka do pobranego na stacje roboczą folderu `Python/Flask_Book_Library`

    W tym przypadku wynik analizy pojawi się na konsoli
2. **Analiza wyników**
    Niezależnie od wyboru aplikacji, która była analizowana wyniki zawierają informację o:
    - Który pakiet, w jakiej wersji jest podatny
    - Opis podatności (czasem krótki, z linkiem zawierającym więcej informacji)

    Należy wybrać jedną wykrytą podatność/pakiet, który zawiera podatność (o najwyższej krytyczności), a następnie wykonać analize możliwości jego wykorzystania. Analiza to nie studium użycia - nie ma potrzeby aby próbować wykorzystać podatność, która została wykryta w danej bibliotece. Chodzi o przeczytanie i zweryfikowanie co powoduje wystąpienie podatności w danym pakiecie typu `W pakiecie X wykryto podatność typu RCE oznaczoną jako Krytyczna. Wykorzystanie podatności jest możliwe po uruchomieniu metody Y z klasy Z. Po analizie w badanej aplikacji klasa Z nie jest wykorzystywana także prawdopodobieństwo wykorzystania tej podatności jest minimalne`
3. **Przesłanie wyników**
    Uzupełnienie PRa o informacje:
    - Screen z wykonanego skanu (np. wynik komendy)
    - Lista wykrytych podatności w tym podaności o Krytyczności Critical
    - Opis i analiza dla jednej wybranej podatności o jak najwyższej krytyczności
    - Ocena tego, czy przeanalizowana podatnosc musi być usunięta i jeśli tak propozycja wersji pakietu, która nie zawiera podatności

# Podesłanie wyników
* Wyniki powinny być podesłane w formie `Pull Requesta` w sforkowanym projekcie
* Pull Request powinien zawierać wszystkie 3 zadania (jeśli każde zadanie jest w innym PR lub zadanie 1 jest zrobione via PR a dwa pozostałe, które nie zawierają zmian w kodzie zgłoszone jako ISSUE wszystkie linki muszą trafić do zadania w Teams )
* Wynik dla zadania 1 - zmiana w kodzie + opis
* Wynik dla zadania 2 - opis + analiza
* Wynik dla zadania 3 - opis + analiza
* Linki do oceny należy umieścić w zadaniu w Teams

# Punktowanie (ćwiczenie oceniane w skali 0-5 pkt):
- 2 za analizę i poprawnienie podatności związanej z wyciekiem wrażliwych danych
- 1 zadanie 2 - wykrycie i analizę wycieku sekretów
- 2 punkty za analizę podatności OpenSource

## Credits
* Java application - [GitHub Repo](https://github.com/pedrohenriquelacombe/spring-thymeleaf-crud-example)
* Python application - [GitHub Repo](https://github.com/MohammadSatel/Flask_Book_Library)