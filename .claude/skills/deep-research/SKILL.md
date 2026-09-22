---
name: deep-research
description: Face cercetare aprofundată (deep research) pe o temă dată de utilizator, cu mai multe runde de căutare pe internet, citire de surse și verificare încrucișată (surse care confirmă sau contrazic concluziile inițiale), apoi scrie un raport structurat într-un fișier Markdown. Folosește acest skill când utilizatorul cere "deep research", "cercetare aprofundată", "research pe tema X", un raport documentat cu surse, sau orice investigație care necesită mai multe căutări și verificare a informației, nu doar un răspuns rapid dintr-o singură căutare.
---

# Deep Research

Acest skill conduce o cercetare aprofundată, în mai multe runde, pe o temă
dată de utilizator și produce un raport structurat, cu surse citate, într-un
fișier `.md`.

## Când NU se folosește

Nu folosi acest skill pentru o întrebare simplă care se răspunde cu o
singură căutare sau din cunoștințe generale. E gândit pentru teme unde
utilizatorul vrea profunzime, verificare a surselor și un livrabil scris,
nu un răspuns rapid în chat.

## Pasul 0 — Clarifică tema și scope-ul

Dacă tema nu e suficient de clară pentru a începe căutările (prea vagă,
ambiguă, sau ar putea însemna lucruri foarte diferite), pune utilizatorului
1-2 întrebări scurte cu `AskUserQuestion` înainte să începi — de exemplu
despre unghiul dorit (ex: tehnic vs. de business), perioada de timp
relevantă, sau dacă vrea surse doar în limba engleză vs. și în alte limbi.
Nu bloca inutil pe întrebări dacă tema e deja clară — pornește direct la
cercetare.

Stabilește implicit:
- **Limba raportului** = limba în care utilizatorul a formulat tema.
- **Numele fișierului de output** = `research/<slug-tema>-<YYYY-MM-DD>.md`
  în directorul curent de lucru (creează directorul `research/` dacă nu
  există). Dacă utilizatorul indică altă locație, folosește aceea.

## Metodologie: runde de cercetare

Cercetarea se face în **2-3 runde**. Fiecare rundă are doi pași:

### Pas A — Căutare și citire (3-5 surse per rundă)

- Folosește `WebSearch` pentru a găsi 3-5 surse relevante pentru rundă.
- Pentru fiecare rezultat promițător, folosește `WebFetch` pentru a citi
  efectiv conținutul sursei (nu te baza doar pe snippet-ul din rezultatele
  căutării).
- Preferă surse primare (studii, documentație oficială, date originale,
  declarații directe) față de agregatoare secundare, dar notează și ce
  spun sursele secundare/jurnalistice când sunt relevante.
- Notează pentru fiecare sursă: titlu, URL, data publicării (dacă există),
  și 2-4 puncte cheie extrase din ea.

### Pas B — Reflecție și verificare încrucișată

După fiecare rundă de căutare, înainte să treci mai departe:

- Gândește-te explicit la ce înseamnă sursele găsite: ce afirmă cu
  certitudine, ce e speculație/opinie, ce e susținut de date vs. anecdotă.
- Identifică afirmațiile centrale din rundă și caută în mod specific
  materiale care confirmă SAU contrazic aceste afirmații (nu doar mai
  multe surse care spun același lucru). Scopul rundei următoare este să
  testeze robustețea concluziilor de până acum, nu doar să adune volum.
- Notează explicit orice dezacord găsit între surse, orice nuanță
  importantă, sau orice gol de informație rămas.
- Formulează 1-3 întrebări/goluri specifice pe care le va acoperi
  următoarea rundă de căutare.

**Prima rundă** pornește din întrebarea inițială a utilizatorului. **Fiecare
rundă următoare** pornește din golurile/dezacordurile identificate la Pasul
B al rundei anterioare — nu repeta aceleași căutări.

Rulează acest ciclu (Pas A + Pas B) de **2-3 ori în total**. Poți opri mai
devreme (minim 2 runde) dacă a doua rundă nu mai aduce informație nouă sau
contradictorie relevantă; nu depăși 3 runde fără motiv.

## Urmărirea progresului

Pe măsură ce avansezi, ține utilizatorul la curent cu update-uri scurte
(ex: "Rundă 1/3: am găsit X surse, direcție Y găsită; trec la verificare
încrucișată"). Dacă task-ul e lung, folosește `TaskCreate`/`TaskUpdate`
pentru a marca fiecare rundă ca `in_progress` → `completed`.

## Pasul final — Raportul scris

Scrie raportul într-un fișier Markdown (vezi mai sus pentru locație), cu
această structură:

```markdown
# <Titlul temei>

*Raport de cercetare generat pe <data>*

## Rezumat executiv
2-4 paragrafe cu concluziile principale, inclusiv nivelul de certitudine
(ex: "susținut consistent de surse" vs. "dovezi mixte/contestate").

## Metodologie
Câteva propoziții: câte runde de căutare, câte surse, ce s-a urmărit la
verificarea încrucișată.

## Constatări principale
Organizate pe subteme/întrebări, nu pe rundă de căutare. Pentru fiecare
constatare majoră:
- Ce spun sursele, cu link-uri inline către sursă.
- Dacă există dezacord între surse, prezintă ambele părți și, dacă poți,
  o evaluare a cărei poziții pare mai bine susținută și de ce.

## Puncte de dezacord / incertitudine
Secțiune dedicată contradicțiilor găsite între surse și golurilor de
informație rămase nerezolvate — nu le ascunde în restul raportului.

## Concluzie
Sinteză finală, răspunzând direct la întrebarea/tema inițială.

## Surse
Listă numerotată cu toate sursele folosite (titlu, autor/publicație dacă
se cunoaște, URL, data).
```

Cerințe de calitate pentru raport:
- Fiecare afirmație factuală importantă are o sursă citată inline
  (link Markdown `[text](url)`), plus intrarea corespunzătoare în secțiunea
  finală „Surse”.
- Nu prezenta o singură sursă ca fiind adevărul consensual dacă alte surse
  găsite o contrazic — semnalează explicit dezacordul.
- Scrie în limba stabilită la Pasul 0.
- La final, confirmă utilizatorului unde a fost salvat fișierul și oferă
  un rezumat de 2-3 propoziții al concluziei principale (nu duplica tot
  raportul în chat).
