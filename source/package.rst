===============
Pakke-opbygning
===============

Jævnfør ordbogen betyder ordet praksis en virkeliggørelse af noget tænkt eller
planlagt. Det er noget, vi udøver eller gør. Bedste praksis er altså den bedste
virkeliggørelse af noget, vi ønsker at opnå. Det er den bedste måde at få frem
på for at opnå dét, vi ønsker at opnå.

Følgende beskriver, hvordan man får en god start på et nyt Python-projekt. Det
centrale spørgsmål er **Hvordan er vejen til at opbygge og vedligeholde en
velorganiseret Python-pakke?**. Teksten går også gradvist frem således,
at pakkens komponenter og hjælpemidler bliver beskrevet i en rækkefølge, som er
hensigtsmæssig, når man bygger en pakke op fra bunden.


Afgrænsning
-----------

Konkret forsøger vi her at beskrive den nuværende, bedste praksis for
programmeringssproget Python.

Vejledningen kan både bruges til opbygning af genbrugelig kode [biblioteker] og
programmer [applikationer, programmel]. Indtil videre skelner denne vejledning
ikke mellem bibliotek og applikation, da organisationen af kildekode,
dokumentation, etc. i projektet som udgangspunkt kan foregå på samme måde langt
hen ad vejen.

Python-udvikling involverer en rækker værktøjer og processer, der ikke har med
Python-programmering at gøre. Eksempelvis bruger vi Git til versionsstyring, og
med GitHub-platformen har vi en central placering til den autoritative historik
for arkiverne. Med GitHubs Actions kan platformen desuden bruges som central
udviklingsserver, der automatisk kan kvalitetssikre Python-koden, bygge
dokumentation og andet. Her beskriver vi kun, hvordan man bruger værktøjer som
disse fra et Python-synspunkt.

Indtil videre er fremgangsmåder til pakke-fremstilling [*en.* build] og
distribution udeladt. Da vi i organisationen ikke har noget officielt sted at
lægge færdigbyggede pakker, vi kan hente fra. Da vi anvender
Python-distributionen MambaForge, er det også en mulighed at bygge en pakke i
et format, der kan installeres af ``mamba``. Vi beskriver i stedet en
fremgangsmåde, hvor brugeren installerer pakken lokalt med git og ``pip``.

Det er desuden ikke formålet her at gå i dybden med Pythons metodologi,
økosystem, hovedorganisation og andre bidragende organisationer.

.. Dette kan komme med i en senere version af denne vejledning.

**Det primære fokus her er de standarder, konventioner og værktøjskombinationer,
der virker godt idag for os i SDFE.**


Informationskilder
------------------

Bedste praksis er dels udtrykt ved standarder, som defineres og implementeres i
organiserede fællesskaber som `The Python Software Foundation`_, men må også
tilpasses de konkrete behov og begrænsninger, som er tilstede i den konkrete
situation, man sidder i.

.. _`The Python Software Foundation`: https://www.python.org/psf/

Følgende er derfor et nedslag i nuværende bedste praksis set fra
Python-organisationens synsvinkel, herunder standard-værktøjs-udviklere
(SetupTools, PIP), open-source-organisationer som folkene bag MambaForge, samt
styrelsens og vores egne personlige erfaringer.

*Synsvinkel*

Som nævnt er synsvinklen valgt, så beskrivelsen af bedste praksis har fokus på
den gradvise overgang fra start til slutprodukt. Vi ser altså på fremgangsmåder
og processer, vi som udviklere gennemgår, inden vi kommer frem til
slutprodukterne. Med et proces-perspektiv giver vi også plads til at nævne mere
generelle overvejelser og metoder, da de hænger naturligt sammen med mere
Python-specifikke arbejdsgange.


Underspørgsmål/processer/indhold
--------------------------------

*   Mappestruktur: Hvordan skal jeg organisere projektets filer og data i mapper
    under projektet?

*   Hvad er den minimale konfiguration, der skal til for at starte et nyt
    Python-projekt?

*   Hvordan skal pakken installeres?

    *   Fra udviklerens synspunkt
    *   Fra brugerens synspunkt
    *   Reproducérbare installationer med miljø-konfigurationer.

*   Hvordan sætter jeg test-funktionalitet op?

*   Hvordan organiserer jeg dokumentation i koden og vejledninger?

*   Hvordan gør jeg koden mere læsbar?

    *   Kildekode: *typing annotations* i Python
    *   Test-kode: tydelige, forståelige, beskrivende funktionsnavne


Fremgangsmåde/checkliste
========================

*   Installér nyeste version af Python med MambaForge

*   Hav en fornuftig mappestruktur, der understøtter forskellige processer i
    programellets livscyklus

    *   Kildefiler i deres respektive mapper, så som ``docs/``, ``tests/`` og
        ``src/``.
    *   Hav alle andre filer og mapper i roden af projekt-mappen.

*   Opret et moderne Python-projekt

    *   Brug ``pyproject.toml``

        *   Denne konfigurationsfil bruges til basal opsætning af projektet og
            de værktøjer, der kan læse deres konfigurationer i denne,
            eksempelvis ``black`` og ``pytest``.

    *   Brug ``setup.cfg``

        *   Bemærk, at der efter ``setuptools>=43.0.0`` ikke er behov for en
            ``setup.py``-fil.




Mappestruktur og konfigurationer
================================

Generelt er valget af mappestruktur vigtig for understøttelse de forskellige
processer i projekt-livscyklussen. Kildekoden er versionsstyret sammen med alt
andet kildemateriale i en versionsstyret projekt-mappe på GitHub. Hver
koderevision har tilknyttet dokumentation og testfunktionalitet, samt al anden
konfiguration til proces-understøttelse. Formålet med at have alt samlet er, at
hvor alle komponenter i projektet følges ad og for en given Git-revision
fungerer sammen.

Konsekvensen af denne fremgangsmåde er blandt andet, at der for en given
revision af koden ikke bare findes tests, der dækker koden, men også tilhørende
dokumentation, der beskriver funktionaliteten i den pågældende revision.

Følgende er et eksempel på mappestrukturen for en færdig Python-pakke i et
Git-arkiv:

.. code-block:: none

    package
    ├── .git
    ├── .github
    │   └── workflows
    │       └── main.yaml
    ├── .gitignore
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

*   Mappen med Python-pakkens kildekode ligger adskilt fra roden i en separat
    mappe `src`.

*   Test-funktionalitet er adskilt fra kildekoden, som den tester, så den ikke
    installeres sammen med pakken.

    *   Brugeren bør ikke have nogen grund til at teste koden.
    *   Pakke bør ikke have en masse overflødig funktionalitet med, herunder
        test-data.

*   Dokumentations-materiale ud over README-filen og eventuelle små-filer til
    dokumentation, ligger i sin egen mappe af samme årsag som med kildekoden:
    Dokumentationen skal også bygges og udgives.

*   Alle andre mapper og filer er konfigurationer, scripts og andet til brug
    primært for udviklere samt for brugeren, der skal installere pakken ud fra
    arkivet.


Udviklerens synsvinkel
----------------------

| Som udvikler
| skal jeg have nogle byggematerialer,
| der gør det muligt at bygge, dokumentere og kvalitetssikre pakken.

Som udvikler har du to primære modtagere:

*   Dig selv og andre udviklere på projektet
*   Brugeren / modtageren.

Filer, som understøtter alt arbejde med kode, dokumentation, etc. ligger som
hovedregel i arkivets rod eller i mapper herunder, som grupperer efter formål
eller værktøj.

Disse filer og mapper er kun til brug af udvikleren og bør være adskilt fra
kildekode, test-funktionalitet, dokumentation og andre slutprodukter.


Brugerens synsvinkel: installation og dokumentation
---------------------------------------------------

| Som bruger
| skal jeg kunne installere og bruge pakken
| så jeg kan udføre mine egne arbejdsopgaver mere effektivt.

For brugeren er kun produktet og den brugervendte dokumentation relevant.

Her kan brugeren installere pakken ved at klone projekt-mappen ned med Git. Her
skal brugeren først checke koden ud og dernæst manuelt oprette et miljø og
installere de pakker (Afhængigheder), som vores program skal bruge. Python er
forudsat installeret hos brugeren, og det er antaget, at brugeren kan bruge
det.

Man kan i ovenstående tilfælde distribuere koden til et pakke-arkiv som the
Python Package Index (PyPI). For brugeren ville det derfor være væsentligt
lettere at installere pakken i et arbitrært mamba-miljø.

Der kan være flere grunde til, at vi ikke distribuerer koden til et
(globalt) Python-pakke-arkiv. Én årsag kan være, at vi kan have brug for, at
brugeren tester en specifik version af koden, hvilket er nemt, hvis brugeren
bare skal checke den givne version ud kortvarigt.



Opsæt versionsstyring
---------------------

Vi går ikke her ind i detaljerne med at oprette et nyt Git-arkiv til pakken, men
skitserer i det følgende skridt til at oprette og arbejde med Git og GitHub.

**Start et nyt arkiv**

*   Opret et nyt Git-arkiv på Github, som skal fungere som den officielle
    placering af din python-pakke.

    *   Sig ja til at oprette README, LICENSE og ikke mindst en standard
        ``.gitignore``-fil til Python.

*   I GitHub, opret en *fork* af det nye Git-arkiv til din egen GitHub-bruger.

*   Kopiér SSH-adressen til din fork'ede version af arkivet.

*   I dit udviklingsmiljø [eksempelvis din SIT-PC eller], klon din fork med
    SSH-adressen, så du kan arbejde lokalt med ændringerne.


**Ændringer**

Når du laver ændringer, kan processen være som følger:

*Lokalt*

*   Opret en ny branch til dine ændringer.
*   Check den nye branch ud og lav dine ændringer.
*   Skub ændringerne til din fork.

*På Github*

*   Opret et Pull-request til det centrale arkiv.
*   Hvis ændringerne kan accepteres, så lav et merge af ændringerne til det
    centrale arkiv.


**Første ændringer**

Begynd med at tilpasse README-filen, som GitHub automatisk oprettede for dig.
Den bliver dit mest læste dokument og vises automatisk, når man tilgår arkivet
på GitHub. Forklar som minimum læseren:

*   hvad projektet går ud på, hvem projektet er til for, og hvordan det skaber
    værdi (eksistensgrundlag),
*   hvordan man kommer igang med at bruge pakken,
*   hvordan man kan bidrage til projektet,
*   hvordan projektet vedligeholdes,


**Konklusion**

Efter disse første skridt, har vi følgende i rod.mappen af arkievet:

.. code-block:: none

    package
    ├── .git
    ├── .gitignore
    ├── LICENSE
    └── README.md


.. note :: I det følgende, bliver alle ændringer foretaget lokalt, med mindre
   andet er angivet.


Reproducérbar Python-miljø-opsætning
------------------------------------

Pakken, vi bygger, afhænger af valgt Python-version og eventuelle, eksterne
pakker [tredjepartsbiblioteker]. Når pakken virker, som den skal, er det med de
versioner af pakkens afhængigheder (og deres egne afhængigheder), som vi enten
selv valgt specifikt ud eller bare dem, der var nyest, da pakken blev
påbegyndt.

Når vi udvikler pakken bruger vi altså en bestemt udgave af Python og specifikke
versioner af de tredjepartsbiblioteker, som pakken bruger for at opnå sin
funktionalitet. Alt, hvad pakken afhænger af, kan ændre funktionalitet over
tid. Nogen gange gør ændringerne i én afhængighed det vanskeligt eller umuligt
at fungere sammen med de andre afhængigheder eller pakkens egen
funktionalitet.

Med ``mamba`` kan vi installere et isoleret miljø, hvor alle afhængigheder,
inklusive Python-version, holdes fast og er uafhængige af andre tilsvarende
miljø-opsætninger.

De specifikke afhængigheders versioner beskrives i en konfigurationsfil, der
konventionelt hedder ``environment.yml`` for den brugervendte installation af
pakken og ``environment-dev.yml`` for udviklingsmiljøet. Sidstnævnte inkluderer
typisk ekstra værktjer, som kun er relevante for udviklere.

Fordelen er altså, at man for både brugere og udviklere sikrer, at de til hver
revision og version af pakken, kan installere opræcis de afhængigheder, der
skal til for at den pågældende version af pakken virker.

**Udviklingsmiljø**

Begynd med at oprette konfigurationsfilen ``environment-dev.yml`` med
beskrivelsen dine afhængigheder som udvikler.

.. code-block :: yaml

    name: package-dev
    channels:
      - conda-forge
    dependencies:
      - python=3.10
      - pytest

I ovenstående eksempel navngiver vi miljøet efter pakkens navn med suffikset
``-dev`` for at vise, at dette er miljø-opsætning for udviklere af pakken.

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

    Vi undlader at bruge ``mamba`` til at oprette miljø-konfigurationsfilen,
    fordi alle afhængigheder til de pakker, vi eksplicit skrev ovenfor kommer
    med. Samtidig tilføjer kommandoen også en linje ``prefix:`` med konkret
    placering af miljøet på maskinen, hvor nedenstående kommando blev skrevet.

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


**Miljø-opsætning til brugerinstallation**

For brugeren, der kun skal installere pakken og dennes afhængigheder, opretter
man nemt et tilsvarende miljø, men uden de for udvikleren relevante
hjælpe-værktøjer.

Et tilsvarende eksempel svarende til ovenstående opsætning for udvikleren ses
nedenfor for konfigruationsfilen ``environment.yml``:

.. code-block :: yaml

    name: package
    channels:
      - conda-forge
    dependencies:
      - python=3.10

For brugeren bliver den tilsvarende vejledning så

.. code-block :: none

    (base)> mamba env create -f environment.yml

Og miljøet kan herefter aktiveres med:

.. code-block :: none

    (base)> mamba activate package
    (package)>

.. note :: Brug unikke navne til miljøerne

    Som det fremgår ovenfor, så er det primære navn på miljøet det samme som
    pakken (med ``-dev``-suffiks for udviklingsmiljøet).

    ``conda``/``mamba`` lægger i udgangspunktet alle miljøerne på samme
    placering i dét miljø, de installeres i. Derfor skal miljø-navnene
    nødvendigvis være unikke.


**Konklusion**

Vi har nu en miljø-opsætning til os selv og andre udviklere, som definerer de
fælles værktøjer, der er relevante under udviklingen af pakken.

Vi har også en tilsvarende opsætning for brugeren, som skal installere pakken.

Disse to filer definerer de afhængigheder, vi starter pakken med. Tilføj disse
filer til versonsstyringen, og de kan nu deles og ændres på tværs af revisioner
og pakkens versioner.


Python-konfigurationsfiler
--------------------------

En ren Python-pakke bliver idag defineret med følgende opsætning:


.. code-block:: none

    package
    ├── pyproject.toml
    ├── setup.cfg
    └── src
        └── package
            ├── module1.py
            ├── module2.py
            └── __init__.py

Det er normal konvention at kalde rodmappen det samme som pakken-mappen inde i
``src``-mappen. ``src``-opsætningen er efterhånden ved at blive alment kendt,
og strukturen er blandt andet valgt, fordi det tvinger én til at installere
pakken lokalt, når man skal teste koden.

Python-fortolkeren betragter en mappe med python-moduler som en pakke. Hvis
pakke-mappen ``package`` lå direkte i roden af projekt-mappen, kan
test-funktionalitet, der kører fra samme mappe ikke importere en installeret
version af pakken, fordi Python-fortolkeren starter med at lede efter
importerede moduler i samme mappe, som test-programmet kører i.

De to filer i projekt-mappen:

*   ``pyproject.toml``
*   ``setup.cfg``

udgør vores pakke-opsætning. ``pyproject.toml`` [`læs mere om TOML
<https://toml.io/>`] fortæller, at vi har med et Python-projekt at gøre, mens
``setup.cfg`` indeholder konfiguration til standard-pakke-værktøjet
`SetupTools`_. Med de nyere versioner af SetupTools er man gået væk fra at
bruge en ``setup.py``-fil til kun at bruge en konfigurationsfil. ``setup.py``
kan stadig bruges, og det er stadig meget normalt at se denne forældede praksis
i eksisterende Python-pakker.

``setup.cfg`` indeholder alle informationer om pakken, mens ``pyproject.toml``
som minimum skal indeholde konfiguration af pakke-værktøj, som altså her er
SetupTools. Der findes idag alternative pakke-væktøjer med forskellig
popularitet, som lægger al deres konfiguration ind i ``pyproject.toml``. Vi
anbefaler dog, at vi bruger SetupTools, som er mere bredt anvendt.

Følgende er en minimal opsætning for ``pyproject.toml`` samt et eksempel på
pakke-metadata i ``setup.cfg``.

.. code-block :: toml

    # pyproject.toml
    [build-system]
    requires = [
        'setuptools>=43.0.0'
    ]
    build-backend = 'setuptools.build_meta'

.. code-block :: ini

    ; setup.cfg
    [metadata]
    name = package
    version = 0.1.0
    description = Best Practise Package
    long_description = file: README.md
    long_description_content_type = text/markdown; charset=UTF-8
    url = https://github.com/...
    author = Firstname Lastname
    author_email = firstname.lastname@sdfe.dk
    license = MIT
    license_file = LICENSE
    project_urls =
        Documentation = https://Kortforsyningen.github.io/...
        Source = https://github.com/Kortforsyningen/...
        Tracker = https://github.com/.../issues

    [options]
    zip_safe = False
    package_dir =
        = src
    packages = find:
    platforms = any
    python_requires = >=3.10

Med ovenstående opsætning kan Pythons pakke-styringsværkøj ``pip`` selv finde ud
af at installere pakken ``setuptools``, som bygger pakken med de givne
metadata, som ``pip`` så installerer.

For at installere pakken, så den er tilgængelig for Python-fortolkeren, vi
bruger i conda-miljøet ``package-dev``, bruger vi ``pip`` som et modul i det
aktiverede miljø, så vi er sikre på, at vi ikke bruger en anden
``pip``-kommando, der kan være tilgængelig i terminalen:

.. code-block :: none

    (package-dev)> python -m pip install -e .

Læs mere om de enkelte konfigurationsmuligheder i dokumentationen for
`SetupTools`_.

.. _`SetupTools`: https://setuptools.pypa.io/


Test-funktionalitet
-------------------

Denne vejledning har et separat kapitel om implementation af test-funktionalitet
og anden kvalitetssikring i Python. Her nævner vi kort, at al
test-funktionalitet bør ligge separat i sin egen mappe kaldet ``tests/``.

De relevante konfigurations-filer og mapper med test-funktionaliteten ser
således ud:

.. code-block:: none

    package
    ├── environment-dev.yml
    ├── pyproject.toml
    ├── setup.cfg
    └── tests
        └── package
            ├── test_module1.py
            └── test_module2.py

Bemærk, at test-koden følger samme struktur som pakkens undermapper.


Dokumentation
-------------

*   Dokumentationen bør være versionsstyret og i hver revision passe til koden,
    den følger.
*   På denne måde kan man altid gå tilbage til en tidligere version af koden og
    se, hvordan den pågældende version skulle bruges.

De relevante konfigurations-filer og mapper med dokumentationsmateriale og
opsætning i vores eksempel ser således ud:

.. code-block:: none

    package
    ├── docs
    │   ├── index.md
    │   └── ...
    ├── environment-dev.yml
    ├── LICENSE
    ├── mkdocs.yml
    ├── pyproject.toml
    ├── README.md
    ├── setup.cfg
    └── src
        └── package
            ├── module1.py
            ├── module2.py
            └── __init__.py

*   Mappen ``docs/`` indeholder en komplet beskrivelse af pakkens indhold til
    alle relevante modtagere, eksempelvis udviklere, brugere, driftsansvarlige
    og andre interessenter. Indholdet består af kildemateriale, primært i form
    af tekst og billeder. Tekst-dokumenterne indeholder typisk direktiver, der
    af en dokumentations-bygger, så som `Sphinx`_ eller `MkDocs`_, oversættes
    til eksempelvis faktabokse, advarsler, tips og tricks, eller deciderede
    kommandoer, hvis resultater kommer med i det endelige
    dokumentationsmateriale, der skal udgives.

*   ``mkdocs.yml`` er et eksempel på en konfigurationsfil for et
    dokumentationsværktøj. I dette eksempel illustrerer vi det med `MkDocs`_,
    der er forholdsvis hurtigt at sætte op og bruger `Markdown`_
    [fil-endelse: ``.md``] som kildeformat. For en robust og markant mere
    alsidig løsning, anbefaler vi Sphinx-dokumentationsværktøjet, der bruger
    reStructuredText [fil-endelse: ``.rst``] som kildeformat.

*   ``LICENSE`` er dokumentation af pakkens rette, juridiske brug.

*   ``README``-filen, her i Markdown-format, er dén fil, man br læse først, når
    man tilgår projektet. På GitHub er den fremhævet som hoveddokumentationen i
    arkivets rod [1]_. Derfor bør den indeholde de vigtigste oplysninger, der
    gør læseren istand til at forstå, hvad projektet går ud på, og hvordan man
    bruger det og bidrager til at forbedre det.

*   Konfigurationsfilerne ``environment-dev.yml``, ``pyproject.toml`` og
    ``setup.cfg`` er med, fordi de er nødvendige for at bygge dokumentationen.

*   Pakkens kildekode i ``src/`` er med, fordi kildekodens dokumentation i form
    af `Python docstrings`_ kan bruges af dokumentationsværktøjet til
    automatisk at få produceret dokumentation af pakkens moduler og
    applikationsprogrammeringsflade [*en.* application-programming interface,
    API].

.. _`Sphinx`: https://www.sphinx-doc.org/
.. _`MkDocs`: https://www.mkdocs.org/
.. _`Markdown`: https://daringfireball.net/projects/markdown/
.. _`Python docstrings`: https://peps.python.org/pep-0257/

.. rubric:: Fodnoter

.. [1] Man får samme effekt i undermapper, der inkluderer en README-fil, men
   hold dig til én README i projekt-mappens rod.
