.. _pythonpakker:

Gode Pythonpakker
=================

I dette kapitel beskriver vi en række gode og anbefalelsesværdige Pythonpakker, der kan hjælpe med at
løse gængse problemstillinger en udvikler i SDFE ofte vil støde på. I udvælgelsen af pakkerne
herunder er der lagt vægt på særligt pythonske implementeringer, der benytter sig af koncepter
introduceret i nyere udgaver af Python.

Håndtering af geodata
----------------------------

`GDAL`_ er det mest udbredte programmel til læsning, skrivning og manipulation
af geodata. GDAL kan bearbejde både raster- og vektordata. Vektordata manipuleres med
underbiblioteket OGR. GDAL-projektet tilbyder pythonpakke men den er ikke særligt intuitiv at
bruge for pythonudviklere, da der er tale om en en-til-en mapning af det underliggende C API.
Bedre alternativer er pakkerne Rasterio (rasterdata) og Fiona (vektordata), der begge tilbyder
et bedre og mere pythonsk API til GDAL og OGR.


Rasterio
+++++++++

`Rasterio dokumentation`_.

Rasterio bruges til at læse og skrive rasterdata. Herunder eksempler på begge dele.

Læsning af rasterdata er utroligt simpelt og minder meget om måden almindelige tekstfiler
læses med Python:

.. code-block:: python

   import rasterio

    with rasterio.open("eksempel.tif", "r") as grid:
        # Læs information om hele filen
        epsg_kode = grid.crs
        gridstørrelse = grid.shape

        # Behandl data for hvert enkelt bånd i rasterfilen.
        # Bemærk at data returnes i form af numpy arrays.
        for båndnr in grid.indexes:
            data = grid.read(båndnr)
            gennemsnit = data.mean()

Oprettelse af en ny fil og eftefølgende skrivning af nyt data er mere omfattende. Det forudsættes
at der er taget stilling til filens dækningsområde, koordinatsystem, antal bånd, gridstørrelse,
filformat og datatype. Alt dette kan dog klares relativt simpelt, som demonstreret i eksemplet
herunder, hvor vi laver en tiff-fil med tre bånd, der omtrentligt dækker det danske område og
registrerer data i ETRS89-koordinater.

.. code-block:: python

    import rasterio
    import numpy as np

    # Grid dimensioner
    antal_bånd = 3
    højde = 256
    bredde = 256

    # Dækningsområde
    vest = 8.0
    øst = 13.0
    syd = 54.0
    nord = 58.0

    # Opsætning af rasterfil
    kwargs = dict(
        driver="GTiff",
        height=højde,
        width=bredde,
        count=antal_bånd,
        crs="EPSG:4258", # ETRS89, geografiske koordinater
        dtype=np.float64,
        transform=rasterio.transform.from_bounds(vest, syd, øst, nord, bredde, højde)
    )

    with rasterio.open("ny_fil.tif", "w", **kwargs) as grid:
        for båndnr in range(1, antal_bånd+1): # bånd er 1-indekserede
            data = np.random.rand(højde, bredde)*100
            grid.write(data, båndnr)

Læs mere om Rasterio i
`projektets dokumentation <https://rasterio.readthedocs.io/en/latest/index.htm>`_.

.. tip::

    Ved installation af Rasterio følger kommandolinjeværktøjet ``rio`` og med. ``rio`` er et
    mere sammenhængende alternativ til GDALs kommandolinjeapplikationer. Læs mere
    `her <https://rasterio.readthedocs.io/en/latest/cli.html#command-line-user-guide>`__.

.. _`GDAL`: https://gdal.org/
.. _`Rasterio dokumentation`: https://rasterio.readthedocs.io/en/latest/index.html


Fiona
++++++

`Fiona dokumentation`_.

Fiona er en moderne pendant til OGR på samme måde som Rasterio er det for GDAL. Fiona tilbyder
en mere pythonsk og intuitiv måde at skrive og læse vektordata på.

.. note::

    På engelsk udtales OGR udtales oftest "Ogre" (*dansk* trold) . Fans af Shrek-serien vil
    genkende at Fiona er den pæne side af trolden. I Python er Fiona den pæne udgave af OGR.

At læse vektordata med Fiona er lige som at gennemløbe en liste af Python-dicts. Hver
feature udgøres af en dict der indeholder information om geometri og attributter. Her et eksempel
på en punktfeature fra stednavneregisteret:

.. code-block:: python

    {'geometry': {'coordinates': (595507.300561634, 6402053.2234713, -999.0),
                  'type': 'Point'},
     'id': '9480',
     'properties': dict([('FEAT_ID', 261996),
                                ('FEAT_KODE', 5010),
                                ('FEAT_TYPE', 'strandpost'),
                                ('AREAL', None),
                                ('SNSOR_ID', 1006651),
                                ('NAVN', 'E320'),
                                ('STAMNAVN', None),
                                ('STAMOPL', 1),
                                ('GODKENDT', '1'),
                                ('INDB_ANTAL', None),
                                ('LAENGDE', None),
                                ('DQ_INDEX', 0),
                                ('TIMEOF_CRE', '2015-07-01'),
                                ('TIMEOF_PUB', '2015-07-01'),
                                ('TIMEOF_REV', None),
                                ('TIMEOF_EXP', '2018-03-31')]),
     'type': 'Feature'}


Herunder et eksempel på læsning af punkter fra en shapefil. I eksemplet finder vi det nordligste
punkt i datasættet.

.. code-block:: python

    import fiona

    with fiona.open("eksempel.shp", "r") as punkter:
        northing_maks = -1
        nordligste_punkt = None
        for punkt in punkter:
            easting, northing, _ = punkt["geometry"]["coordinates"]
            if northing > northing_maks:
                northing_maks = northing
                nordligste_punkt = punkt


Skrivning af data til en ny fil er selvsagt mere omfattende end at læse data fra
en eksisterende fil. I eksemplet herunder oprettes en ny shapefil med afsæt i et schema, der
definerer geometritype og attributter for datasættet. Efterfølgende åbnes en ny fil og denne
populeres med data.

.. code-block:: python


    import fiona
    import fiona.crs

    SCHEMA = {
        "geometry": "Point",
        "properties": dict(
            [
                ("navn", "str"),
                ("kote", "float"),
            ]
        ),
    }

    def ny_feature(navn, kote, easting, northing):
        """ Returner ny feature der passer til SCHEMA"""
        return {
            "geometry": {"type": "Point", "coordinates": (easting, northing)},
            "properties": dict(
                [
                    ("navn", navn),
                    ("kote", kote),
                ]
            ),
        }


    gnss_stationer = [
        ny_feature("SKEJ", 72.028, 573225.054, 6227584.642),
        ny_feature("BUDP", 57.890, 719707.285, 6182582.500),
    ]

    with fiona.open(
        "ny_fil.shp",
        "w",
        driver="ESRI Shapefile",
        crs=fiona.crs.from_epsg(25832),  # ETRS89 / UTM 32N
        schema=SCHEMA,
    ) as stationer:
        for station in gnss_stationer:
            stationer.write(station)

Tilføjelse af data til et eksisterende datasæt er simplere. Det samme er skrivning af en ny fil
med samme opbygning som en eksisterende. Eksempler på disse tilfælde findes i
`dokumentationen <https://fiona.readthedocs.io/en/latest/manual.html#writing-vector-data>`_

.. tip::

    Ved installation af Fiona følger kommandolinjeværktøjet ``fio`` og med. ``fio`` er et
    mere sammenhængende alternativ til OGRs kommandolinjeapplikationer. Læs mere
    `her <https://fiona.readthedocs.io/en/latest/cli.html>`__.

.. _`Fiona dokumentation`: https://fiona.readthedocs.io/en/latest/manual.html
