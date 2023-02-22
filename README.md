# Sieve of Erastothenes

In deze laatste opdracht gaan we de krachten van OpenMP en MPI combineren. Dit is uiteraard handig als je een multi-processor multi-node supercomputer of computercluster tot je beschikking hebt.

Op Canvas is een simpel voorbeeld van code waarin MPI en OpenMP gecombineerd worden: `mpi-openmp.cc`

- Compilen en runnen met mpic++ (voeg `-fopenmp` (op macOS `-Xclang -fopenmp -lomp`) aan het compile command toe)
   - Op macOS wordt dat dan dus: `mpic++ -Xclang -fopenmp -lomp -Wall -ansi -pedantic -std=c++17 mpi-openmp.cpp -o mpi-openmp`
   - Op Windows/Linux: `mpic++ -fopenmp -Wall -ansi -pedantic -std=c++17 mpi-openmp.cpp -o mpi-openmp`
- `mpirun`/`mpiexec` om uit te voeren

In deze opgave gaan een conceptueel eenvoudig probleem oplossen; maar dit probleem is computationeel lastig, en erg belangrijk voor cryptografie en security. Het gaat om het vinden van alle priemgetallen tot een bepaalde (grote) limietwaarde. Om dit probleem op te lossen gaan we een "hybride" parallelle versie maken van een klassiek algoritme, genaamd [Eratosthenes Zeef](https://nl.wikipedia.org/wiki/Zeef_van_Eratosthenes).

Om alle priemgetallen te vinden onder een bepaalde limietwaarde N, werkt de sequentiële variant van dit algoritme alsvolgt (dit is een voorbeeld, je mag ook je eigen versie bedenken):

- Creeër de zeef -- een dynamische array van $2$ tot $N$ (dit kan een array van booleans zijn, of ints of wat dan ook waar je random access toe kan hebben en eenvoudig gebruikt kan worden om bepaalde elementen te "markeren").
- Aangezien $1$ geen priem is, zetten we $k$ op $2$.
- Loop totdat $k^2 > N$:
       - In de zeef, markeer alle elementen op de indexen die een veelvoud zijn van $k$ tussen $k^2$ en $N$ (inclusief).
       - Vind de kleinste index groter dan $k$ die niet gemarkeerd is, en zet k op deze waarde.
- De indexen van de niet gemarkeerde elementen in de zeef zijn priemgetallen.

In deze opgave gaan we een hybride variant maken, waarbij we MPI gebruiken om de "zeef" te verspreiden over verschillende nodes en OpenMP gaan gebruiken om de processen op de nodes te versnellen.

Om je oplossing te checken: onder de 100 zijn er 25 priemgetallen, onder de 1.000 zijn het er 168.

## Deel 1

Implementeer de sequentiële variant van Erathosthenes Zeef, zoals hierboven beschreven. Verifieer dat je implementatie correct is (bijv. door voor een kleine limit te checken of alle priemgetallen worden gevonden).

Draai vervolgens je implementatie met een hoge limietwaarde (bijv. 1.000.000.000 of meer), waarbij je gebruik maakt van maar 1 process. Meet de tijd die het algoritme nodig heeft om deze uit te rekenen en schrijf deze op in een spreadsheet.

## Deel 2

Maak een conceptuel ontwerp van een parallelle variant waarin je MPI gebruikt om meerdere processen aan te sturen, en OpenMP om de processes multithreaded te maken. Teken je ontwerp uit, of beschrijf het aan de hand van een voorbeeld.

Implementeer je ontwerp door middel van gebruik te maken van de bovengenoemde modules, en test je oplossing met verschillende combinaties van meerdere threads en meerdere nodes (eventueel over meerdere computers). Noteer de tijden die je meet voor elke variant in je spreadsheet.

Maak een duidelijke analyse van de performance winsten (speed-up en efficiency) van je implementatie, op basis van de door jou verzamelde getallen. Maak duidelijke plots om je verhaal kracht bij te zetten.

Voeg tenslotte een (max) halve A4 aan reflectie toe over je inlevering. In welke zin vind je dat je geslaagd ben in het maken van deze opgave; wat kon er beter, wat vond je eenvoudig/lastiger...

## Inleveren HPP Opdrachten
Voor de opdrachten van High Performance Programming lever je een verslag in, in PDF formaat.

Begin het verslag met:

- De titel van de opdracht;
- Je naam en studentnummer;
- Een link naar een private Github/Gitlab repository met je werk.

- Lees de hele opdracht goed door, stel alvast vragen als iets niet duidelijk is
- Voor ieder deel / vraag:
      -  Vermeld het nummer van het deel of de vraag;
      -  Maak de opdracht en/of beantwoord de vraag;
      -  Kies code snippets en/of screenshots om je oplossing te laten zien;
      -  Beschrijf je oplossing beknopt, waarbij je vooral duidelijk maakt hoe je het hebt aangepakt.

Bewaar / exporteer je verslag als PDF en lever die in.
