==============
Bedste praksis
==============

Jævnfør ordbogen betyder ordet praksis en virkeliggørelse af noget tænkt eller planlagt. Det er noget, vi udøver eller gør. Bedste praksis er altså den bedste virkeliggørelse af noget, vi ønsker at opnå. Det er den bedste måde at få frem på for at opnå dét, vi ønsker at opnå.

Følgende beskriver, hvordan man får en god start på et nyt Python-projekt. Det centrale spørgsmål er **Hvordan er vejen til at opbygge og vedligeholde en velorganiseret Python-pakke?**. Præsentationen går også gradvist frem således, at pakkens komponenter og hjælpemidler bliver beskrevet i en rækkefølge, som er hensigtsmæssig, når man bygger en pakke op fra bunden.

*Objektet*

Konkret forsøger vi her at beskrive den nuværende, bedste praksis for programmeringssproget Python. Vejledningen kan både bruges til opbygning af genbrugelig kode [biblioteker] og programmer [applikationer, programmel]. Indtil videre skelner denne vejledning ikke mellem bibliotek og applikation, da organisationen af kildekode, dokumentation, etc. i projektet som udgangspunkt kan foregå på samme måde langt hen ad vejen.

*Informationskilder*

Bedste praksis er dels udtrykt ved standarder, som defineres og implementeres i organiserede fællesskaber som The Python Software Foundation, men må også tilpasses de konkrete behov og begrænsninger, som er tilstede, når man skal gøre det bedste.

Ved at komme så tæt på standarder (idealer såvel som tekniske krav) og alment gældende praksis, sikrer vi, at vi bruger vores værktøj og midler på den bedste mulige måde, når vi skal understøtte vores andre kerneopgaver med effektiv programmel.

Følgende er derfor et nedslag i nuværende bedste praksis set fra Python-organisationens synsvinkel, herunder standard-værktøjs-udviklere (SetupTools, PIP), open-source-organisationer som folkene bag MambaForge, samt styrelsens og vores egne personlige erfaringer.

.. Python-koden skal bygges, testes og dokumenteres, så udviklere kan vedligeholde koden, og brugere kan bruge det.

.. *   Byg (udvikling) / dokumentér / test / brugertest
.. *   Udgivelse: Byg pakke (pakke-1.2.10) og distribution (publicering)
.. *   Ibrugtagning: Brugerinstallation, udrulning til miljøer som test, præproduktion og produktion.

*Synsvinkel*

Som nævnt er synsvinklen valgt, så beskrivelsen af bedste praksis har fokus på den gradvise overgang fra start til slutprodukt. Vi ser altså på fremgangsmåder og processer, vi som udviklere gennemgår, inden vi kommer frem til slutprodukterne. Med et proces-perspektiv giver vi også plads til at nævne mere generelle overvejelser og metoder, da de hænger naturligt sammen med mere Python-specifikke arbejdsgange.

*Afgrænsning*

Det er ikke formålet at gå i dybden med Pythons metodologi, økosystem, hovedorganisation og andre bidragende organisationer.

Python-udvikling involverer en rækker værktøjer og processer, der ikke har med Python-programmering at gøre. Eksempelvis bruger vi Git til versionsstyring, og med GitHub-platformen har vi en central placering til den autoritative historik for arkiverne. Platformen kan desuden bruges som central udviklingsserver, der automatisk kan kvalitetssikre Python-koden, bygge dokumentation og andet. Her beskriver vi kun, hvordan man bruger værktøjer som disse fra et Python-synspunkt.

**Det primære fokus er de standarder, konventioner og værktøjskombinationer, der virker godt idag.**


Underspørgsmål/processer
------------------------

*   Hvordan skal jeg organisere projektets filer og data i mapper under projektet [mappestruktur]?

*   Hvordan skal pakken installeres?

    *   Fra et stabilitetssynspunkt: reproducérbare installationer
    *   Fra brugerens synspunkt: ...

*   Hvad er den minimale konfiguration, der skal til for at starte et nyt Python-projekt?

*   Hvordan sætter jeg test-funktionalitet op?

*   Hvordan organiserer jeg dokumentation i koden og vejledninger?

*   Hvordan gør jeg koden mere læsbar?

    *   Kildekode: *typing annotations* i Python
    *   Test-kode: tydelige, forståelige, beskrivende funktionsnavne

Processer
---------

*   Dokumentation

    *   Design-dokumentation
    *   Brugsscenarier

*   (Givet forståelse for opgaven) Skriv testene først [*en.* test-driven developement, TDD].

    *   Nøgleord: Refaktorisering, teknisk gæld
    *   Refaktorisering: Alle (enheds)tests tester én ting. De er den første grund til at refaktorisere.

*   Installér pakker

    *   Virtuelt miljø med ``mamba``

        *   Bruger-opsætning
        *   Udvikler-opsætning

    *   Installation af pakken med ``pip``

        *   Installér det rigtige sted med ``python -m pip install --upgrade pip``
        *   Opdatér ``pip`` med ``python -m pip install --upgrade pip`` . (``--upgrade`` kan skrives kort med ``-U``)
        *   Installér pakken lokalt med ``python -m pip install -e .`` i roden af mappen.


*   Skriv koden

    *   TDD

    *   Typing annotations

        *   http://mypy-lang.org/

*   CI-infrastruktur


Aspekter under bedste praksis
=============================

*   Sikkerhedsovervejelser

    *   Opdaterede versioner
    *   Hvordan véd jeg, om en pakke, jeg anvender er forsvarlig at bruge?

*   Test og testdækning [*en.* code coverage]

    *   tests/
    *   pytest

        *   Opsætning i pyproject.toml, herunder output til code coverage værktøj
        *   fixtures
        *   conftest.py

    *   codecov

    *   CI

        *   GitHub Actions
        *   codecov.io

*   Pakkens mappestruktur

*   Kode-dokumentation, typing, docstrings

    *   `Sphinx https://www.sphinx-doc.org/`


*   Ikke-pakke-filer (datafiler)

    *   Placering af pakkens datafiler i pakken selv
    *   Adgang til pakkens egne datafiler.

::
    The PyPA [Python Package Authority] recommends that any data files you wish to be accessible at run time be included inside the package.




Mappestruktur
=============

Generelt er valget af mappestruktur vigtig for understøttelse de forskellige processer i projekt-livscyklussen. Kildekoden er versionsstyret sammen med alt andet kildemateriale i en versionsstyret projekt-mappe på GitHub. Hver koderevision har tilknyttet dokumentation og testfunktionalitet, samt al anden konfiguration til proces-understøttelse. Formålet med at have alt samlet er, at hvor alle komponenter i projektet følges ad og for en given en Git-revision fungerer sammen.

Følgende er et eksempel på mappestrukturen for en færdig Python-pakke i et Git-arkiv:

.. code-block:: none

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
        └── package
            ├── test_module1.py
            └── test_module2.py

Bemærkninger:

*   Mappen med Python-pakkens kildekode ligger adskilt fra roden i en separat mappe `src`.

*   Test-funktionalitet er adskilt fra kildekoden, som den tester, så den ikke installeres sammen med pakken.

    *   Brugeren bør ikke have nogen grund til at teste koden.
    *   Pakke bør ikke have en masse overflødig funktionalitet med, herunder test-data.

*   Dokumentations-materiale ud over README-filen og eventuelle små-filer til dokumentation, ligger i sin egen mappe af samme årsag som med kildekoden: Dokumentationen skal også bygges og udgives.


Opsæt versionsstyring
---------------------

Vi går ikke her ind i detaljerne med at oprette et nyt Git-arkiv til pakken, men skitserer i det følgende skridt til at oprette og arbejde med Git og GitHub.

**Start et nyt arkiv**

*   Opret et nyt Git-arkiv på Github, som skal fungere som den officielle placering af din python-pakke.

    *   Sig ja til at oprette README, LICENSE og ikke mindst en standard ``.gitignore``-fil til Python.

*   I GitHub, opret en *fork* af det nye Git-arkiv til din egen GitHub-bruger.

*   Kopiér SSH-adressen til din fork'ede version af arkivet.

*   I dit udviklingsmiljø [eksempelvis din SIT-PC eller], klon din fork med SSH-adressen, så du kan arbejde lokalt med ændringerne.


**Ændringer**

Når du laver ændringer, kan processen være som følger:

*Lokalt*

*   Opret en ny branch til dine ændringer.
*   Check den nye branch ud og lav dine ændringer.
*   Skub ændringerne til din fork.

*På Github*

*   Opret et Pull-request til det centrale arkiv.
*   Hvis ændringerne kan accepteres, så lav et merge af ændringerne til det centrale arkiv.


**Første ændringer**

Begynd med at tilpasse README-filen, som GitHub automatisk oprettede for dig. Den bliver dit mest læste dokument og vises automatisk, når man tilgår arkivet på GitHub. Forklar som minimum læseren:

*   hvad projektet går ud på, hvem projektet er til for, og hvordan det skaber værdi (eksistensgrundlag),
*   hvordan man kommer igang med at bruge pakken,
*   hvordan man kan bidrage til projektet,
*   hvordan projektet vedligeholdes,


**I det følgende, bliver alle ændringer foretaget lokalt, med mindre andet er angivet.**


Opbyg mappestrukturen
---------------------

| Som udvikler
| skal du have nogle byggematerialer,
| der gør det muligt at bygge pakken.

.. Som udvikler har du to primære modtagere:

.. *   dig selv og andre udviklere på projektet
.. *   brugeren / modtageren.

Filer, som understøtter alt arbejde med kode, dokumentation, etc. ligger som hovedregel i arkivets rod eller i mapper herunder, som grupperer efter formål eller værktøj.

Disse filer og mapper er kun til brug af udvikleren og bør være adskilt fra kildekode, test-funktionalitet, dokumentation og andre slutprodukter.


**Python-miljø-opsætning**

Vi starter med at installere

Begynd med at oprette en konfigurationsfil ``environment-dev.yml`` med beskrivelsen dine afhængigheder som udvikler.

.. code-block :: yaml

    name: package-dev
    channels:
      - conda-forge
    dependencies:
      - python=3.10
      - pytest

I ovenstående eksempel navngiver vi miljøet efter pakkens navn med suffikset ``-dev`` for at vise, at dette er miljø-opsætning for udviklere af pakken.

Når nye pakker skal tilføjes, så skriv navn og version in i filen manuelt.


Konfigurationsfilen kan læses af ``mamba`` på følgende måde:

.. code-block :: none

    (base)> mamba env create -f environment-dev.yml

Og miljøet kan herefter aktiveres med:

.. code-block :: none

    (base)> mamba activate package-dev
    (package-dev)>


**Resultat**

Vi har nu adgang til Python 3.10

.. code-block :: none

    (package-dev)> python
    Python 3.10.4 | packaged by conda-forge | (main, Mar 24 2022, 17:32:50) [MSC v.1929 64 bit (AMD64)] on win32
    Type "help", "copyright", "credits" or "license" for more information.
    >>>

samt test-værktøjet ``pytest``

.. code-block :: none

    (package-dev)> pytest
    ============================= test session starts ==============================
    platform win32 -- Python 3.10.4, pytest-7.1.1, pluggy-1.0.0
    rootdir: C:\Users\B088195\Desktop\git\package
    collected 0 items

    ============================ no tests ran in 0.01s =============================

    (package-dev)>

, som vi kommer tilbage til nedenfor.




.. warning:: Eksempel på ikke-anbefalet praksis

    Vi undlader at bruge ``mamba`` til at oprette miljø-konfigurationsfilen, fordi alle afhængigheder til de pakker, vi eksplicit skrev ovenfpr kommer med. Samtidig tilføjer kommandoen også en linje ``prefix:`` med konkret placering af miljøet på maskien, hvor nedenstående kommando blev skrevet.

    Til reference er her skridtene til at lade ``mamba`` oprette miljø-filen:

    *   Opret et miljø til udvikling af pakken, her kaldet ``package``:

        .. code-block :: none

            (base)> mamba create -n package-dev

    *   Aktivér miljøet

        .. code-block :: none

            (base)> mamba activate package-dev
            (package-dev)>

    *   Opret en mamba-miljø-konfigurationsfil:

        .. code-block :: none

            (package-dev)> mamba env export -f environment-dev.yml



Brugeren og installation
------------------------

For brugeren er kun produktet og den brugervendte dokumentation relevant.

I eksemplet, vi bygger op her, beder vi brugeren om at hente kodearkivet ned med Git. Her skal brugeren først checke koden ud og dernæst manuelt oprette et miljø og installere de pakker (Afhængigheder), som vores program skal bruge. Python er forudsat installeret hos brugeren, og det er antaget, at brugeren kan bruge det.

Man kan i ovenstående tilfælde distribuere koden til et pakke-arkiv som the Python Package Index (PyPI). For brugeren ville det derfor være væsentligt lettere at installere pakken i et arbitrært mamba-miljø.

Der kan være flere grunde til, at vi ikke distribuerer koden til et (globalt) Python-pakke-arkiv. Én årsag kan være, at vi kan have brug for, at brugeren tester en specifik version af koden, hvilket er nemt, hvis brugeren bare skal checke den givne version ud kortvarigt.



Python-konfigurationsfiler
--------------------------

Python-projektfiler knyttet til pakken

*   ``pyproject.toml``
*   ``setup.cfg``
*   ``environment.yml``


Test-funktionalitet



Grundlæggende begreber, mekanismer og værktøjer
================================================


Relevante processer for et Python-projekt
-----------------------------------------

Antagelser: Ikke nødvendigt at nævne, hvad Python er. Brug korte, konkrete beskrivelser af processerne, og hvad de enkeltvis kræver af 'økosystemet'

*   Python-distribution
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


Mulige, fremtidige alternativer
===============================

``setuptools_scm``
------------------

*   https://setuptools.pypa.io/en/latest/userguide/extension.html#adding-support-for-revision-control-systems

