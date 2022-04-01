.. _tests:

=============
Test din kode
=============

Testfunktionalitet er en væsentlig del af kvalitetssikringen af koden.
Test-funktionaliteten bidrager primært med to ting:

*   Udvikleren bliver udfordret på sit API-design.
*   Kodens funktionalitet kan automatisk afprøves mange gange, og afviklingen
    kan bruges som en begrænsende faktor [*en.* gate] for, om udvikleren kan
    skubbe sine bidrag til det centrale arkiv på GitHub. På GitHub
    (eller generelt: den centrale udviklingsserver) bør éns
    continuous-integration pipeline sørge for at teste alle bidrag, inden de
    kan få lov til at blive lagt sammen med koden på hovedsporet.


Perspektiver
------------

*   Det er nævnt af andre, at man først bør refaktorisere sin kode, når der en
    ekstra funktion i koden, der skal genbruge dét, man har skrevet i en samlet
    funktion/metode. Andre igen har nævnt, at det er en sund praksis at
    betragte sine enhedstests som dén ekstra funktion, der giver anledning til
    refaktoriseringen.


``pytest``
==========

Vi anbefaler `Pytest`_ som test-rammeværk.


Opsætning
---------

Opsæt ``pytest`` i din pakkes ``pyproject.toml`` eller, for simple scripts, i en
``pytest.ini``-fil.

Eksempel på opsætning i ``pyproject.toml``

.. code-block:: toml

    [tool.pytest.ini_options]
    addopts = '--strict-markers -r A -q'
    testpaths = ['tests']

Ovenstående eksempel viser en opsætning for en standard pakke-opsætning med en
pakke-struktur som vist under kapitlet pakke-opbygning.

Læs mere på pytests egen dokumentation for opsætningsmuligheder med disse
formater.


Grundlæggende eksempler med ``pytest``
--------------------------------------

Nedenstående er et eksempel på, hvordan man bygger test-funktionalitet op med
pytest.

Koden illustrerer også, hvordan man kan bruge Arrange, Act og Assert-paradigmet
i forhold til opsætning af test-koden.

Test-funktioner skal være stjerneklare, så man meget nemt kan se, hvad de
tester (her hjælper lange funktionsnavne).

Læseren af testen lærer samtidig, hvordan éns API skal bruges, jævnfør
ovenstående idé med at lade test-funktioner være på lige fod med anden
funktionalitet i koden, de tester.

.. code-block:: python

    import math

    from brahmath.geometry import (
        semi_perimeter,
     )


    def test_semi_perimeter():
        # Single setup for clarity
        # Arrange
        sides = [1, 1, 1, 1]

        # Act
        result = semi_perimeter(*sides)
        expected = 2

        # Assert
        assert math.isclose(result, expected), \
            default_assertionerror_message(result, expected)

        # Multiple scenarios
        test_data = (
            ([1, 2, 3, 4], 5),
        )
        for (sides, expected) in test_data:
            result = semi_perimeter(*sides)
            assert math.isclose(result, expected), \
                default_assertionerror_message(result, expected)

I eksemplet ovenfor er vist, hvordan pytest udnytter Pythons indbyggede
``assert``-statement som primære tilgang til at udtrykke, hvad man forventer.

Alle Python-moduler og -funktioner på konfigurations-stien til
test-funktionaliteten bliver af pytest betragtet som test-funktionalitet. Der
er altså ikke behov for, at man som udgangspunkt importerer test-rammeværkets
moduler.

Men mere avancerede muligheder eksisterer dog, hvilket kan ses på
`Pytest`_-dokumentationen.

.. _`Pytest`: https://docs.pytest.org/
