# Tika-metadata-tool

## Table of Contents

- [Achtergrond informatie](#achtergrond-informatie)
- [Installatie](#installatie)
- [Gebruik](#gebruik)
- [Docker](#docker)
- [Feature suggesties en bugs](#feature-suggesties-en-bugs)
- [Auteurs](#auteurs)
- [Licentie](#licentie)

## Achtergrond informatie

De Metadata Tool is een Python-gebaseerde applicatie voor het extraheren, visualiseren en exporteren van metadata uit digitale bestanden. De tool is ontwikkeld voor archieven die in tijden van digitalisering steeds meer te maken krijgen met het metadatateren van bestanden. Met behulp van Apache Tika worden metadata automatisch uit bestanden geëxtraheerd en opgeslagen als sidecar-bestanden. De geëxtraheerde metadata kan vervolgens worden gevisualiseerd en geanalyseerd via een interactieve gebruikersinterface, en geëxporteerd naar Excel voor verdere verwerking. De tool is zo veel mogelijk met flexibiliteit in gedachten ontworpen, het idee is dat gebruikers kunnen zelf bepalen welke metadata velden worden meegenomen, hoe de output wordt gestructureerd, en welke bestandstypen worden verwerkt.

## Installatie

Voor het gebruik van de Metadata Tool is het volgende vereist:

* Python 3.10 of hoger: te downloaden via python.org

* Java 8 of hoger: (vereist voor Apache Tika) te downloaden via java.com

* Apache Tika: voor het extraheren van metadata. Raadpleeg de officiële Tika documentatie voor installatie-instructies. [Download](https://archive.apache.org/dist/tika/3.3.2/tika-app-3.3.2.jar) en plaats het tika-app-3.3.2.jar bestand in de hoofdmap van het project.

**Python packages**

Installeer de benodigde Python packages via pip:

```bash
pip install -r requirements.txt
```

## Gebruik

De applicatie wordt gestart via de terminal:

```bash
streamlit run app.py
```

De interface bestaat uit vier tabbladen:

### 1. Genereren metadata (Tika)

In dit tabblad wordt Apache Tika gebruikt om metadata uit bestanden te extraheren. De gebruiker specificeert een root directory en de tool leest de metadata recursief uit. De geëxtraheerde metadata wordt standaard opgeslagen als sidecar-bestand naast het originele bestand. Optioneel kan een alternatieve outputmap worden opgegeven.

### 2. Metadata visualisatie

Dit tabblad biedt een interactieve omgeving voor het analyseren en exporteren van de geëxtraheerde metadata in Excel formaat. De gebruiker kan:

- Specifieke DataFrames selecteren en exporteren naar Excel
- Een voorbeeld van een DataFrame bekijken in de interface
- Beschrijvende statistieken toevoegen aan de output
- Een steekproef nemen van de data
- Duplicaten tussen kolommen analyseren

### 3. Metadata selectie

In dit tabblad worden de geëxtraheerde metadata velden gefilterd en opgeslagen als `.metadata.json` of `.metadata.yaml` sidecar-bestanden. De selectie is gebaseerd op vooraf gedefinieerde metadata structuren per bestandstype. (Zie class variables `MetaDataPipeline` in `src/metadata_pipeline.py`)

### 4. Metadata files verwijderen

Dit tabblad biedt functionaliteit voor het verwijderen van gegenereerde metadata bestanden. Standaard worden alleen `.metadata.json` bestanden verwijderd. Optioneel kunnen ook alle `.json` bestanden of `.yaml` bestanden worden verwijderd.

## Docker

De tool kan ook via Docker worden gedraaid. Hiermee zijn Python, Java (OpenJDK 21) en alle packages automatisch beschikbaar.

**Vereisten**

* [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Windows/Mac) of Docker Engine (Linux)

**Stap 1: Clone de repository**

```bash
git clone https://github.com/GrA-Cain/tika-metadata-extraction-tool.git
cd tika-metadata-extraction-tool
```

**Stap 2: Pas het volume-pad aan in `docker-compose.yml`**

Open `docker-compose.yml` en vervang het pad onder `volumes` door de lokale map met de bestanden die je wilt verwerken:

```yaml
volumes:
  - C:\Users\jij\archiefbestanden:/data   # Windows
  - /home/jij/archiefbestanden:/data      # Mac/Linux
```

**Stap 3: Start de container**

```bash
docker compose up --build
```

De applicatie is daarna bereikbaar via: **http://localhost:8501**

In de Streamlit UI gebruik je `/data` als root directory — dit verwijst naar de lokale map die je in stap 2 hebt ingesteld.

**De container stoppen**

```bash
docker compose down
```

**Volgende keren starten (zonder rebuild)**

```bash
docker compose up
```

# Backlog

## 🐛 Bugs

- [ ] Output dir bug: `metadata_genereren()` output alle bestanden ook in root directory — zie `metadata_pipeline.py`
- [ ] Duplicaten analyse mogelijk niet geoptimaliseerd voor grote datasets (>200 files, ongetest)

## ✨ Features

- [ ] UI: `.metadata.json` output configureerbaar maken zonder in de code te duiken.
- [ ] UI: Groups/namespaces beheren (toevoegen/verwijderen) zonder code aanpassing en determinologie uniformeren.
- [ ] Sample fractie voor DataFrame previews.
- [ ] Apache Tika CLI parameters configureerbaar in UI.
- [ ] Meer bestandsformaten ondersteunen + gebruikerskeuze welke meegenomen worden.
- [ ] Alternatieve output map instellen voor `.metadata` bestanden.

### Auteurs

[@Marco Venema]([https://github.com/marcovenema](https://github.com/mvenema94))
[@Cain Weideman](https://github.com/GrA-Cain)

## Licentie
De software wordt door de Groninger Archieven beschikbaargesteld onder een GPL v3.0 licentie. Zie [LICENSE](https://github.com/GroningerArchieven/tika-metadata-extraction-tool/blob/master/LICENSE). Op eventueel meegeleverde componenten zijn de daartoe behorende licenties van toepassing.
