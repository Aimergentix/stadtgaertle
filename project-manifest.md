# Projektbasis: Stadtgärtle Rheinfelden
## Website Project — Status May 2026

> **Document type:** Design Phase output (HACE §7, Phase 1).
> **Language:** English per HACE §2 (language convention). Content values in `site-content.json` are German.
> **Roles:** Archon owns this document. Aimergent reads it as a content constraint.

---

## Part A — Technical Specification (HACE-Compliant)

This section defines all schemas, Small Worlds, and interfaces **before** any code is
generated. It is the Archon's gate. Nothing in Part B (background research) may be used
to justify scope expansion beyond what is defined here.

---

### A.1 Website Purpose (Settled)

The website is a **Content Display Shell** (HACE §6). Its single function: reduce the
recurring communication burden on the Gärtnermeister  (garden master) by making rules,
schedules, and contact information publicly accessible without his direct intervention.

Non-goals (hard): authentication, member management, database, server-side logic, payment,
form processing with persistence. All of these belong to a future portal at a separate subdomain.

---

### A.2 Small Worlds — Planned Atomic Components

Each row is one commit. The Aimergent generates one file per Side-Chat session.

| #   | Small World          | File path                                      | Reads from                        |
|-----|----------------------|------------------------------------------------|-----------------------------------|
| 1   | SiteContent type     | `src/types/site-content.ts`                    | (defines shape)                   |
| 2   | Navigation type      | `src/types/navigation.ts`                      | (defines shape)                   |
| 3   | ServicesRouter type  | `src/types/services-router.ts`                 | (defines shape)                   |
| 4   | site-content.json    | `src/config/site-content.json`                 | `site-content.ts`                 |
| 5   | navigation.json      | `src/config/navigation.json`                   | `navigation.ts`                   |
| 6   | services-router.ts   | `src/config/services-router.ts`                | `services-router.ts` type         |
| 7   | Layout               | `src/components/Layout.tsx`                    | navigation.json, site-content.json |
| 8   | HeroSection          | `src/components/HeroSection.tsx`               | site-content.json                 |
| 9   | AboutSection         | `src/components/AboutSection.tsx`              | site-content.json                 |
| 10  | CalendarSection      | `src/components/CalendarSection.tsx`           | site-content.json                 |
| 11  | RulesSection         | `src/components/RulesSection.tsx`              | site-content.json                 |
| 12  | PlotMapSection       | `src/components/PlotMapSection.tsx`            | site-content.json, public/images/ |
| 13  | ContactSection       | `src/components/ContactSection.tsx`            | site-content.json, services-router|
| 14  | Footer               | `src/components/Footer.tsx`                    | navigation.json, site-content.json |
| 15  | HomePage             | `src/pages/HomePage.tsx`                       | all Section components            |

> **Scope gate:** PlotMapSection (row 12) is a static image with overlaid labels — not an
> interactive web map. Interactive mapping is explicitly deferred to a future project.

---

### A.3 `site-content.json` — Schema Skeleton

The Aimergent must generate a file that satisfies this shape exactly. Values are in German.
Keys are in English.

```typescript
// src/types/site-content.ts
interface SiteContent {
  brand: {
    name: string;           // "Stadtgärtle Rheinfelden"
    tagline: string;        // one-line description
    foundedYear: number;    // 2015
    areaSquareMeters: number; // 2000
  };
  hero: {
    heading: string;
    subheading: string;
    imageAlt: string;
  };
  about: {
    heading: string;
    body: string[];         // paragraphs as array of strings
    permacultureNote: string;
  };
  dioxinNote: {
    heading: string;
    body: string;           // why raised beds are mandatory
  };
  calendar: {
    heading: string;
    seasons: SeasonEntry[];
  };
  rules: {
    heading: string;
    items: RuleEntry[];
    compostHeading: string;
    compostRules: string[];
  };
  plotMap: {
    heading: string;
    description: string;
    imageFile: string;      // filename only, resolved to public/images/
    imageAlt: string;
  };
  contact: {
    heading: string;
    gardenMasterName: string;
    email: string;
    phone: string;
    mobile: string;
  };
  footer: {
    copyrightHolder: string;
    imprintLabel: string;
    privacyLabel: string;
  };
}

interface SeasonEntry {
  id: string;
  label: string;
  months: string;
  description: string;
}

interface RuleEntry {
  id: string;
  rule: string;
}
```

---

### A.4 `navigation.json` — Schema Skeleton

```typescript
// src/types/navigation.ts
interface NavItem {
  id: string;
  label: string;           // German display label
  href: string;            // internal anchor or page path
  external: false;
}

interface NavConfig {
  primary: NavItem[];      // main menu items
}
```

Planned primary items (labels in German, hrefs are anchor ids):
`Über uns` / `#about` · `Kalender` / `#calendar` · `Regeln` / `#rules` ·
`Beetplan` / `#plot-map` · `Kontakt` / `#contact`

---

### A.5 `services-router.ts` — Known External URLs

```typescript
// src/config/services-router.ts
// All outbound URLs in one place. No URL may appear anywhere else in the codebase.
export const services = {
  officialCityPage: "https://www.rheinfelden.de/stadtgaertle",
  cityMetzgergrube: "https://www.rheinfelden.de/de/vielseitig/Gruenes-Rheinfelden/Karl-Metzgergrube",
  urbanGardenNetwork: "https://urbane-gaerten.de/urbane-gaerten/gaerten-im-ueberblick/stadtg%C3%A4rtle-rheinfelden",
  crossiety: "https://crossiety.app/groups/13284",
} as const;

export type ServiceKey = keyof typeof services;
```

> **Not yet known:** Portal URL for member login / plot claim. Add when subdomain is defined.

---

### A.6 Information Still Required from the Gärtnermeister 

The following data is needed before the Aimergent can populate `site-content.json`.
No component that reads this data may be generated until the Archon provides the values.

| Data point           | Required for         | Status    |
|----------------------|----------------------|-----------|
| Seasonal calendar    | CalendarSection      | ⬜ Pending |
| Garden rules (full)  | RulesSection         | ⬜ Pending |
| Compost rules        | RulesSection         | ⬜ Pending |
| Plot map / floor plan| PlotMapSection       | ⬜ Pending |
| Member count / waitlist | ContactSection    | ⬜ Pending |
| Association structure | ContactSection      | ⬜ Pending |
| Season photography   | HeroSection, About   | ⬜ Pending |

---

## Part B — Background Research (Source Material)

> The following sections are the Archon's domain research. They are the source from which
> `site-content.json` values will be derived. The Aimergent reads them for content
> constraints only — not as scope justification.

---

## 1. Das Projekt im Überblick: Was ist das Stadtgärtle?

Das Stadtgärtle ist ein öffentlicher Mitmach-Gemeinschaftsgarten in der Karl-Metzgergrube in
Rheinfelden (Baden), Landkreis Lörrach, Baden-Württemberg (PLZ 79618). Es liegt an der
Mouscron-Allee, innenstadtnah und öffentlich zugänglich.

Die Fläche umfasst rund 2.000 Quadratmeter. Das Gründungsjahr ist 2015 – genauer gesagt
wurde im Herbst 2015 der erste Kürbisgarten durch die Stadtgärtnerei angelegt, das
Kürbissuppenfest im Herbst 2015 anlässlich der Eröffnung des Areals war der symbolische
Startschuss für das von Bürgern getragene Projekt.

Kontakt vor Ort: Gärtnermeister Joachim Schlageter, erreichbar unter joachim.schlageter@gmx.de,
Telefon 07761 9266127, Mobil 0171 3677164.

Offizielle städtische Webseite: https://www.rheinfelden.de/stadtgaertle

---

## 2. Die Entstehungsgeschichte

### Der Ursprung: Entente Florale und IBA Basel

Die Stadt Rheinfelden (Baden) initiierte das Stadtgärtle als nachhaltiges Projekt im Rahmen
der **Entente Florale** – einem europäischen Wettbewerb für blühende Städte und Dörfer –
der auf die Landesgartenschau Grün 07 folgte. Das Projekt entstand in enger Zusammenarbeit
mit der **IBA Basel 2020** (Internationale Bauausstellung Basel). Ein fester Bestandteil des
Areals ist die sogenannte „rote Freiraumkiste", kurz **IBA KIT** – ein Experiment-Pavilion
der IBA Basel.

Um die Bevölkerung zunächst für das Thema Gemeinschaftsgärtnern zu gewinnen, legte die
Stadtgärtnerei zuerst einen kleinen Kürbisgarten am Eingangsbereich der Karl-Metzgergrube
an. Über den Sommer konnten Passanten die Setzlinge gießen und das Wachstum verfolgen. Zum
Kürbissuppenfest der Bürgerstiftung im Herbst 2015 wurden die Kürbisse als Suppe verspeist –
und damit war das bürgergetragene Stadtgärtle geboren.

### Wachstum und Internationalisierung

Seit dem Frühjahr 2018 wurde das Projekt unter dem Namen **„Stadtgärtle international"** um
eine Integrationskomponente erweitert. Bürgermeisterin Diana Stöcker eröffnete das zugehörige
Fest. Das Projekt entstand aus dem Wunsch, Flüchtlingsfamilien aus den Anschlussunterkünften
in den Gemeinschaftsgarten zu integrieren. Fünf weitere Hochbeete wurden gemeinsam mit
interessierten Geflüchteten angelegt. Das Konzept: Jedes Beet wird von zwei Haushalten oder
zwei Einzelpersonen gemeinsam bestellt.

### 10 Jahre Stadtgärtle (2025)

Anlässlich des zehnjährigen Bestehens veranstaltete das Stadtgärtle ein erweitertes
Rahmenprogramm beim alljährlichen Kürbissuppenfest. Oberbürgermeister Klaus Eberhardt ließ
die Erfolgsgeschichte des Projekts Revue passieren. Das Programm umfasste Führungen durch
den Garten mit Gärtnermeister the Gärtnermeister , einen Spaziergang zum Thema Artenvielfalt
in der Metzgergrube sowie Auftritte des Improtheaters und Kinderschminken.

---

## 3. Die Menschen dahinter

### the Gärtnermeister  – das Herzstück

the Gärtnermeister , Jahrgang 1964, ist das lebendige Zentrum des Stadtgärtles. Er ist
Gärtnermeister und Dipl. Permakultur-Gestalter. Er arbeitet als selbstständiger
Landschaftsgestalter, berät und begleitet Permakultur-Projekte, ist in der deutschsprachigen
Permakultur-Ausbildung aktiv und betreibt Permakultur-Feldforschung. Er leitet das Stadtgärtle
seit seiner Gründung.

**Website purpose anchor:** Die Website soll the Gärtnermeister entlasten. Er soll wichtige Hinweise
kommunizieren können, ohne sie vor Ort und wiederholt erklären zu müssen.

### Die Mitmachgärtnerinnen und -gärtner

Beim Start 2015 waren es rund 20 Aktive. Bis 2017 wuchs die Zahl auf rund 30 Bewohner
zwischen drei und 83 Jahren. Nach der Internationalisierung 2018 hatte das Projekt 37
Mitglieder. Laut aktuellem Stand des Netzwerks Urbane Gärten engagieren sich mittlerweile
fast 50 Personen im Stadtgärtle.

### Die Stadt Rheinfelden

Die Planungshoheit für die Metzgergrube liegt beim Stadtbauamt. Die Stadt stellt die
infrastrukturellen Voraussetzungen und unterstützt das Gemeinschaftsgärtnern durch die
Finanzierung der qualifizierten Begleitung (the Gärtnermeister ) sowie des gärtnerischen
Equipments.

---

## 4. Was im Garten steht und passiert

### Infrastruktur und Ausstattung

Im Stadtgärtle wird in **Hochbeeten aus Holz und Dachziegeln** gegärtnert – ein
Designprinzip, das Ortscharakter hat und den kontaminierten Untergrund durch die Erhöhung
umgeht. Weitere Elemente: Obstbäume und Beerensträucher, ein Beet für den Kindergarten
St. Michael, ein Lehmofen, Bienenkästen, Sitzbänke und Pavillons, Komposttoiletten sowie
ein Landartprojekt. Eine Boule-Gruppe trifft sich regelmäßig an der roten Freiraumkiste.

### Gärtnerischer Ansatz: Permakultur

Das Stadtgärtle arbeitet konsequent nach Prinzipien der **Permakultur** – einem ganzheitlichen
Gestaltungsansatz, der natürliche Ökosysteme nachahmt. the Gärtnermeister  ist
Diplom-Permakultur-Designer.

### Jahreshöhepunkt: Das Kürbissuppenfest

Das alljährliche Kürbissuppenfest im Herbst ist das zentrale Gemeinschaftsereignis des
Stadtgärtles. Es hat Traditions-Charakter und markiert gleichzeitig den Abschluss der
Gartensaison.

---

## 5. Der Ort: Die Karl-Metzgergrube

Die Karl-Metzger-Grube war ursprünglich eine Kies- und Sandgrube. Sie wurde **2012
wiederverfüllt** und wird seit 2015 als stadtnahes Erholungsgebiet genutzt. Das rund
**6 Hektar** große Areal entwickelt sich unter dem Konzept des **IBA KIT** (Kreativität,
Innovation, Transformation) zu einer innenstadtnahen Grünfläche neuen Typs.

---

## 6. Die Dioxin-Geschichte — ökologisches Erbe und Designkonsequenz

> **Why this matters for the website:** This section explains why raised beds are mandatory
> and why the compost project has special rules. Both are front-facing content for RulesSection.

Die Böden der Rheinfelder Innenstadt sind mit **polychlorierten Dibenzodioxinen und
Dibenzofuranen (PCDD/PCDF)** belastet. Die Ursache liegt in der industriellen Vergangenheit
Rheinfeldens als Chemiestandort.

Die Metzgergrube spielte in dieser Sanierungsgeschichte eine besondere Rolle: Seit 2010
konnte schwach dioxinbelasteter Bodenaushub (bis zu 100 ng I-TEq/kg) aus Rheinfelden zur
Verfüllung in der Grube abgelagert werden. **Seit Februar 2018 ist diese Möglichkeit
erschöpft.**

Das bedeutet im Klartext: Die Hochbeete mit eingebrachter, unbelasteter Erde sind nicht nur
ein gestalterisches Element, sondern eine **notwendige ökologische Schutzmaßnahme**.

---

## 7. Ökologische und naturschutzrechtliche Richtlinien

### Bundes-Bodenschutzgesetz (BBodSchG)

Das Bundes-Bodenschutzgesetz ist die rechtliche Grundlage dafür, warum im Stadtgärtle nicht
einfach in den anstehenden Boden gegärtnert wird.

### Bundesnaturschutzgesetz (BNatSchG)

Wild lebende Tiere dürfen nicht ohne vernünftigen Grund gefangen oder getötet werden (§39
BNatSchG). Das Stadtgärtle fördert aktiv die Artenvielfalt.

### Naturschutzrechtliche Empfehlungen

Für Gärten auf potenziell belasteten Flächen empfiehlt das Landratsamt Lörrach: kein Anbau
von Wurzelgemüse direkt im Boden, kein Umgraben des natürlichen Bodens. Das Hochbeet-Konzept
erfüllt all diese Anforderungen vollständig.

Für das Kompostprojekt: Kompost aus Gartenabfällen des Stadtgärtles selbst ist unbedenklich.
Eingebrachtes Material von außen müsste auf Belastungen geprüft werden.

---

## 8. Europäischer Vergleich

### Prinzessinnengarten, Berlin

2009 von „Nomadisch Grün" auf einer 6.000 m² Brache in Berlin-Kreuzberg gegründet.
Unterschied zum Stadtgärtle: der Prinzessinnengarten ist explizit mobil konzipiert. Das
Stadtgärtle ist dauerhafter und tiefer in die Stadtstruktur Rheinfeldens eingebettet.

### Interkulturelle Gärten (Deutschland/Europa)

Das Stadtgärtle international fügt sich in eine breitere Bewegung der interkulturellen
Gemeinschaftsgärten ein. Das erste interkulturelle Gartenprojekt Deutschlands entstand
1996 in Göttingen.

---

## 9. Sources

| Source | URL |
|--------|-----|
| Stadt Rheinfelden – Stadtgärtle | https://www.rheinfelden.de/stadtgaertle |
| Stadt Rheinfelden – Karl-Metzgergrube | https://www.rheinfelden.de/de/vielseitig/Gruenes-Rheinfelden/Karl-Metzgergrube |
| Stadt Rheinfelden – IBA KIT | https://www.rheinfelden.de/de/innovativ/Projekte-externer-Traeger/IBA-Basel-2020/KIT-Karl-Metzgergrube |
| Netzwerk Urbane Gärten | https://urbane-gaerten.de/urbane-gaerten/gaerten-im-ueberblick/stadtg%C3%A4rtle-rheinfelden |
| Badische Zeitung 2017 | https://www.badische-zeitung.de/das-stadtgaertle-in-rheinfelden-gedeiht-praechtig |
| Badische Zeitung 2019 | https://www.badische-zeitung.de/rheinfelden/das-stadtgaertle-in-rheinfelden-trotzt-dem-klimawandel |
| Verlagshaus Jaumann 2018 | https://www.verlagshaus-jaumann.de/inhalt.rheinfelden-das-stadtgaertle-ist-fuer-alle-da |
| Stadt Rheinfelden – 10 Jahre | https://www.rheinfelden.de/de/aktuell/Staedtische-Nachrichten/Staedtische-Nachricht?id=13037 |
| Landkreis Lörrach – Dioxin | https://www.loerrach-landkreis.de/de/Service-Verwaltung/Fachbereiche/Umwelt/Dioxin-DL |
| Stoll VITA Stiftung – Permakultur | https://www.stollvitastiftung.de/einfuehrungskurs-permakultur/ |
| Crossiety – Stadtgärtle Gruppe | https://crossiety.app/groups/13284 |
| Prinzessinnengarten Berlin | https://prinzessinnengarten.net/about/ |
