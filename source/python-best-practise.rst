==============
Bedste praksis
==============

Følgende beskriver kort, hvordan man får en god start på et nyt Python-projekt. Det centrale spørgsmål er **Hvordan organiseres en Python-pakke bedst?**. Denne del svarer på spørgsmålet i forhold til følgende underspørgsmål:

*   Hvordan skal jeg organisere projektets filer og data i mapper under projektet [mappestruktur]?
*   Hvordan sætter jeg test-funktionalitet op?
*   hvordan organiserer jeg dokumentation?


Formål
======

At beskrive nuværende, bedste praksis for brug af Python som programmeringssprog til genbrugelig kode [biblioteker] og egentlige programmer [applikationer, programmel], der løser specifikke opgaver. Den anbefalede fremgangsmåde her beskrevet skelner dog ikke mellem disse to overordnede formål, da organisationen af kildekode, dokumentation, etc. i projektet som udgangspunkt kan foregå på samme måde langt hen ad vejen. Som nævnt i starten af denne vejledning, er retningslinjerne her under løbende udvikling, så en mere specialiceret vejledning kan komme i en fremtidig version.


Afgrænsning/Begrænsninger
=========================

Bedste praksis er dels udtrykt ved standarder, som defineres og implementeres i fællesskab, men må også tilpasses de konkrete behov og begrænsninger, som er tilstede, når man skal gøre det bedste.

Ved at komme så tæt på standarder (idealer såvel som tekniske krav) og alment gældende praksis, sikrer vi, at vi bruger vores værktøj og midler på den bedste mulige måde, når vi skal understøtte vores andre kerneopgaver med effektiv programmel.

Følgende er derfor et nedslag i nuværende bedste praksis set fra Python-organisationens synsvinkel, herunder standard-værktøjs-udviklere (SetupTools, PIP), open-source-organisationer som folkene bag MambaForge, samt styrelsens og vores egne personlige erfaringer.

.. Python-koden skal bygges, testes og dokumenteres, så udviklere kan vedligeholde koden, og brugere kan bruge det.

.. *   Byg (udvikling) / dokumentér / test / brugertest
.. *   Udgivelse: Byg pakke (pakke-1.2.10) og distribution (publicering)
.. *   Ibrugtagning: Brugerinstallation, udrulning til miljøer som test, præproduktion og produktion.


Mappestruktur
=============

Valget af mappestruktur vigtig for understøttelse de forskellige processer i projekt-livscyklussen. Kildekoden er versionsstyret sammen med alt andet kildemateriale i en versionsstyret projekt-mappe på GitHub. Hver koderevision har tilknyttet dokumentation og testfunktionalitet, samt al anden konfiguration til proces-understøttelse. Formålet med at have alt samlet er, at man så vidt muligt altid har en revision, hvor alle komponenter i projektet fungerer sammen, som de ser ud i den pågældende revision.

Følgende er et eksempel på mappestrukturen i et Git-arkiv:

.. code-block:: text

    package
    ├── .git
    ├── .github
    │   └── workflows
    │       └── main.yaml
    ├── docs
    │   ├── index.md
    │   └── ...
    ├── environment-dev.yml
    ├── environment.yml
    ├── LICENSE
    ├── mkdocs.yml
    ├── pyproject.toml
    ├── README.md
    ├── scripts
    │   ├── examples
    │   │   └── example1.py
    │   └── ci
    │       └── some_behaviour.sh
    ├── setup.cfg
    ├── src
    │   └── package
    │       ├── module1.py
    │       ├── module2.py
    │       └── __init__.py
    └── tests
        └── kalk
            ├── test_module1.py
            └── test_module2.py

Bemærkninger:

*   Kildekoden ligger adskilt fra roden i en separat mappe `src`. Python-pakker i projektet ligger i hver deres mappe.
*   Test-funktionalitet er adskilt fra kildekoden, som den tester, så den ikke installeres sammen med pakken.
    *   Brugeren bør ikke have nogen grund til at teste koden.
    *   Pakke bør ikke have en masse overflødig funktionalitet med, herunder test-data.
*   Dokumentations-materiale ud over README-filen og eventuelle små-filer til dokumentation, ligger i sin egen mappe af samme årsag som med kildekoden: Dokumentationen skal også bygges og udgives.


Python-projektfiler knyttet til pakken

*   pyproject.toml
*   setup.cfg




Grundlæggende begreber, mekanismer og værktøjer
================================================


Relevante processer for et Python-projekt
-----------------------------------------

Antagelser: Ikke nødvendigt at nævne, hvad Python er. Brug korte, konkrete beskrivelser af processerne, og hvad de enkeltvis kræver af 'økosystemet'

*   Python-distribution

    Vi bruger MambaForge, som gør følgende nemt for os:

    -   Pakkestyringsværktøjet `mamba` er hurtigere til at opløse afhængigheder end `conda`, der følger med Anaconda-distributionen.
    -   `mamba` bruger som standard kun det kuraterede pakke-arkiv `conda-forge` som kilde til tredjepartsbiblioteker. I modsætning til licensbetingelserne for de pakker, der kommer fra `condas` standard-kanal eller andre, så kan pakker fra `conda-forge`, så kan disse bruges frit i styrelsens egne løsninger.

    **Installation**

    -   Alle seneste versioner: https://github.com/conda-forge/miniforge/releases/latest
        +   Windows: Vælg `Mambaforge-Windows-x86_64.exe`
    -   Direkte link til seneste Windows-version: https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-Windows-x86_64.exe


*   Installations-værktøjer
    *   [PIP](https://pip.pypa.io/)
    *   `conda` (undgå)
    *   `mamba` (fordele)


*   Andre, konkrete værktøjer
    *   SetupTools


Projektfiler (konfigurationsfiler og metadata) versus egentlig kildekode (og, separat herfra, testkode)

*   Kvalitetssikring af koden

    *   Kvalitetssikring af revisioner:

        *   Pre-commit-hooks

    *   Énsretning af syntax:

        *   Black

    *   Vedligeholdelses-kvalitet

        *   `wily`

    *   Test-dækning

        *   pytest-cov

*   Pakkeværktøjer

    *   SetupTools
    *   Hvilket format? `wheel` -> .whl

*   Distribution

    *   GitHub? -> Hvordan inkluderer i setup.cfg


build metadata and project metadata,

Fremgangsmåde/checkliste
========================

*   Installér nyeste version af Python med MambaForge

*   Hav en fornuftig mappestruktur, der understøtter forskellige processer i programellets livscyklus
    *   Kildefiler
    *   ...

*   Opret et moderne Python-projekt

    *   Brug `pyproject.toml`
        *   Denne konfigurationsfil bruges til basal opsætning af
        projektet og de værktøjer, der kan læse deres
        konfiurationer i denne, eksempelvis `black` og `pytest`.

    *   Brug `setup.cfg`
    *   Bemærk, at der efter setuptools>=43.0.0 ikke er behov for en
        setup.py-fil.


Udeståender
===========

*   Branching-strategi

*   Hvordan vedligeholder (udgiver og versionerer) man et python-projekt med to eller flere pakker i src?

*   Fastholdte udviklingsafhængigheder (lock files)

    -   [conda-lock]() (virker fint med mamba)

        Installation

            pip install conda-lock
            mamba install -c conda-forge conda-lock

        https://github.com/conda-incubator/conda-lock

        https://conda-incubator.github.io/conda-lock/
