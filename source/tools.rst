.. _værktøjer:

===================
Udviklingsværktøjer
===================

Ud over kode-gennemsyn af kollegaer, så er der til ethvert projekt et behov for en standard for, hvordan koden bør bygges op.

En række værktøjer kan være fordelagtige at bruge i forbindelse med Pythonudvikling. Følgende er en række anbefalinger, der kan øge kvaliteten af den udviklede kode.

*   Værktøj til at læse og redigere kode. Også kaldet ent integreret udviklingsmiljø .
*   Værktøj til at rette koden ind, mens mans skriver den. På engelsk/amerikansk bruges begrebet *linter*, som er en maskine til at fjerne bomuldhår, -frø og fnug/fnuller fra bomuld under opspinningsprocessen.
*   Værktøj til (automatisk) kodeformatering, så koden skrives på samme måde gennem hele kodebasen.
*   Værktøj til at køre kodebasens test-kode [test-rammeværk].
*   Værktøj til at analysere kodens kvalitet, eksempelvis ved statisk kodeanalyse eller vedligeholdeskompleksiteten over tid.


Integreret udviklingsmiljø
==========================

Det integrerede udviklingsmiljø [*en* integrated development environment, IDE] er typisk et tekstredigeringsværktøj, der giver en udvikler mulighed for at redigere kildekode og helt eller delvist automatisk udføre rutine-opgaver så som at bygge, teste, debug'e, analysere og dokumentere kode med værktøjer (ofte plugins), som er integrerede i tekstredigeringsværktøjet.

**Microsoft Visual Studio Code [VSCode]**

IDE'er som VSCode har indbygget mulighed for at tilføje udvidelser [extensions i VSCode], der er specielt udviklet til IDE'en. Nogle udvidelser kan dog afhænge af, at du selv installerer nogle pakker ved siden af.

Det er ikke alle udvidklingsværktøjer, der er knyttet til en given IDE. Der findes indbyggede og tredjeparts-biblioteker, som både kan integreres med en IDE, men også kan køre selvstændigt.

I denne vejlednings eksempler udgør udviklingsmiljøet et ``conda``-miljø (installeret med ``mamba``-værktøjet) defineret med en konfigurationsfil ``environment-dev.yml``, som indeholder en liste med (Python-)pakker, der skal bruges af udviklere på det givne projekt.

Disse værktøjer kan typisk bruges alene, men fordi IDE'er som VSCode automatisk kan se, at man arbejder i et ``conda``-miljø, kan brugergrænsefladen i IDE'en forholdsvis nemt sættes op, så man nemmere kan bruge værktøjerne.


Løbende kodetilretning [*en.* linting]
======================================

*...*


Automatisk kodeformatering
--------------------------

Python core developer Jack Diederich udtrykker med sætningen `*All effort feels like useful effort* https://youtu.be/rrBJVMyD-Gs?t=178` sin erfaring med blandt andet kvalitetssikring af kode med *code review* og *linting*, hvor man bruger tid på at tale om forbedringer af kode-opsætningen. Det føles nyttigt, fordi man har brugt tid på det. Det er bare ikke dét, der giver god kode.

`black`_ er et værktøj, der automatisk reformaterer kode. Formålet med værktøjet er, at lade værktøjet styre hvordan koden fremstår, så man kan rbuge sin tid på vigtigere aspekter af koden.

Ikke alle er enige med måden ``black`` formatterer koden på, men det er énsartet, læsbart, og man behøver ikke selv at skrive kode på samme måde. ``black`` retter bare koden til.

Black kan tilpasses i ``pyproject.toml``.


.. _`black` : https://github.com/psf/black



Test-rammeværk
--------------

Python kommer med indbygget test-funktionalitet i modulet ``unittest``, som fint kan bruges. Det er dog langt hurtigere og nemmere at komme igang med det relativt udbredte værktøj ``pytest``.


``pytest`` er en Python-pakke, som typisk bruges både som modul og kommandolinjeværktøj.

Med vores standard-opsætning,


Kodeanalyse
-----------

For Python er der mulighed for at bruge type-annoteringer i koden, hvis korrekthed kan checkes af et ekstra `mypy`






Grundlæggende begreber, mekanismer og værktøjer
================================================


Relevante processer for et Python-projekt
-----------------------------------------

Antagelser: Ikke nødvendigt at nævne, hvad Python er. Brug korte, konkrete beskrivelser af processerne, og hvad de enkeltvis kræver af 'økosystemet'

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





