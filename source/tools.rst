.. _værktøjer:

===================
Udviklingsværktøjer
===================

En række værktøjer kan være
fordelagtige at bruge i forbindelse med Pythonudvikling. Følgende er en række
anbefalinger, der kan øge kvaliteten af den udviklede kode. De kan hjælpe med
følgende:

*   at læse og redigere kode. Også kaldet et integreret udviklingsmiljø.
*   at rette koden ind, mens mans skriver den [*en.* linting] [1]_
*   (automatisk) kodeformatering, så koden skrives på samme måde gennem hele
    kodebasen.
*   at køre kodebasens test-kode [test-rammeværk].
*   at analysere kodens kvalitet, eksempelvis ved statisk kodeanalyse eller
    vedligeholdeskompleksiteten over tid.

Vi gennemgår nogle af ovenstående, men tilføjer flere over tid.


Integreret udviklingsmiljø
==========================

Det integrerede udviklingsmiljø [*en* integrated development environment, IDE]
er typisk et tekstredigeringsværktøj, der giver en udvikler mulighed for at
redigere kildekode og helt eller delvist automatisk udføre rutine-opgaver så
som at bygge, teste, debug'e, analysere og dokumentere kode med værktøjer
(ofte plugins), som er integrerede i tekstredigeringsværktøjet.

**Microsoft Visual Studio Code [VSCode]**

IDE'er som VSCode har indbygget mulighed for at tilføje udvidelser [extensions i
VSCode], der er specielt udviklet til IDE'en. Nogle udvidelser kan dog afhænge
af, at du selv installerer nogle pakker ved siden af. En oplagt udvidelse at
installere fra `Market Place`_ er `Python`_.

.. _`Market Place`: https://marketplace.visualstudio.com/
.. _`Python`: https://marketplace.visualstudio.com/items?itemName=ms-python.python

Det er ikke alle udviklingsværktøjer, der er knyttet til en given IDE. Der
findes indbyggede og tredjeparts-biblioteker, som både kan integreres med en
IDE, men også kan køre selvstændigt.

I denne vejlednings eksempler udgør udviklingsmiljøet et ``conda``-miljø
(installeret med ``mamba``-værktøjet) defineret med en konfigurationsfil
``environment-dev.yml``, som indeholder en liste med (Python-)pakker, der skal
bruges af udviklere på det givne projekt.

Disse værktøjer kan typisk bruges alene, men fordi IDE'er som VSCode automatisk
kan se, at man arbejder i et ``conda``-miljø, kan brugergrænsefladen i IDE'en
forholdsvis nemt sættes op, så man nemmere kan bruge værktøjerne.

.. figure:: images/tools/vscode_discovering-python-interpreters.*
    :align: center

    *Nederst til venstre i VSCode, kan man i et Python-projekt se, at IDE'en
    automatisk leder efter alle de Python-fortolkere, programmet kan finde.*

Programmet fanger også fortolkerne i de enkelte ``mamba``-miljøer, og det er
efterfølgende nemt at skifte til dén fortolker, man ønsker at bruge i sin
udvikling ved enten at bruge VSCode's `Command Palette`_
[``Ctrl`` + ``Shift`` + ``p``] vælger "Python: Select Interpreter".

.. figure:: images/tools/vscode_button-to-open-python-interpreter-selction-menu.*
    :align: center

    *Nederst til højre i VSCode kan man trykke på knappen "Python", for at åbne
    listen med mulige Python-fortolkere.*

.. _`Command Palette`: https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette
.. _`VSCode brugergrænseflade`: https://code.visualstudio.com/docs/getstarted/userinterface

*Terminalen*

Tilgængelige terminaler såsom POwer Shell og Command Prompt er integreret i VSCode og kan åbnes som et separat panel i brugergrænsefladen. Arbejder man fra terminalen og vil gerne åbne et dokument med en kommando i VSCode, skal terminalen have installeret kommandolinjeværktøjet ``code``. Kommandoen kan installeres ved fra VSCode's Command Palette at vælge "install 'code' command in path".


Automatisk kodeformatering
==========================

Python core developer Jack Diederich udtrykker med sætningen *All effort feels
like useful effort* [`link <https://youtu.be/rrBJVMyD-Gs?t=178>`_] sin erfaring
med blandt andet kvalitetssikring af kode med *code review* og *linting*, hvor
man bruger tid på at tale om forbedringer af kode-opsætningen. Hans pointe er,
at éns indsats føles nyttig, fordi man har brugt tid på den. Det er bare ikke
besværet i sig selv, der giver god kode (eller noget andet for dén sags
skyld).

En problemstilling, der ofte fylder en del hos udviklere, er valget af kodestil,
hvilket tager ressourcer fra udvikling af konkret, værdiskabende
funktionalitet.

Løsningen er en fællesskabsfremmende fjende: `Black`_, som er et værktøj, der
automatisk reformaterer kode til en énsartet syntaks, som er nem at læse og
minimerer størrelsen på diffs. Formålet med værktøjet er, at lade værktøjet
styre hvordan koden fremstår, så man kan bruge sin tid på vigtigere aspekter af
koden.

.. _`Black`: https://github.com/psf/black

Det smarte ved værktøjet er, at man ikke behøver at bekymre sig om, hvordan man
selv skriver koden, da ``black`` retter al kode til. Derfor kan man bruge tiden
på at tænke over indholdet og mindre over, hvordan koden skal se ud.

*Tilpasninger*

Black kan tilpasses i ``pyproject.toml``, hvis man eksempelvis ønsker at undlade
at omformatere visse filer.

.. important::

    I nogle tilfælde kan black forringe læsbarheden, som det kan ses af følgende
    eksempel, hvor en matrix er formateret for netop at øge læsbarheden:

    .. code-block:: python

        matrix = np.array([
            [-sin(φ)*cos(λ), -sin(λ), cos(φ)*cos(λ)],
            [-sin(φ)*sin(λ),  cos(λ), cos(φ)*sin(λ)],
            [        cos(φ),       0,        sin(φ)],
        ])

    Sender man ovenstående igennem ``black`` får man følgende:

    .. code-block:: python

        matrix = np.array(
            [
                [-sin(φ) * cos(λ), -sin(λ), cos(φ) * cos(λ)],
                [-sin(φ) * sin(λ), cos(λ), cos(φ) * sin(λ)],
                [cos(φ), 0, sin(φ)],
            ]
        )

    For at markere i koden, hvor man ønsker, at holde fast i sin egen
    formatering, kan man skrive ovenstående, som får ``black`` til at ignorere
    denne del af koden.

    .. code-block:: python
        :emphasize-lines: 1, 7

        # fmt: off
        matrix = np.array([
            [-sin(φ)*cos(λ), -sin(λ), cos(φ)*cos(λ)],
            [-sin(φ)*sin(λ),  cos(λ), cos(φ)*sin(λ)],
            [        cos(φ),       0,        sin(φ)],
        ])
        # fmt: on

    **Husk dog, at der skal være god grund til at omgå Blacks standard-indstillinger.**


Test-rammeværk
==============

Python kommer med indbygget test-funktionalitet i modulet ``unittest``, som fint
kan bruges. Det er dog langt hurtigere og nemmere at komme igang med det
relativt udbredte værktøj ``pytest``.

``pytest`` er en Python-pakke, som typisk bruges både som modul og
kommandolinjeværktøj.

Se :ref:`kapitlet om test-opsætning <tests>` for flere detaljer om brugen
af pytest.


.. rubric:: Fodnoter

.. [1] På engelsk/amerikansk bruges begrebet *linter*, som er en maskine til at
   fjerne bomuldhår, -frø og fnug/fnuller fra bomuld under opspinningsprocessen
   under tekstilproduktionen.
