
Bedste praksis
==============

::
    Her beskrives kort hvordan man får en god start på et nyt Python
    projekt. Her kan fx berøres hvordan en Python pakke bedst
    organiseres (mappestruktur osv), opsætning af test-framework og
    dokumentation. Her kan med fordel tages udgangspunkt i ”Template
    Repositories” der udvikles i forbindelse med milepæl 4.1.


Antagelser
----------

Formål: at beskrive nuværende, bedste praksis for brug af Python som programmeringssprog til genrbugelig kode [biblioteker] og egentlige programmer [applikationer, programmel], der løser specifikke opgaver.

Modtager: En forretningskendt udvikler, der skal bygge nyt eller vedligeholde/opdatere gammelt programmel, og som derfor har brug for at vide, hvordan vi tænker over og implementerer forretningskritiske løsninger i Python.


Grundlæggende bregreber, mekanismer og værktøjer
------------------------------------------------

Antagelser: Ikke nødvendigt at nævne, hvad Python er. Brug korte, konkrete beskrivelser af processerne, og hvad de enkeltvis kræver af 'økosystemet'

*   Python-distribution

*   Installations-værktøjer
    *   [PIP](https://pip.pypa.io/)
    *   `conda` (undgå)
    *   `mamba` (fordele)

*   Andre, konkrete værktøjer
    *   SetupTools

*   Mappestruktur til understøttelse af forskellige processer i programmel-livscyklussen.
    *   Fordi vi bør teste en installeret version af koden, lægger vi kildekoden i `src`.
    *   Fordi brugeren ikke behøver at teste koden selv, skal test-funktionalitet afsondres fra kildekoden.
    *   Fordi processen med at arbejde med kildefiler som programmelkode, testkode, scripts og dokumentation afføder en masse midlertidige filer eller slutprodukter [artefakter] til distribution andetsteds, skal versionsstyringssystemet konfigureres til at ignorere disse, så der ikke er for meget støj i arkivet.
    *   ...

*   Python-projektfiler
    *   Organisation: Bibliotek eller applikation?
    *   Versionsstyring og alle de filer, der hører til denne.
    *   Python-projekt-filer i et versionsstyret arkiv
        *   Pyproject.toml
        *   setup.cfg
        *   **NÆSTE SKRIDT: PEP 621 og flytning af metadata fra setup.cfg til pyproject.toml**

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
    *   `wheel`

*   Distribution?

build metadata and project metadata,

Fremgangsmåde
-------------

*   Installér nyeste version af Python med MambaForge
*   Opret et moderne Python-projekt
    *   Brug `pyproject.toml`
        *   Denne konfoiurationsfil bruges til basal opsætning af
            projektet og de værktøjer, der kan læse deres
            konfiurationer i denne, eksempelvis `black` og `pytest`.
    *   Brug `setup.cfg`
    *   Bemærk, at der efter setuptools>=43.0.0 ikke er behov for en
        setup.py-fil.

---

Udeståender
-----------

*   Hvordan vedligeholder (udgiver og versionerer) man et python-projekt med to eller flere pakker i src?

