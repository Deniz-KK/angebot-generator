# GA4 Einrichtung – Schritt-für-Schritt

## Überblick

Das System sendet zwei Events an GA4:

| Event | Wann | Wer löst es aus |
|---|---|---|
| `angebot_generated` | Berater klickt "Generieren" | Generator (Berater-Seite) |
| `angebot_link_geklickt` | Kunde klickt den Link | Shopify-Weiche (Kunden-Seite) |

Beide Events enthalten dieselben Felder: `beraterin`, `kontaktkanal`, `produkt_namen`, `link_id`.

---

## Schritt 1 – Custom Dimensions in GA4 anlegen

> **Warum:** GA4 kennt unsere eigenen Felder (z.B. "beraterin") noch nicht.
> Diese müssen einmalig registriert werden.

1. Geh zu [analytics.google.com](https://analytics.google.com)
2. Property **G-8MSBJB2KWM** auswählen
3. Oben links: **Verwaltung** (Zahnrad-Symbol)
4. Unter "Property": **Custom Definitions** → **Custom Dimensions**
5. Klick auf **"Custom Dimension erstellen"** und diese 4 Dimensionen anlegen:

| Name (Anzeigename) | Scope | Parametername (genau so!) |
|---|---|---|
| Beraterin | Event | `beraterin` |
| Kontaktkanal | Event | `kontaktkanal` |
| Produkte | Event | `produkt_namen` |
| Link ID | Event | `link_id` |

> Achtung: Groß-/Kleinschreibung beim Parameternamen exakt wie in der Tabelle!

**Wichtig:** Nach dem Anlegen der Dimensionen dauert es bis zu 24 Stunden,
bis GA4 historische Daten damit befüllt. Neue Events werden sofort erkannt.

---

## Schritt 2 – Report in GA4 bauen (Exploration)

1. Linke Seitenleiste: **Explore** (Kompasssymbol)
2. Klick auf **"Blank"** (leere Vorlage)
3. Name vergeben: z.B. "Angebots-Bericht"

### Dimensionen hinzufügen
Im linken Panel unter "Dimensions" auf **"+"** klicken und auswählen:
- `Beraterin` (die Custom Dimension von oben)
- `Kontaktkanal`
- `Produkte`
- `Event Name`

### Metriken hinzufügen
Unter "Metrics" auf **"+"** klicken:
- `Event Count`

### Tabelle aufbauen
- Unter **"Rows"**: `Beraterin` und `Kontaktkanal` reinziehen
- Unter **"Columns"**: `Event Name` reinziehen
- Unter **"Values"**: `Event Count` reinziehen

### Filter setzen (nur unsere Events)
Unter **"Filters"** → **"Add Filter"**:
- Dimension: `Event Name`
- Condition: `exactly matches`
- Value: `angebot_generated` → Klick auf "+" → `angebot_link_geklickt`

---

## Schritt 3 – So sieht der fertige Report aus

| Beraterin | Kontaktkanal | angebot_generated | angebot_link_geklickt |
|---|---|---|---|
| Sofie | telefon | 12 | 9 |
| Sofie | chat | 5 | 4 |
| Merle | email | 8 | 6 |
| Katrin | telefon | 3 | 2 |

→ Die Spalte "angebot_generated" = wie viele Angebote hat die Beraterin erstellt
→ Die Spalte "angebot_link_geklickt" = wie oft hat der Kunde den Link geöffnet

### Produkte separat auswerten
Neuen Report (Exploration) anlegen, selbe Methode:
- **Rows**: `Produkte`
- **Values**: `Event Count`
- **Filter**: Event Name = `angebot_generated`

→ Zeigt welche Produkte am häufigsten in Angeboten vorkommen.

---

## Schritt 4 – Report exportieren / präsentieren

In der Exploration oben rechts: **"Share"** → Link kopieren (nur für GA4-Nutzer in der Property sichtbar)

Oder: Screenshot der Tabelle für Präsentation.

---

## Checkliste: Alles eingerichtet?

- [ ] Shopify-Seite "angebot-redirect" angelegt und Template eingebaut
- [ ] 4 Custom Dimensions in GA4 registriert
- [ ] GA4 Exploration-Report gebaut
- [ ] Testlauf: Angebot generieren → Link klicken → in GA4 "DebugView" prüfen ob Events ankommen

### DebugView nutzen (Testlauf)
GA4 → Verwaltung → **DebugView**
Dann im Generator ein Angebot erstellen → du siehst den Event `angebot_generated` sofort in Echtzeit.
Den Link öffnen → du siehst `angebot_link_geklickt` aus Shopify.
