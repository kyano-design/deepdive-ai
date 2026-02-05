- HTML / CSS / JavaScript
Basis voor een snelle, lichte interface.
- Vue.js
Component‑gebaseerde structuur, ideaal voor dynamische UI‑elementen zoals speloverzichten, saldo‑updates en notificaties.
- TailwindCSS
Snelle styling zonder rommelige CSS‑bestanden.
Backend
- Node.js (Express)
Geschikt voor real‑time interacties, API‑routes en snelle request‑handling.
- WebSockets (Socket.io)
Voor live updates zoals saldo, spelresultaten of dagelijkse bonusmeldingen.
- JWT‑authenticatie
Lichtgewicht en veilig voor sessiebeheer.
Database
- MongoDB
Flexibel voor spelersprofielen, speelgeschiedenis, limieten en nepgeldtransacties.
- Redis (optioneel)
Voor caching van speldata en rate‑limiting bij bonusclaims.
Hosting & Infra
- Vercel / Netlify voor frontend
- Render / Railway / AWS voor backend
- MongoDB Atlas voor database
- GitHub Actions voor CI/CD

Globale Architectuur
1. Client–Server Model
De frontend communiceert met een REST‑API voor standaardacties (registratie, profiel, limieten) en met WebSockets voor live spelinteractie.
2. Componenten
- Frontend SPA
UI, routing, speloverzichten, saldo‑weergave.
- API‑server
Auth, spelersbeheer, nepgeldlogica, limieten, notificaties.
- Game Engine (server‑side module)
Berekent spelresultaten, random outcomes (met RNG‑library), verwerkt inzetten.
- Database
Profielen, transacties, spelgeschiedenis, limieten.
- WebSocket‑laag
Real‑time saldo‑updates, spelresultaten, bonusmeldingen.
3. Datastromen
- Speler logt in → JWT token
- Speler start spel → inzet wordt naar backend gestuurd
- Backend berekent resultaat → saldo wordt geüpdatet
- WebSocket stuurt live update terug naar speler
- Geschiedenis wordt opgeslagen in MongoDB

Belangrijke Keuzes
1. Nepgeld i.p.v. echt geld
- Geen financiële risico’s
- Minder strenge wetgeving
- Toegankelijker voor casual spelers
- Mogelijkheid tot dagelijkse rewards en speelse progressie
2. RNG op server‑side
- Voorkomt manipulatie door spelers
- Resultaten zijn eerlijk en controleerbaar
- Server bepaalt altijd de uitkomst, client toont alleen animatie
3. WebSockets voor real‑time feedback
- Direct saldo‑updates
- Snellere spelervaring
- Mogelijkheid voor live events of minigames
4. MongoDB voor flexibiliteit
- Spelgeschiedenis en transacties verschillen per speltype
- Document‑structuur past goed bij variabele data
- Makkelijk schaalbaar
5. Limieten en verantwoord spelen
Ook bij nepgeld blijft dit belangrijk:
- Speeltijdlimieten
- Ronde‑limieten
- Dagelijkse bonuslimieten

Bekende Risico’s
1. Misbruik van nepgeldbonussen
- Spelers proberen meerdere accounts te maken
- Oplossing: IP‑rate‑limiting, device‑fingerprinting, e‑mail‑verificatie
2. Overbelasting van WebSockets
- Veel gelijktijdige spelers kunnen de server belasten
- Oplossing: horizontaal schalen, Redis pub/sub
3. RNG‑wantrouwen
- Spelers kunnen denken dat het “rigged” is
- Oplossing: transparante uitleg over RNG, seed‑documentatie
4. Cheating via client‑manipulatie
- Client kan worden aangepast
- Oplossing: alle logica server‑side, client is puur UI
5. Dataconsistentie
- Snelle updates kunnen race conditions veroorzaken
- Oplossing: atomic updates in MongoDB, transacties waar nodig
