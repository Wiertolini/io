<div align="center">

# 🛹 SkateFlow

### Społeczność deskorolkowa 2.0
*Łączymy pasję do deskorolki z nowoczesną technologią wideo i społecznościową rywalizacją.*

![SkateFlow Banner](https://via.placeholder.com/800x200/2a2a2a/ffffff?text=SkateFlow+Platforma+Wideo+Deskorolkowa)

</div>

## 📖 Spis treści
- [O projekcie](#-o-projekcie)
- [Kluczowe funkcje](#-kluczowe-funkcje)
- [Role użytkowników](#-role-użytkowników)
- [Cykl życia obiektów](#-cykl-życia-obiektów)
- [Technologie](#-technologie)
- [Licencja](#-licencja)

---

## 🚀 O projekcie

**SkateFlow** to wyspecjalizowana platforma społecznościowa typu wideo, dedykowana wyłącznie kulturze i społeczności deskorolkowej. Projekt łączy mechanizmy znane z popularnych mediów społecznościowych z elementami gier rywalizacyjnych oraz profesjonalnej oceny sportowej.

### 🎯 Główny cel
Stworzenie cyfrowego środowiska, w którym użytkownicy mogą nie tylko dzielić się swoimi osiągnięciami wideo, ale również:
- Rywalizować w grach **S.K.A.T.E.**
- Zdobywać uznanie (odznaki jakości, oceny sędziów)
- Odkrywać i weryfikować nowe miejsca do jazdy (**Spoty**)
- Budować autorytet w środowisku poprzez merytoryczną ocenę innych skaterów

---

## 🔑 Kluczowe funkcje

### 👤 Dla Skatera (Użytkownika)
- **Zarządzanie kontem:** Rejestracja, logowanie, edycja profilu (bio, avatar, miasto).
- **Przesyłanie klipów:** Dodawanie filmów z opcją przycinania fragmentu, nadawanie tytułów, tagowanie trików i miejscówek.
- **Interakcje społecznościowe:** 
  - Przeglądanie spersonalizowanego feedu.
  - Głosowanie **Hype** 🔥 (pozytywna) lub **Bail** 💥 (negatywna) na klipy.
  - Dodawanie komentarzy pod klipami.
  - Obserwowanie innych użytkowników.
- **Rywalizacja S.K.A.T.E.:**
  - Tworzenie wyzwań (wskazywanie przeciwnika i triku początkowego).
  - Dołączanie do wyzwań (akceptacja, dogrywanie odpowiedzi wideo).
  - Udział w społecznościowej grze.

### ⚖️ Dla Sędziego
*(Wymagana weryfikacja uprawnień przed każdą akcją)*
- **Ocena triku:** Wystawianie noty punktowej (1-10).
- **Oznaczanie poziomu trudności:** Tagowanie klipów etykietami: `Łatwy` / `Trudny` / `Pro`.
- **Nadawanie Odznaki Jakości (Quality Badge):** Specjalne wyróżnienie dla wybitnych klipów, zwiększające ich zasięgi.
- **Rozstrzyganie bitew video:** Werdykt w pojedynkach S.K.A.T.E. (wskazanie zwycięzcy).

### 🛡️ Dla Moderatora
- **Zarządzanie treścią:** Usuwanie komentarzy naruszających regulamin.
- **Egzekwowanie zasad:** Nakładanie i zdejmowanie **Shadowbanu** (ograniczenie widoczności treści użytkownika bez jego wiedzy).
- **Weryfikacja miejscówek (Spoty):** Akceptacja lub odrzucenie nowo zgłoszonych lokalizacji do jazdy.

### ⚙️ Dla Administratora
- **Zarządzanie personelem:** Nadawanie i odbieranie ról (Sędzia, Moderator).
- **Konfiguracja systemu:** Ustalanie wag algorytmu wyświetlania feedu.
- **Zarządzanie słownikiem trików:** Dodawanie, ukrywanie i kategoryzacja ewolucji deskorolkowych (`Flip`, `Grind`, `Grab`).
- **Bezpieczeństwo:** Logowanie i odzyskiwanie hasła.

---

## 👥 Role użytkowników

System opiera się na hierarchii użytkowników z różnymi poziomami uprawnień:

```mermaid
classDiagram
    class Użytkownik {
        +id: UUID
        +email: String
        +hashHasła: String
        +rola: Enum
        +czyShadowban: Boolean
    }
    class Skater {
        +bio: String
        +urlAvatara: String
        +liczbaObserwujących: int
    }
    class Sędzia {
        +dziennyLimitOdznak: int
    }
    class Moderator {
        +regionModeracji: String
    }
    class Administrator {
        +poziomUprawnień: int
    }

    Użytkownik <|-- Skater
    Użytkownik <|-- Sędzia
    Użytkownik <|-- Moderator
    Użytkownik <|-- Administrator
🔄 Cykl życia obiektów (Diagramy stanów)
🎬 Wyzwanie S.K.A.T.E.
stateDiagram-v2
    [*] --> Oczekujące: utwórzWyzwanie()
    Oczekujące --> Aktywne: akceptujWyzwanie()
    Oczekujące --> [*]: odrzućWyzwanie()
    Aktywne --> Aktywne: dodajKlipOdpowiedzi()
    Aktywne --> Zakończone: rozstrzygnijWyzwanie()
    Zakończone --> [*]
📹 Klip (wideo)
stateDiagram-v2
    [*] --> Przetwarzany: prześlijKlip()
    Przetwarzany --> Opublikowany: zapisz()
    Przetwarzany --> Odrzucony: błądFormatu()
    Opublikowany --> Wyróżniony: przyznajOdznakę()
    Opublikowany --> [*]: usuń()
    Wyróżniony --> [*]: usuń()
    Odrzucony --> [*]
👤 Użytkownik
stateDiagram-v2
    [*] --> Niezweryfikowany: zarejestruj()
    Niezweryfikowany --> Aktywny: potwierdźEmail()
    Aktywny --> Zablokowany: nałóżShadowban()
    Zablokowany --> Aktywny: zdejmijShadowban()
    Niezweryfikowany --> [*]: brakAktywacji(24h)
    Aktywny --> [*]: usuńKonto()
    Zablokowany --> [*]: usuńKonto()
🧰 Technologie
(Na podstawie funkcjonalności i wymagań projektowych)

Backend: Node.js / Python (Django/FastAPI)

Frontend: React.js / Vue.js

Baza danych: PostgreSQL (zarządzanie użytkownikami, metadanymi) + S3 (przechowywanie wideo)

CI/CD: Docker, GitHub Actions / GitLab CI

Zarządzanie plikami: FFmpeg (przetwarzanie i przycinanie wideo)

📜 Licencja
Projekt SkateFlow jest udostępniany na licencji MIT.
Szczegółowe informacje znajdują się w pliku LICENSE.

<div align="center"> <sub>Dokumentacja projektowa opracowana na podstawie diagramów UML i specyfikacji przypadków użycia.</sub> </div> ```