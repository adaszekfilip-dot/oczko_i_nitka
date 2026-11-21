# Oczko i Nitka — sklep z rękodziełem na szydełku

Responsywna strona sklepu do publikacji na GitHub Pages. Katalog obsługuje różne rodzaje rękodzieła: maskotki, breloczki, kwiaty, torebki, ubrania i dodatki, ozdoby oraz kategorię „Inne”. Właścicielka może dodawać, edytować i usuwać produkty, dodawać zdjęcia oraz obsługiwać zamówienia. Dane są przechowywane w Firebase.

## Pliki

- `index.html` — strona sklepu i panel właścicielki.
- `firebase-config.js` — konfiguracja projektu Firebase i UID właścicielki.
- `firestore.rules` — reguły bazy produktów i zamówień.
- `storage.rules` — reguły zdjęć w Firebase Storage.

## 1. Podłącz Firebase — krok po kroku

### Krok 1: utwórz aplikację WWW i skopiuj konfigurację

1. Otwórz [Firebase Console](https://console.firebase.google.com/) i wybierz swój projekt (albo utwórz nowy).
2. Kliknij zębatkę obok „Project Overview” → **Project settings** → zakładka **General**.
3. W sekcji **Your apps** dodaj aplikację WWW przyciskiem `</>` (jeśli jeszcze jej nie ma).
4. Nadaj jej nazwę, np. `Oczko i Nitka`.
5. Firebase pokaże obiekt `firebaseConfig`. Skopiuj jego wartości.
6. Otwórz plik `firebase-config.js` i zastąp teksty `WKLEJ_TUTAJ...` prawdziwymi wartościami z Firebase. Nie zmieniaj nazw pól.

Przykładowo plik powinien wyglądać mniej więcej tak (to tylko przykład — użyj swoich danych):

```js
export const firebaseConfig = {
  apiKey: "TWÓJ_API_KEY",
  authDomain: "twoj-projekt.firebaseapp.com",
  projectId: "twoj-projekt",
  storageBucket: "twoj-projekt.firebasestorage.app",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};

export const ADMIN_UID = "UID_TWOJEGO_KONTA";
```

Nie kopiuj przykładowych wartości powyżej. Wklej własny obiekt z konsoli. Firebase web config nie jest hasłem administratora; bezpieczeństwo zapewniają reguły dostępu. Nigdy nie umieszczaj hasła do konta w plikach strony.

### Krok 2: włącz logowanie właścicielki

1. W Firebase otwórz **Build → Authentication** i kliknij **Get started**.
2. W zakładce **Sign-in method** włącz **Email/Password** i zapisz.
3. Otwórz **Users** → **Add user**. Utwórz konto swoim adresem e-mail i silnym hasłem.
4. Kliknij utworzone konto i skopiuj jego **UID**.
5. Wklej UID w `firebase-config.js` w miejsce `WKLEJ_TUTAJ_UID_ADMINA`.
6. Ten sam UID wklej w pliku `firestore.rules` oraz `storage.rules`, zastępując tam `WKLEJ_TUTAJ_UID_ADMINA`.

### Krok 3: utwórz bazę Firestore

1. Otwórz **Build → Firestore Database** → **Create database**.
2. Wybierz lokalizację bazy i utwórz ją.
3. Wejdź w zakładkę **Rules**.
4. Zastąp zawartość reguł zawartością pliku `firestore.rules` (z już wstawionym UID właścicielki).
5. Kliknij **Publish**.

### Krok 4: skonfiguruj Storage na zdjęcia

1. Otwórz **Build → Storage** → **Get started** i utwórz bucket.
2. Otwórz zakładkę **Rules**.
3. Wklej reguły z pliku `storage.rules` (z już wstawionym UID właścicielki) i kliknij **Publish**.
4. Sprawdź w `firebase-config.js`, czy `storageBucket` jest dokładnie wartością podaną w konfiguracji aplikacji WWW.

**Ważne o kosztach:** Firebase Storage w niektórych nowych projektach może wymagać planu rozliczeniowego Blaze. Przed aktywacją sprawdź warunki i limity w swojej konsoli Google/Firebase. Nie włączaj płatnego planu, jeśli nie akceptujesz możliwych opłat.

### Krok 5: opublikuj pliki na GitHub

1. Utwórz repozytorium na GitHub.
2. Wgraj do jego głównego katalogu wszystkie cztery pliki strony i konfiguracji: `index.html`, `firebase-config.js`, `firestore.rules`, `storage.rules`.
3. Otwórz **Settings → Pages**.
4. W sekcji **Build and deployment** wybierz **Deploy from a branch**, gałąź `main` i folder `/ (root)`, a następnie kliknij **Save**.
5. Po wdrożeniu otwórz adres strony pokazany przez GitHub Pages.

Nie uruchamiaj strony dwuklikiem z pliku `index.html` (`file://`). Moduły Firebase wymagają otwarcia strony przez serwer, np. GitHub Pages.

### Krok 6: dodaj adres strony do Firebase Authentication

Otwórz Firebase → **Authentication → Settings → Authorized domains**. Dodaj domenę, na której działa strona, np. `twoj-login.github.io` (bez `https://` i bez ścieżki). Jeśli później podepniesz własną domenę, dodaj również np. `www.twojsklep.pl` oraz `twojsklep.pl`, jeśli będziesz korzystać z obu adresów.

## 2. Jak wejść do panelu właścicielki

1. Otwórz opublikowaną stronę sklepu.
2. Kliknij **Panel właściciela** i zaloguj się e-mailem oraz hasłem utworzonym w Firebase Authentication.
3. Po zalogowaniu pojawi się pasek właścicielki. Kliknij **Zarządzaj sklepem**.
4. W formularzu wybierz kategorię, wpisz nazwę, cenę i opis, dodaj zdjęcie oraz ustaw dostępność.
5. Niżej możesz edytować lub usuwać produkty i zmieniać statusy zamówień.

Produkty i zamówienia są wspólne dla wszystkich odwiedzających, ponieważ zapisują się w Firebase. Starsze produkty bez kategorii będą wyświetlane w kategorii „Maskotki”.

## 3. Ładniejszy adres strony — bez nazwy repozytorium

Masz dwie opcje:

### Opcja A: darmowy adres GitHub Pages

Jeżeli repozytorium nazwiesz dokładnie `TWOJ_LOGIN.github.io`, strona będzie dostępna pod adresem `https://TWOJ_LOGIN.github.io/` — bez końcówki `/nazwa-repozytorium/`. To nadal adres z nazwą Twojego loginu.

### Opcja B: własna domena, np. `www.twojsklep.pl`

To najlepsza opcja, jeśli chcesz, żeby adres wyglądał jak prawdziwy sklep.

1. Kup domenę u wybranego rejestratora domen (np. z końcówką `.pl`).
2. Najpierw na GitHub otwórz repozytorium sklepu → **Settings → Pages**.
3. W polu **Custom domain** wpisz wybraną domenę, np. `www.twojsklep.pl`, i kliknij **Save**.
4. Zaloguj się do panelu firmy, u której kupiono domenę, i otwórz ustawienia DNS.
5. Dodaj rekord **CNAME**:
   - typ: `CNAME`
   - nazwa/host: `www`
   - wartość/cel: `TWOJ_LOGIN.github.io` (bez `https://` i bez nazwy repozytorium)
6. Jeśli chcesz też obsługiwać adres bez `www` (np. `twojsklep.pl`), skonfiguruj domenę główną według instrukcji GitHub Pages. Dla rekordu typu `A` GitHub podaje obecnie te adresy IPv4:
   - `185.199.108.153`
   - `185.199.109.153`
   - `185.199.110.153`
   - `185.199.111.153`
7. Wróć do **Settings → Pages**. Gdy DNS się zaktualizuje, włącz **Enforce HTTPS**, jeśli GitHub pokaże tę opcję.
8. W Firebase Authentication dodaj domenę do **Authorized domains** (krok 6 wyżej).

Zmiany DNS mogą zacząć działać szybko, ale czasem propagują się do 24 godzin. Nie kasuj rekordu CNAME dodanego przez GitHub w repozytorium.

## 4. Ważne przed rozpoczęciem sprzedaży

- Strona przyjmuje zamówienia, ale nie obsługuje płatności online. Po otrzymaniu zamówienia skontaktuj się z klientem w sprawie płatności i dostawy.
- Przed sprzedażą uzupełnij wymagane informacje o sprzedawcy, regulamin, politykę prywatności oraz zasady dostawy i zwrotów.
- Nie udostępniaj nikomu hasła do konta właścicielki.
- Reguły Firebase muszą być opublikowane — nie zostawiaj publicznej bazy w trybie testowym.
- Warto ustawić alerty budżetowe i monitorować użycie usług Firebase/Google Cloud.

## Oficjalne instrukcje

- [Dodawanie Firebase do aplikacji WWW](https://firebase.google.com/docs/web/setup)
- [Logowanie e-mail i hasło w Firebase](https://firebase.google.com/docs/auth/web/start)
- [Własna domena w GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
