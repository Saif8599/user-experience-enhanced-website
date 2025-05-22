De instructie vind je in: [INSTRUCTIONS.md](https://github.com/fdnd-task/enhanced-website/blob/main/docs/INSTRUCTIONS.md)

# Enhanced website - Bieb in bloei
Ontwerp en maak een interactieve website die snel laadt en prettig te gebruiken is.

![image](https://github.com/user-attachments/assets/9f180f86-6b71-43ac-abed-513c24f161aa)

![image](https://github.com/user-attachments/assets/a44ac908-9434-473a-af0d-dab3c1d63b37)

## Inhoudsopgave

  * [Beschrijving](#beschrijving)
  * [Kenmerken](#kenmerken)
  * [Functionaliteiten](#functionaliteiten)
  * [Installatie](#installatie)
  * [Bronnen](#bronnen)
  * [Licentie](#licentie)

## Beschrijving
### Over Bieb in bloei
Bieb in Bloei is een digitaal platform met daarop een overzicht van de duurzame projecten van de Buurtcampus Oost.

De Buurtcampus is een dynamische, laagdrempelige plek waar iedereen zich welkom en uitgenodigd voelt om kennis op te doen, te ontwikkelen en te delen. Met als doel: samen de buurt duurzamer, gezonder en inclusiever maken.
Bij de buurtcampus draaien verschillende duurzame projecten; De Stekjes bieb, De Zadenbieb en de Geveltuin.. en er komen nog meer duurzame projecten bij in de toekomst. Op het platform Bieb in Bloei krijgen deze projecten een gezicht.

## Kenmerken
In dit project heb ik gebruik gemaakt van Node.js en Express om een webserver te maken. Voor het genereren van dynamische HTML-pagina's is er gebruik gemaakt van Liquid, dit maakt de webpagina's flexibel en makkelijk te onderhouden maakt.



### Ontwerpkeuzes
Tijdens deze sprint was mijn focus op het redesignen van de stekjes pagina. Bieb in bloei staat voor groen, dit kan je ook terug zien bij de stekjes.


- **Kleur**: Groen als hoofdkleur om een duurzaam leefruimte te creëren.
- **Typografie**: Het Poppins lettertype is zowel leesbaar in kleine lettertypen als in grotere en het geeft moderne en vriendelijke uitstraling. 
- **UI**: Er is gebruik gemaakt van hover states, dit om het gebruiksvriendelijk te maken.
  
![image](https://github.com/user-attachments/assets/a54eb01f-a182-4443-bec2-1ecb997273f7)

![image](https://github.com/user-attachments/assets/d5e8e008-0e6a-4fbe-abf3-a313dce57e44)
![image](https://github.com/user-attachments/assets/a15ed21c-a0a7-4de5-8c20-0c749b1a56da)


### Routes & data
- Homepagina ["/"](https://github.com/Saif8599/user-experience-enhanced-website/blob/main/server.js#L32-L45) : Er een GET route voor de index zonder gegevens gemaakt en rendert index.liquid.
- Stekjes pagina ["/stekjes"](https://github.com/Saif8599/user-experience-enhanced-website/blob/main/server.js#L47-L62) : Haalt gegevens op van de beschikbare stekjes via de Directus API en toont deze in stekjes.liquid
- Stekje detail ["/stekje/[ID]"](https://github.com/Saif8599/user-experience-enhanced-website/blob/main/server.js#L64-L136) : Hier word eerst de gegevens van een specifieke stekje gefilterd en getoont op stekjeDetail.liquid. Daarna Heb je de mogelijkheid om deze te liken of unliken en dit word met een post route verwerkt naar de Directus API.
- Favorieten ["/favorieten"](https://github.com/Saif8599/user-experience-enhanced-website/blob/main/server.js#L138-L153) : Deze route haalt de stekjes op die opgeslagen zijn in de Directus API en toont deze op de favorieten.liquid

### UI States
**Stekjes pagina**:
- Ideal state: Waneer de beschikbare stekjes getoond word kan je naar de detail pagina van een stekje gaan.

**Stekje detail**:
- Ideal state: Waneer een stekje toegevoegd word kan je hier gedetailleerd een stekje bekijken.
- Empty state: Waneer er geen beschikbare stekjes zijn word Harry getoond met feedback.
- Loading state: Zodra een gebruiker het stekje wilt liken of unliken komt er een load animatie in de button te zien.
- Succes state: Zodra de loading state voorbij is krijg je een notificatie te zien met feedback dat je actie uitgevoerd is.

![image](https://github.com/user-attachments/assets/70cfb0df-fe5d-42c5-9b65-0398d2323f57)


https://github.com/user-attachments/assets/53bbfad7-4ab9-4730-9a76-38e717e592dc


### Perceived performance
Om de performance te verbeteren heb ik gebruik gemaakt van responsive images en formats. Dit heb ik toegepast bij de images van de stekjes. Door een vaste width/height en formats te geven zorgt dit voor snellere laadsnelheden en voorkom layout shifts. 


Op de Img element gebruik ik de `loadin loading="lazy"`. Dit zorgt ervoor dat de afbeeldingen pas worden ingeladen wanneer ze in beeld komen. 
```HTML
<img src="https://fdnd-agency.directus.app/assets/{{ stekje.foto.id }}" width="{{ stekje.foto.width }}" height="{{ stekje.foto.height }}" alt="{{ stekje.naam }}" loading="lazy" style="background: #969494;">
```
Bij het laden van afbeeldingen wordt een grijze achtergrond getoond als placeholder. Dit geeft directe feedback dat de inhoud snel verschijnt, waardoor wachten minder lang voelt.

https://github.com/user-attachments/assets/8aba0d95-df47-4087-a5cd-933abe87c4b4

### Layout shift
Images hebben in de srcset een vaste width en height. Hierdoor ontstaat er geen layout shifts. 

```HTML
        <picture>
            <!-- Format voor betere compressie -->
          <source type="image/avif" srcset="https://fdnd-agency.directus.app/assets/{{ stekje.foto.id }}?format=avif?width=400&height=400">
          <source type="image/webp" srcset="https://fdnd-agency.directus.app/assets/{{ stekje.foto.id }}?format=webp?width=400&height=400">


            <!-- Fallback voor browsers die niet ondersteunen -->
          <img
            src="https://fdnd-agency.directus.app/assets/{{ stekje.foto.id }}"
            width="{{ stekje.foto.width }}"
            height="{{ stekje.foto.height }}"
            alt="{{ stekje.naam }}"
            loading="lazy"
            style="background: #969494;">
        </picture>
```

### Responsive images
In de picture element gebruik ik source tags met daarin de srcset attribute. Daarbinnen zit `https://fdnd-agency.directus.app/assets/` + dynamische data.

Voor Images uit Directus API gebruik ik `?format=avif?width=400&height=400` achter het gekozen data.. 

### View transitions
Om de performance voor de gebruiker te verbeteren heb ik view transitions toegepast.

**Multi page**

​​Bij het navigeren naar een andere pagina zie je een overgang weg vervagen.

https://github.com/user-attachments/assets/6d50e2eb-b0b5-44f7-962d-c7cd78d3bcf0


### Progressive Enhancement
Progressive enhancement is een strategie die begint met een basis die voor iedereen toegankelijk is.

1. Bepaal de core functionaliteit.
2. Bouw de functionaliteit met HTML en CSS
3. Voeg extra enhancements toe met Javascript voor betere UX

## Functionaliteiten
- **Stekje liken/unliken**: Gebruikers kunnen hun favoriete planten stekje een like geven door op een like-knop te klikken. Als je het planten stekje niet meer leuk vind kan je dit ook unliken door de rode knop te klikken.
- **Favoriete stekjes**: Alle gelikete stekjes worden automatisch opgeslagen in een persoonlijke "Favorieten"-sectie. Hierdoor kunnen gebruikers zien welke stekjes ze leuk vonden.

## Installatie

1. **Clone de repository:**
   ```bash
   git clone https://github.com/Saif8599/user-experience-enhanced-website.git
   ```
2. **Installeer de dependencies:**
   ```bash
   npm install
   ```
3. **Start de server:**
   ```bash
   npm start
   ```
4. **Toegang tot de applicatie:**
   - Open een browser en ga naar: `http://localhost:8000`
## Bronnen
- [FDND Agency](https://github.com/fdnd-agency/biebinbloei.nl/wiki/Design-Challenge)
- [Figma design](https://www.figma.com/design/LzqERL2DPlqiWDZx4Qn86d/Bieb---Bloei?node-id=20-2122&t=p28pU0kyZk1GJcBy-1)
## Licentie

This project is licensed under the terms of the [MIT license](./LICENSE).
