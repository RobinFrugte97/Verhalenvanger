# Logboek Robin

- [De opdracht](#de-opdracht)
- [Persoonlijke doelen](#persoonlijke-doelen)
- [Waar heb ik aan gewerkt](#waar-heb-ik-aan-gewerkt)
	- [Gemaakte/gestylede componenten](#gemaakte/gestylede-componenten)
	- [Aan meegewerkt/een gedeelte gemaakt](#aan-meegewerkt/een-gedeelte-gemaakt)
- [Waar ben ik trots op](#waar-ben-ik-trots-op)
	- [Components semantisch opbouwen](#components-semantisch-opbouwen)
	- [Inclusiveness fix in de Intersection Observer](#inclusiveness-fix-in-de-intersection-observer)
	- [Code in functions](#code-in-functions)
- [Wat heb ik geleerd](#wat-heb-ik-geleerd)
	- [Functions gebruiken](#functions-gebruiken)
	- [Inclusive design](#inclusive-design)
	- [SCSS](#scss)
- [Conclusie meesterproef](#conclusie-meesterproef)

## De opdracht

Voor het Scheepvaartmuseum maken we een user-generated verhalen site over het schip 'de Oranje'. Mensen moeten hun verhaal kunnen delen met de mogelijkheid om media toe te voegen zoals foto's, video's en geluidfragmenten. Deze verhalen moeten terug te vinden zijn op een overzichtpagina. Gebruikers van de site moeten ook reacties achter kunnen laten op vehalen.

## Design challenge

Hoe kunnen wij de website zo aantrekkelijk mogelijk maken voor de gebruiker, zodat de gebruiker zich uitgenodigd voelt om zijn/haar eigen verhaal achter te laten om een user-generated verhalen site draaiend te houden? Hoe nodigen we gebruikers uit verhalen te lezen?


## Getting Started

Om met dit project van start te gaan moet je eerst deze repository overzetten naar je locale machine met `git clone`.

Daarna:

```js
yarn install
```

Om te beginnen met developen:

```js
yarn run start
```

Om de code op te bouwen:

```js
yarn run build
```

## Persoonlijke doelen

- Betere JavaScript schrijven
- Modulaire JavaScript schrijven
- Inclusive design
- Schonere CSS & een nieuwe CSS extension


## Waar heb ik aan gewerkt

### Gemaakte/gestylede Componenten:

- Detailpagina verhaal
	- Detailpagina video component
	- Detailpagina tekst component
	- Detailpagina audio component
	- Detailpagina tags component
	- Detailpagina foto component
- Verhaaldeel toevoeg buttons
- Call to action verhaal pagina en homepagina
- Uploadpagina video preview
- Implementatie Intersection Observer

### Aan meegewerkt/een gedeelte gemaakt:

- Overzichtpagina layout
- Verhaal blokken op overzichtpagina en homepagina
- Uploadpagina foto en audio upload preview
- Intersection Observer

## Waar ben ik trots op

### Components semantisch opbouwen

In dit project hebben we alles opgedeeld in componenten. Elke pagina en elk deel van de pagina is opgedeeld in componenten, zodat we het werk goed kunnen verdelen onder 6 man.

Uit het code-review gesprek met Joost bleek dat ik te veel nutteloze `div` elementen om alle componenten heen had gezet, waardoor het én niet semantisch, én niet inclusive was.
Na het gesprek heb ik alle overbodige `div` elementen om de componenten weggehaald. De reden dat de `div` elementen om de componenten heen stonden was styling. Het was heel makkelijk om
alle componenten uit te lijnen met de `div` elementen met dezelfde `class` om elk component zetten om zo het geheel te stylen. Ik had daarbij niet nagedacht over de semantiek en mensen
die hulptools gebruiken zoals screenreaders.

Na het gesprek met Joost ben ik gelijk aan de gang gegaan. Ik heb alle overbodige styling elementen verwijderd en de correcte elementen de juiste styling meegegeven.
Ik heb mijn best gedaan om de componenten semantisch te krijgen. In het vervolg van het project ben ik meer gaan kijken naar de semantiek van componenten waar ik mee te maken kreeg.

Vakken toegepast:
- WAFS
- CSSTTR
- Browser technologies

Meer code en context:

[Link naar de bijbehoordende commit](https://github.com/moniac/meesterproef-scheepvaartmuseum/pull/74/commits/303bb7c1aafd17384ae26c452072275046f2a162)

### Inclusiveness fix in de Intersection Observer

Op de detailpagina hadden we het idee om elk verhaal deel met een animatie te visualiseren. Daarvoor hebben we voor de Intersection Observer gekozen. De Intersection Observer is zo gebouwd dat we alle componenten er één voor één in kunnen zetten. Jamie heeft daarmee iets super tofs neergezet.

Wanneer de Intersection Observer klaar was om in de verhaal pagina's te zetten, bleek dat alle componenten in een aantal containers gezet moesten worden. Het viel mij gelijk op, omdat ik daarvoor ook al bezig was met de semantiek van de individuele componenten. Ik heb met JavaScript de code omgeschreven dat de containers om alle componenten niet meer nodig zijn voor de Intersection Observer. De code kijkt nu naar alle child elements in de hoofdcontainer van de verhaal pagina's. Op al deze children wordt de Intersection Observer uitgevoerd zonder dat er containers nodig zijn.

Vakken toegepast:
- WAFS
- Browser technologies

Code snippet:

``` js
function selectComponents() {
	const allElements = document.querySelectorAll( '.container--main > *' )

	function enhancedAddClass( allElements ) {
		allElements.classList.add( 'detail__content' )
	}

	function perComponent() {
		allElements.forEach( enhancedAddClass )
		enhancedDetailInit()
	}
	perComponent()
}
```

Meer context en code:

[Link naar de bijbehoordende pull  request](https://github.com/moniac/meesterproef-scheepvaartmuseum/pull/89/files)

### Code in functions

Wat ik nu voor het eerst goed heb gedaan, is goed en consistent gebruik gemaakt van functions. Waar mogelijk heb ik mijn JavaScript opgedeeld in functions met een naam, zodat deze functions later gebruik konden worden.

Vakken toegepast:
- WAFS
- Performance matters

Code snippet:

``` js
function toggleToolButton() {
	const addStoryButtons = document.querySelectorAll( '.button--regular--hidden' )
	const plusButton = document.querySelector( '.button--show' )
	if ( !addStoryButtons || !plusButton ) return
	function toggleButtons( addStoryButtons ){
		addStoryButtons.classList.toggle( 'button--regular--hidden' )
	}
	function buttonEventlistener() {
		addStoryButtons.forEach( toggleButtons )
		if ( plusButton.classList.contains( 'button--show--rotate' ) ) {
			plusButton.classList.remove( 'button--show--rotate' )
			plusButton.classList.add( 'button--show' )
		} else {
			plusButton.classList.remove( 'button--show' )
			plusButton.classList.add( 'button--show--rotate' )
		}
	}
	plusButton.addEventListener( 'click', buttonEventlistener )
}
export default toggleToolButton
```
### Uploadpagina video preview

Om de gebruiker feedback te geven op de verhaaluploadpagina, hebben we van elk component
een preview gemaakt. Wanneer de gebruiker een youtubelink in het invulveld heeft geplaatst en
de focus uit het invuldveld verdwijnt, wordt een iframe element aangemaakt met de ingevulde link
erin geplaatst.

Vakken toegepast:
- Web design
- WAFS

Code snippet:

``` js
function videoPreview() {
	const iframeLink = input.value
	if ( ! iframeLink.indexOf( 'https://youtube.com/' ) ) return
	if ( iframeLink.indexOf( '/watch?v=' ) ) {
		const replacedLink = iframeLink.replace( '/watch?v=', '/embed/' )
		const iframe = document.createElement( 'iframe' )
		iframe.classList.add( 'videoPreview' )
		iframe.src = replacedLink
		TweenMax.set( iframe, {autoAlpha: 0, y: -10} )
		event.target.insertAdjacentElement( 'afterEnd', iframe )
		TweenMax.to( iframe, .4, {autoAlpha: 1, y: 0, clearProps: 'all' } )

	} else if ( iframeLink.indexOf( '/embed/' ) ) {
		const iframe = document.createElement( 'iframe' )
		iframe.classList.add( 'videoPreview' )
		iframe.src = iframeLink
		TweenMax.set( iframe, {autoAlpha: 0, y: -10} )
		event.target.insertAdjacentElement( 'afterEnd', iframe )
		TweenMax.to( iframe, .4, {autoAlpha: 1, y: 0, clearProps: 'all' } )
	}
}
```

## Wat heb ik geleerd

### Functions gebruiken

Mede dankzij de onderlinge afspraken die we binnen de groep hebben gemaakt over code conventions, heb ik bij elk stuk JavaScript de code in functions verdeeld. Dit maakt de code een stuk efficiënter. Je kunt alles hergebruiken. Elk JavaScript bestand per component moet naar het hoofdbestand worden geïmporteerd. Door vanaf het begin al met named functions te werken gaat dit super makkelijk.


Uit de index.js:

``` js
import header from './components/header/header.js'
import {uploadForm, disableAllInputs} from './components/uploadForm/uploadForm.js'
import setUpMap from './components/map/map.js'
import toggleToolButton from './components/storyAddItemButton/storyAddItemButton.js'
import autocompleteFromApiInit from './components/lib/autocomplete.js'
import colorThief from './components/storyHighlight/storyHighlight.js'
import initReportComponent from './components/reportComponent/reportComponent.js'
import removeStoryInit from './components/adminStory/adminStory.js'
import { selectComponents } from './components/enhancedDetail/enhancedDetail.js'
import {optimizedResize, animateOnIntersect} from './components/lib/helpers.js'
import initSW from './service-worker-handler.js'


( function IIFE () {

	if( !( document.documentElement.classList && document.querySelectorAll ) ) return
	animateOnIntersect()
	setUpMap()
	selectComponents()
	header()
	uploadForm()
	disableAllInputs()
	toggleToolButton()
	optimizedResize()
	autocompleteFromApiInit()
	colorThief()
	initReportComponent()
	removeStoryInit()
	initSW()
} )()
```

### Inclusive design

Na het gesprek met Joost heb ik veel meer gelet op semantiek. Ik heb opgelet bij de dingen die ik heb gemaakt dat ze niet alleen werken of awesome zijn, maar dat ze ook semantisch correct zijn. Bij elk component dat ik heb gemaakt, heb ik gecheckt en met de groep overlegd/gediscussieerd over de correcte HTML elementen voor de case. Ik heb dit project geleerd dat ik vanaf het begin moet nadenken over semantiek, accessibility en inclusiveness. Doe je het in één keer goed of besteed je er in ieder geval aandacht aan, dan scheelt dat je een hoop werk achteraf. Je maakt het web ook een betere plek voor iedereen.

### SCSS

Tijdens het project hebben we gebruik gemaakt van SCSS. Dit heeft heel veel tijd gescheeld, omdat er gezamelijk een primary en secondary color bepaald werd en in een variable gezet werd. Net als bij de JavaScript kon er enorm veel code hergebruikt worden dankzij het gebruik van SCSS. SCSS heeft een heel duidelijke syntax, waardoor ik hele nette css ging schrijven. Dankzij het nesten van SCSS is de code ook heel overzichtelijk. Ik heb niet meer onnodig dubbele CSS geschreven, omdat ik het overzicht over selectors verloor, dankzij het overzichtelijke nesten dat SCSS aanbiedt.

Code snippet van de verhaal blokjes op de homepagina en overzichtpagina:

``` SCSS
.stories {
	&__grid {
		display: flex;
		flex-flow: row wrap;
		justify-content: center;
		align-content: stretch;

		&--card {
			position: relative;
			width: 100%;
			height: 16rem;
			color: $secondary-color-text;
			transition: transform 150ms ease-out;
			margin: 2% 0;
			@include mq('medium') {
				width: 48%;

				&:nth-child(odd) {
					margin-right: 2%;
				}

				&:nth-child(even) {
					margin-left: 2%;
				}

				&:hover {
					&-background {
						transform: scale(1.05);
					}
					transform: scale(1.05);
				}
			}
			@include mq('large') {
				height: 20rem;
			}

			&-background {
				width: 70%;
				height: 100%;
				position: absolute;
				right: 0;
				object-fit: cover;
				transition: transform 150ms ease-out;
			}

			article {
				position: absolute;
				top: 0;
				width: 100%;
				height: 100%;
				padding: 2rem;
				text-shadow: 1px 1px 0 #000;
				background: linear-gradient(to right, black 30%, rgba(0, 0, 0, 0.7) 40%, rgba(0, 0, 0, 0.2) 80%, rgba(0, 0, 0, 0) 100%);

				h2 {
					color: $secondary-color-text;
					@include mq('large') {
						max-width: 60%;
					}
				}

				p {
					max-width: 60%;
					position: absolute;
					bottom: 2rem;
					margin-bottom: 0;
				}
			}
		}
	}
}
```

## Conclusie meesterproef

Voordat ik begon aan de meesterproef, was ik van plan om alleen te werken. Dat was ik gewend van de minor, omdat alle voorgaande projecten/vakken individueel waren. In het begin vond ik het lastig om mijn deel uit te kiezen en daar aan te gaan werken. Dankzij het opdelen van het geheel in componenten ging dit uiteindelijk een stuk makkelijker. Ook was het aan het begin enorm wennen aan de tooling en het feit dat je je aan de code- en naamgeving conventies moet houden binnen het team. Dit leidt er wel toe dat je goed nadenk over wat je codeert.

## Contributors

| Contributor  | Github URL                       |
|--------------|:-------------------------------  |
| Mees         | https://github.com/meesrutten       |
| Mo           | https://github.com/moniac        |
| Niels        | https://github.com/NielsFS       |
| Jamie        | https://github.com/jajan20 |
| Servin       | https://github.com/servinlp      |
