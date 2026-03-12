---
name: finn-jobb
description: Søk etter stillinger på finn.no med filtrering på nøkkelord, sted, heltid/deltid, bransje og lønn. Bruk denne skillen når brukeren vil finne jobber på finn.no, se ledige stillinger i Norge, filtrere jobbannonser, sammenligne stillinger eller lagre favoritter. Trigger på fraser som "finn jobb", "ledige stillinger", "søk på finn", "hvilke jobber finnes", "vis meg jobber innen X", "sammenlign disse stillingene", eller liknende jobbsøk-relaterte forespørsler knyttet til finn.no. Bruk alltid denne skillen når brukeren nevner finn.no og jobb i samme kontekst.
---

# Finn.no Jobbsøk

Denne skillen guider Claude til å søke etter stillinger på finn.no via nettsøk, presentere relevante resultater og hjelpe brukeren med å filtrere, sammenligne og lagre favoritter.

---

## 1. Forstå brukerens søk

Før du søker, kartlegg disse parameterne (spør om de mangler i kontekst):

| Parameter | Eksempel |
|---|---|
| Nøkkelord/tittel | "sykepleier", "backend-utvikler", "markedskoordinator" |
| Sted/by | "Oslo", "Bergen", "remote", "hele Norge" |
| Heltid / deltid | "heltid", "deltid", "ikke spesifisert" |
| Bransje/kategori | "IT", "helse", "økonomi", "logistikk" |
| Lønn/godtgjørelser | "over 600k", "konkurransedyktig", "bonus", "pensjon" |

Hvis brukeren har gitt nok informasjon, gå rett til søk uten å spørre.

---

## 2. Søkestrategi

Bruk `web_search`-verktøyet med målrettede søk mot finn.no:

**Grunnleggende søk:**
```
site:finn.no/stillinger [nøkkelord] [sted]
```

**Med ekstra filtre:**
```
site:finn.no/stillinger [tittel] [by] heltid
site:finn.no/stillinger [bransje] [by] lønn
```

**Tips:**
- Søk 1–3 ganger med ulike varianter om første søk gir få treff
- Bruk norske synonymer (f.eks. "revisor" og "regnskapsfører")
- For lønn: finn.no viser ikke alltid lønn direkte – sjekk annonsens innhold via `web_fetch` ved behov

---

## 3. Hente annonsedetaljer

Når du har søkeresultater med lenker til finn.no-annonser, bruk `web_fetch` for å hente:
- Stillingstittel og arbeidssted
- Arbeidsgiver
- Heltid/deltid og arbeidsform (kontor, hybrid, remote)
- Lønn og godtgjørelser (om oppgitt)
- Søknadsfrist
- Kort stillingsbeskrivelse

---

## 4. Presentasjon – fleksibelt format

Tilpass presentasjonen til hva brukeren trenger:

### Enkel liste (standard ved brede søk):
```
1. **[Stillingstittel]** – [Arbeidsgiver]
   📍 [Sted] | ⏰ [Heltid/Deltid] | 💰 [Lønn hvis kjent]
   📅 Frist: [dato] | 🔗 [finn.no-lenke]
```

### Detaljert visning (ved spesifikke søk eller brukeren ber om mer):
Vis fullstendig beskrivelse med krav, arbeidsoppgaver og goder.

### Sammenligning (når brukeren vil sammenligne stillinger):
Bruk en tabell med kolonnene: Tittel | Arbeidsgiver | Lønn | Sted | Frist | Fordeler

---

## 5. Favoritter

Brukeren kan be om å lagre favoritter i samtalen. Oppretthold en liste:

```
⭐ MINE FAVORITTER
1. [Tittel] – [Arbeidsgiver] – [Lenke]
2. ...
```

Når brukeren sier "lagre denne", "legg til favoritter" eller liknende, legg annonsen til listen og vis den oppdaterte listen.

---

## 6. Sammenligning

Når brukeren ber om å sammenligne 2 eller flere stillinger:
- Hent detaljer fra hver annonse via `web_fetch`
- Presenter en sammenligningstable
- Gi en kort anbefaling basert på parameterne brukeren har nevnt som viktige

---

## 7. Begrensninger å kommunisere

- finn.no krever ikke innlogging for å se annonser, men noen detaljer kan være skjult
- Lønn er ikke alltid oppgitt i annonsen
- Søkeresultater kan være noen dager gamle – anbefal brukeren å sjekke finn.no direkte for siste status
- For å søke på en stilling, må brukeren gå til finn.no selv

---

## Eksempelflyt

**Bruker:** "Finn meg IT-jobber i Oslo innen utvikling"

1. Søk: `site:finn.no/stillinger utvikler Oslo`
2. Hent topp 5 resultater
3. Presenter som liste med tittel, arbeidsgiver, sted, lønn (om tilgjengelig), frist og lenke
4. Spør: "Vil du at jeg filtrerer mer, henter detaljer om noen av disse, eller lagrer noen som favoritter?"
