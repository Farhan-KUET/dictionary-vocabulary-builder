# Dictionary \& Vocabulary Builder

A JavaFX desktop application for looking up English words and building a personal vocabulary list. Word definitions are fetched from a public dictionary API, parsed from JSON, and saved to a local SQLite database.

|||
|-|-|
|**Author**|Farhan Shariar|
|**Roll**|2307113|
|**Course No**|CSE-2200|
|**Institution**|Khulna University of Engineering \& Technology (KUET)|

\---

## Features

* **Word search** – enter an English word to see its phonetics, part of speech, definition, example sentence and synonyms.
* **Responsive UI while searching** – network requests run on a background thread pool, so the window never freezes. A progress indicator and status bar show what is happening.
* **Save to vocabulary** – store a searched word in a local SQLite database. Duplicates are detected and rejected.
* **Saved Vocabulary tab** – browse saved words alphabetically, filter them by keyword, and view full details in a split view.
* **Edit and delete** – update a saved entry through a dialog, or delete it after a confirmation prompt.
* **API failover** – if the primary dictionary API is unreachable or has no result, the app automatically falls back to the Datamuse API.
* **Input validation** – search terms are sanitized and validated before any request is sent.
* **Styled interface** – custom look and feel through `style.css`.
* **Unit tests** – JUnit 5 tests for database CRUD, JSON parsing and validation.

\---

## Technologies

|Area|Technology|
|-|-|
|Language|Java 21|
|GUI|JavaFX 21.0.2 (`javafx-controls`, `javafx-graphics`)|
|Database|SQLite via `sqlite-jdbc` 3.45.1.0 (JDBC, `PreparedStatement`)|
|JSON|Gson 2.10.1|
|HTTP|`java.net.http.HttpClient`|
|Concurrency|`ExecutorService` (fixed thread pool), JavaFX `Task`, `Runnable`, `ReentrantLock`|
|Build|Apache Maven (compiler, javafx-maven-plugin, surefire, shade)|
|Testing|JUnit 5.10.2|
|Version control|Git and GitHub|

\---

## Course Topic Coverage (CSE-2200)

|Week|Topic|Where it appears in this project|
|-|-|-|
|1|Java syntax and core OOP|Classes, objects, encapsulation, collections and control flow across `model/`, `service/` and `util/`. Model classes: `DictionaryEntry`, `Meaning`, `Definition`, `Phonetic`, `SavedWord`.|
|2|Git and version control|Project is hosted on GitHub with a `.gitignore`. See the *Commit History* section below.|
|3|JavaFX GUI|`DictionaryApplication` (Stage, Scene, CSS) and `MainController`, which builds the UI with `BorderPane`, `TabPane`, `SplitPane`, `ScrollPane`, `VBox`, `HBox`, `GridPane`, `ListView`, `TextField`, `TextArea`, `Button`, `Label`, `ProgressIndicator`, `Alert` and `Dialog`. Events are handled with lambda handlers.|
|4|Multithreading and concurrency|`Executors.newFixedThreadPool(4)` and a `Runnable` in `MainController`, `SearchTask extends Task`, `Platform.runLater` for UI updates, and a shared `ReentrantLock` guarding all database access.|
|5|Quiz|Not applicable to the project.|
|6|SQLite and JavaFX|`DatabaseManager` creates the `saved\_words` table. `DatabaseService` implements INSERT, SELECT (including a `WHERE` keyword filter), UPDATE and DELETE using `PreparedStatement`.|
|7|JSON parsing and API handling|`DictionaryService` builds the API URL, sends the request with `HttpClient`, checks the status code and passes the body to `JsonParser`, which uses Gson to convert JSON into Java objects.|

\---

## Database

The application uses a single SQLite table, created automatically on first launch in `data/dictionary.db`:

```sql
CREATE TABLE IF NOT EXISTS saved\_words (
    id             INTEGER PRIMARY KEY AUTOINCREMENT,
    word           TEXT NOT NULL UNIQUE,
    phonetic       TEXT,
    part\_of\_speech TEXT,
    definition     TEXT,
    example        TEXT,
    synonyms       TEXT,
    created\_at     TIMESTAMP DEFAULT CURRENT\_TIMESTAMP
);
```

The `data/` folder is created at runtime and excluded from Git.

\---

## Project Structure

```
dictionary-vocabulary-builder/
├── pom.xml
├── README.md
├── LICENSE
├── .gitignore
└── src/
    ├── main/
    │   ├── java/com/dictionaryapp/
    │   │   ├── Main.java                     # Launcher class
    │   │   ├── DictionaryApplication.java    # JavaFX Application (Stage, Scene, CSS)
    │   │   ├── controller/
    │   │   │   └── MainController.java       # UI construction, events, thread pool
    │   │   ├── database/
    │   │   │   └── DatabaseManager.java      # Connection, table creation, shared lock
    │   │   ├── model/
    │   │   │   ├── DictionaryEntry.java
    │   │   │   ├── Meaning.java
    │   │   │   ├── Definition.java
    │   │   │   ├── Phonetic.java
    │   │   │   └── SavedWord.java
    │   │   ├── service/
    │   │   │   ├── DictionaryService.java    # HTTP requests and API failover
    │   │   │   ├── DatabaseService.java      # CRUD operations
    │   │   │   └── SearchTask.java           # Background search Task
    │   │   └── util/
    │   │       ├── JsonParser.java           # Gson-based JSON to Java objects
    │   │       └── ValidationUtil.java       # Input sanitizing and validation
    │   └── resources/
    │       └── style.css
    └── test/java/com/dictionaryapp/
        ├── service/DatabaseServiceTest.java
        └── util/
            ├── JsonParserTest.java
            └── ValidationUtilTest.java
```

\---

## Requirements

* JDK 21 or newer
* Apache Maven 3.9 or newer
* Internet connection (for dictionary API requests)

\---

## How to Run

**IntelliJ IDEA**

1. Open the project folder; IntelliJ imports `pom.xml` automatically.
2. Open `src/main/java/com/dictionaryapp/Main.java`.
3. Run `Main.main()`.

**Command line**

```bash
mvn clean test        # run unit tests
mvn javafx:run        # launch the application
mvn clean package     # build an executable JAR in target/
```

\---

## How to Use

1. Open the **Dictionary Search** tab, type a word and press Enter or click Search.
2. Review the definition, phonetics, example and synonyms, then click **Save** to add the word to your vocabulary.
3. Open the **Saved Vocabulary** tab to browse, filter, edit or delete saved words.

\---

\---

## License

Released under the MIT License. See [LICENSE](LICENSE).

