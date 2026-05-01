Statement of Originality

This report is submitted as part requirement for the degree of BSc Computer Science
with Artificial Intelligence at the University of Sussex. It is the product of my own labour
except where indicated in the text. The report may be freely copied and distributed
provided the source is acknowledged. I hereby give / withhold permission for a copy of
this report to be loaned out to students in future years .

Candidate no. 283284
signed
30/4/2026

Acknowledgement

I would like to thank my advisor Dr Jeff Mitchell for supporting me throughout my final year project, especially considering this project is not within his field of speciality. I would also like to thank my parents, my sister and my church shepherd for the guidance they have provided me during difficult times.

Abstract

This project presents a web application that tracks personal Pokemon trading card game collection using the unofficial Pokemon TCG Developer Portal API which is now part of Scrydex . The aim is to provide collectors with a transparent, web‑based tracker focused on Pokémon, addressing limitations in existing platforms such as Collectr, PriceCharting and TCGPlayer, which often restrict data access and custom analysis through proprietary backends and paid APIs.

The system adopts a lightweight full stack architecture with a frontend built with Vite and React Router, and a Python FastAPI backend served by Uvicorn that mediates all communication requests with the external Pokemon API. Users can search for cards and sets, and manage a personal collection that records cards; collections are persisted in a simple JSON file to keep the web implementation lightweight, at the cost of scalability and robustness compared with a full database.

1. Introduction

1.1 Motivation and context

Since childhood, I have been watching the Pokemon tv show as well as playing the games. I witnessed generations of shows and pokemon that came out in more than a decade. In recent years, I have dwelled myself in collecting as well as playing the Pokemon trading card game. With different sets of cards, my collection grew and I found some applications that offered to keep track of the user's collection.

With the recent development of the Pokemon TCG community, these types of applications started to offer a wider range of features such as real-time market place and checking for authenticated grading card prices.Hence, I wanted to develop an application with same paid features without users payment. 

This project will give me the opportunity to create the collection application that suited to my requirement, as well as discovering limitations of what I can implement into the application.

1.2 Project Aims and Objectives
The aim of this project is to design and develop a lightweight, web-based Pokémon Trading Card Game (PTCG) collection tracker that allows users to search for cards using an open card-data API and manage a personal collection through a clean and accessible interface, applying full-stack web development principles using modern frameworks and libraries.
The Objective of this project are as following;
To research and critically evaluate existing Pokémon TCG collection tracking platforms, identifying their features, limitations and the gaps that a new application could address.
To design and implement a full-stack web application consisting of a React frontend and a Python FastAPI backend, communicating with the Pokémon TCG Developer Portal API to retrieve live card data.
To implement core collection management functionality, including searching for cards, adding them to a personal collection with relevant metadata such as condition, language and variant, and removing them.
To implement a basic authentication system with hashed password storage, and to critically discuss how this could be extended with stronger mechanisms such as JWT and two-factor authentication.
To evaluate the prototype through structured functional testing, assessing whether core requirements were met and identifying limitations and future improvements.

1.3 Report Structure
The remainder of this report is organised as follows.
Chapter 2 discusses the professional and ethical considerations relevant to the project, including compliance with the BCS Code of Conduct, data protection, and the use of third‑party APIs and user testing procedures.
Chapter 3 reviews background and related work, covering existing Pokémon TCG collection platforms, the Pokémon TCG Developer Portal/Scrydex API, and the technical and academic context for a web‑based collection tracker.
Chapter 4 presents the requirements analysis, including the target users, functional and non‑functional requirements, and key user stories that guided the design of the prototype.
Chapter 5 describes the system design and architecture, outlining the overall structure of the React frontend, FastAPI backend, JSON-based storage, and integration with the external card‑data API.
Chapter 6 details the implementation of the system, explaining the main frontend components and routes, backend endpoints and services, and the security and error‑handling mechanisms that were actually implemented.
Chapter 7 reports on testing and evaluation, including unit and component tests, manual functional testing of core workflows, and a critical assessment of how well the prototype meets the stated requirements.
Chapter 8 concludes the report by summarising the main achievements, reflecting on limitations, and outlining future work such as database integration, more robust authentication, and possible pricing‑data extensions.

2. Professional Conduct and Compliance
This project follows the professional expectations set out in the BCS Code of Conduct and the University of Sussex Informatics Ethics Framework. However, the project did not include user testing to evaluate usability and interface design. Instead, predefined test data will be used for testing and evaluating the interface design and the functionality of the web app.
2.1 Third-Party APIs and Intellectual Property
The application depends on external data sources, the Pokemon API, which was later integrated into the Scrydex platform, and public information from other collection tools such as Collectr, PriceCharting and TCGPlayer. To ensure ethical and lawful use of these resources, the project restricted API usage to retrieving card metadata (names, sets, identifiers, images and types) through documented endpoints and respected the terms of service of each provider. No proprietary pricing datasets or high‑volume transaction data were scraped or redistributed.
All screenshots, examples and descriptions of existing applications are used for critical comparison only and are properly referenced in the report. The project does not attempt to clone or redistribute their interfaces, databases or media assets. Any card images or logos included during development were either implemented  directly from the official API in a way consistent with its documentation or treated as temporary assets for prototyping purposes only. This approach was chosen to avoid infringement of intellectual property rights while still enabling meaningful analysis of the existing ecosystem.

2.2 Ethical Procedures and Research Integrity
As no human participants were involved in the testing of this project, the ethical risks associated with user studies, such as data collection, informed consent, and participant anonymity, were not applicable. Testing and evaluation of the application were conducted using predefined test data and structured functional test cases rather than direct interaction with human users. This decision simplified the ethical scope of the project while still allowing meaningful evaluation of core application functionality.
The project followed the University of Sussex Code of Practice for Research and the School of Informatics guidelines throughout development. All code, written content and documentation produced in this report are original work unless explicitly referenced. 
External libraries, APIs and third-party tools used throughout the project are properly acknowledged in the references section of this report. The use of any AI-assisted writing tools was conducted in compliance with the University of Sussex policy on the use of generative AI in assessed work. Any limitations in the testing approach, such as the absence of real-user feedback and the use of predefined rather than live user data, are reported transparently in the evaluation chapter of this report.



3. Background and related works 
3.1 Pokemon TCG and Collection Tracking
The Pokémon Trading Card Game (TCG) was first introduced in Japan in 1996 by Media Factory, before being licensed internationally and released in North America and Europe in 1998 (https://japan-figure.com/blogs/news/when-did-pokemon-cards-come-out ). Following collaborations with different companies such as Mcdonalds, postal offices, which offered exclusive promotion cards for specific collaboration. 
The popularity of the Pokémon TCG has created a large and active collector community, spanning casual players who collect cards for enjoyment to serious investors who track the financial value of their collections. The secondary market for Pokémon cards has grown significantly, with rare and graded cards regularly selling for thousands of pounds at auction (https://www.bbc.co.uk/news/business-56413186 ). This growth has led to an increasing demand for digital tools that allow collectors to catalogue, manage and evaluate their collections efficiently.
Collection tracking refers to the process of recording and organising a set of owned items, in this case trading cards, with associated metadata such as condition, variant, language and whether a card is graded or ungraded. Digital collection trackers provide an accessible alternative to manual spreadsheet-based approaches, offering search, filtering and in some cases automated price retrieval. For Pokémon collectors specifically, the complexity of the card catalogue, spanning hundreds of sets, multiple languages, variant prints such as reverse holos, full arts and secret rares, and professional grading scales, makes digital tracking tools particularly valuable.

3.2 Existing Collection Tracking Platforms
Several platforms currently exist that offer collection tracking for trading card games including Pokémon. The most relevant platforms analysed during this project were Collectr, PriceCharting, TCGPlayer, Dex, AcornTCG and RareCandy.
Collectr (https://getcollectr.com/ ) is a dedicated TCG collection tracker that allows users to log their cards, track estimated collection value and view market trends. It offers a clean interface and supports a wide range of sets and variants. However, Collectr is a proprietary platform with limited API access, meaning users cannot easily export or interact with their data programmatically.
PriceCharting (https://www.pricecharting.com/ ) is a broader multi-category platform covering video games, sports cards and trading card games including Pokémon. It provides historical price data and allows basic collection logging. Its primary strength is pricing data rather than metadata-rich collection management, and it does not focus exclusively on Pokémon. 
TCGPlayer ( https://www.tcgplayer.com/ ) is primarily a marketplace platform for buying and selling trading cards, though it also offers collection tracking features. Its collection tools are tightly integrated with its marketplace, which can be useful for users looking to buy or sell but less appropriate for collectors who simply want to catalogue what they own. 
Dex (https://dextcg.com/ ), is a Pokémon-focused collection tracker and card database. It offers detailed card information and collection management. Its rebrand and integration with the Pokemon API during the course of this project made it particularly relevant as a competitor and data source.  
AcornTCG (https://www.acorntcg.com/ ) and RareCandy (https://www.rarecandy.com/ )  are smaller, community-oriented platforms that offer collection tracking and pricing tools for Pokémon cards. Both provide useful features but have smaller user bases and less comprehensive data coverage than the larger platforms.  
Across these platforms, several common limitations were identified. Most are proprietary, meaning users have no access to underlying data or APIs for custom analysis. Many focus heavily on pricing and market activity rather than detailed collection metadata. Few support granular variant tracking such as distinguishing between first edition, shadowless, reverse holo and full art prints of the same card. These limitations provided the motivation for developing this application as a transparent, open API-based alternative focused specifically on Pokémon collection management. 

3.3 API and Data Accessibility
An Application Programming Interface (API) is a defined interface through which software systems communicate and exchange data. In the context of web applications, REST APIs are commonly used to allow a frontend client to request data from a backend server or external service over HTTP (https://developer.mozilla.org/en-US/docs/Web/API) . 
The primary external data source used in this project is the Pokemon API, a community-developed and officially recognised API providing access to Pokémon card metadata including names, set information, card types, images and identifiers. At the time development began, this API was hosted independently at pokemontcg.io. During the course of the project, the API was acquired and integrated into the Scrydex platform, which assumed maintenance and hosting responsibilities. Following the transition, general API reliability improved; however, intermittent server timeout issues continued to occur after sustained periods of querying, which presented a real implementation challenge that is discussed further in Chapter 6 (http://scrydex.com/ ). 
The availability of an open, documented card-data API is significant because it allows developers to build tools that retrieve accurate, up-to-date card information without scraping proprietary databases or paying for commercial data access. This contrasts with platforms such as TCGPlayer and Collectr, which do not offer open APIs for their collection or pricing data, limiting third-party development and user data portability. The decision to build PotaTCG around this open API was therefore both a practical and an ethical one, ensuring that all card data used in the application came from a legitimate and documented source. 

3.4 Technical and Academic Context
The technical implementation of PotaTCG draws on several established concepts in full-stack web development, human-computer interaction and software engineering. 
React is a widely used open-source JavaScript library for building user interfaces, developed and maintained by Meta. (https://react.dev/ ). It follows a component-based architecture in which the user interface is composed of reusable, self-contained components that manage their own state and re-render efficiently when data changes.  This model is well suited to the features of a collection tracker, where card lists, search results and collection views must update dynamically in response to user interaction without requiring full page reloads. 
Vite is a modern frontend build tool that provides fast development server startup and hot module replacement, making it particularly effective for React-based projects during development (https://vite.dev/ ).
FastAPI is a Python web framework designed for building APIs quickly and efficiently, with automatic data validation through Pydantic and built-in support for asynchronous request handling via Uvicorn (http://fastapi.tiangolo.com/ ). Its design encourages clean separation between route handling, service logic and data schemas, which aligns with good software engineering practice for maintainable backend code. 
From a human-computer interaction perspective, usability is a key consideration for any collection management tool. Nielsen's usability heuristics, including visibility of system status, error prevention and recognition rather than recall,  (Nielsen, J. (1993). Usability engineering. AP Professional. https://sussex.idm.oclc.org/login?url=https://ebookcentral.proquest.com/lib/suss/detail.action?docID=1190977) provides a useful framework for evaluating whether the interface supports effective and efficient collection management. These principles informed decisions about how search results are displayed, how feedback is given when adding or removing cards, and how navigation is structured across the application.
The use of JSON as a lightweight data format for persistence reflects an approach in non-scalable web prototypes where the overhead of a full database is not justified by the scale of the data. However, this approach is not recommended with scalable business models and application development. Academic and industry literature consistently identifies relational databases such as SQLite or PostgreSQL as the appropriate solution for multi-user, persistent web application storage. 

3.5 Summary and Implications

The review of existing platforms and related work reveals several clear gaps that this project aims to address. Most established collection tracking tools are proprietary and heavily focused on pricing and market activity, with limited support for recording detailed card metadata such as condition, language and variant. For collectors who want a straightforward way to catalogue what they own rather than track market values, these platforms can feel unnecessarily complex or restrictive.
From a technical perspective, the combination of React, FastAPI and an open REST API offers a modern, modular architecture that is appropriate for a prototype collection tracker. The decision to use JSON-based file storage rather than a database is a pragmatic trade-off for the scope of this project, and the implications of this decision are carried forward into the design, implementation and evaluation chapter.
Overall, the background research supports the rationale for this project as a focused, Pokémon-specific collection tracker that prioritises ease of use and data transparency over the pricing-driven features of larger existing platforms, while acknowledging that those platforms currently offer a significantly broader range of functionality.


4. Requirement analysis
4.1 User Needs and Target Audience
The primary target audience for this application is Pokémon TCG collectors who want a straightforward digital tool to catalogue and manage their card collections. Based on analysis of existing platforms and the broader collector community, three broad user groups were identified,
Casual collectors who want a simple way to record what cards they own, with basic details such as condition and language, without needing to engage with complex pricing or market tools.
A reliable, accurate and easy-to-use interface is needed for searching cards and recording ownership. 
4.2 Functional Requirements
The functional requirements for this project were defined using the MoSCoW prioritisation method, which categorises requirements into Must have, Should have, Could have and Won't have for this iteration. This approach was used to focus development effort on the features most essential to a working prototype. 
The Must have requirements represent the minimum viable product: 
a working search interface, the ability to add and remove cards from a personal collection, and a basic authentication system. The Should have and Could have requirements represent desirable extensions that were considered during design but were not all fully implemented in the prototype due to time constraints. 
4.3 Non-Functional Requirements
Non-functional requirements describe the qualities the system should have rather than specific features. The non-functional requirements were identified for this project. It should be noted that these describe the intended qualities of the full system; the prototype only partially satisfies some of them, particularly scalability and advanced security, which are discussed further in Chapter 7. 
4.4 User Stories and Use Cases
User stories were used to capture the core interactions a collector would need to perform within the application. They are written from the perspective of the end user to keep the focus on user value rather than technical implementation. 
Must have stories:
As a collector, I want to search for Pokémon cards by name so that I can find cards I am looking for quickly.
As a collector, I want to add a card to my collection so that I have an accurate record of what I own.
As a collector, I want to remove a card from my collection so that my records stay up to date.
As a user, I want to register and log in to the application so that my collection is private and personal to me.
Should have stories:
As a collector, I want to view my full collection in one place so that I can see everything I own at a glance.
As a collector, I want to see card images and set information so that I can identify cards accurately.
As a user, I want to change my password and display name so it is what I prefer to do .
As a user, I want to search using a filter option so that I can search for specifically what I want.
Could have stories:
As a collector, I want to export my collection data so that I can back it up or use it elsewhere.
As a collector, I want to see estimated prices for my cards so that I can track the approximate value of my collection.
As a collector, I want to record whether a card is graded and its grade so that I can distinguish between raw and graded copies.
As a collector, I want to connect with other collectors so that I can see their collections.

4.5 Application Conceptual Model
The domain model below describes the key conceptual entities within the system and their relationships. This model represents the logical structure of the application regardless of how data is physically stored in the prototype.
A User owns one Collection, which contains zero or more CollectionItems. Each CollectionItem references a Card, which is retrieved from the external Pokémon TCG API and is not stored locally. A CollectionItem also carries metadata specific to that user's copy of the card, such as condition, language, variant and quantity.

5. System design and architecture
5.1 Overall Architecture
The application is implemented as a lightweight full-stack web application consisting of two main parts: 
a React frontend and a Python backend built using the FastAPI framework, served by Uvicorn. The two parts communicate over HTTP, with the frontend making requests to the backend for all data operations. The backend is responsible for handling business logic, managing user and collection data, and mediating all communication with the external Pokemon API.
Rather than using a dedicated database management system, user and collection data is stored in a flat JSON file on the server. The implications of this are discussed in Chapter 7.

5.2 Frontend Architecture
The frontend is built in JavaScript using the React library, with Vite as the frontend development and build tool. The application is structured around a single root component that wraps everything in two key providers: 
one that manages global authentication state, and the other one that handles client-side routing using React Router DOM.
Authentication state is shared across the application through React's Context API. When a user logs in, their session information is stored in the browser's local storage and made available to any component that needs it, without having to pass data manually through every level of the component tree. Pages that require a logged-in user are protected by a route guard that redirects unauthenticated users back to the login page.
The frontend is divided into pages, reusable components and supporting logic for API calls and state management. The different pages include the main views of the application  login, card browsing and collection management. Components are smaller reusable pieces shared across those pages, such as the buttons on the page, search bar and navigation bar. API call logic is kept separate from the UI, with dedicated modules handling communication in the backend.

5.3 Backend Architecture
The backend is a Python application built with FastAPI and served by Uvicorn, which supports asynchronous request handling. The application is structured into three clear layers: 
routes, services and data/external communication.
The routes layer is the entry point for all incoming requests. Three separate routers handle distinct areas of the application: 
One route for authentication and account management, another route for card and set search, and a route for collection operations. Each router is responsible only for receiving requests, validating input and returning responses.
The services layer contains the actual logic of the application. User management and password handling are handled by a user service, collection add and remove operations by the other service, and all outbound communication with the external Pokémon TCG API by the API. Keeping this logic in the service layer rather than directly in the routes means each area can be developed, tested and changed independently.
Input validation is handled automatically by Pydantic schema definitions, which FastAPI uses to check that incoming request data matches the expected format before it reaches the route handler. Environment configuration, including the external API key and base URL, is loaded separately through a core configuration module so that credentials are not hardcoded into the source.
CORS middleware is applied at the application level to allow the frontend to communicate with the backend during development. The current configuration allows requests from any origin, which is appropriate for a local development environment but would need to be restricted to a specific origin before any production deployment.

5.4 Data Storage Design
All user and collection data is stored in a single JSON file on the backend server. This file is loaded into memory at the start of every read or write operation and saved back to disk once the operation is complete. There is no caching layer between requests.
Each user record contains a randomly generated salt, a hashed password, a display name and a collection array. Each item in the collection array is a card object from the Pokémon TCG API with an added quantity field to track how many copies of that card the user owns.
This approach was chosen because it is simple to implement and requires no additional infrastructure. 

5.5 External API Integration
Communication with Pokemon API is handled by a dedicated service using the httpx library, which provides asynchronous HTTP request support compatible with FastAPI's async model.
Card search requests are built by combining a name filter and a set name filter using the API's query language, so a single search term can match against both card names and set names. Set browsing requests return results ordered by release date, with the most recent sets shown first. An API key, if configured, is included in each request header.
A ten-second timeout is applied to all requests. If the timeout is exceeded or the API returns an error, the error is passed back to the frontend as a readable response. No retry logic is implemented. If a request fails it fails immediately. There is also no server-side caching, meaning every search triggers a fresh request to the external API. Both of these are acknowledged limitations discussed further in chapter 7.

6. Implementation 
6.1 Frontend Implementation
The frontend was originally planned to include a simple two-page structure covering browsing and collection management. During development this expanded to include an account management page and a more structured authentication flow than initially anticipated. The final frontend consists of four pages: LoginPage, BrowsePage, CollectionPage and AccountPage.
LoginPage is the entry point for all users. It presents a form for both logging in and registering a new account. On successful login the user is redirected to BrowsePage. If credentials are incorrect or a required field is missing, an error message is displayed to the user inline. Registration follows the same flow, if the chosen username is already taken, an error is returned from the backend and surfaced to the user on the same page.
BrowsePage is the main card search interface. Users can search for cards by name or set using a search bar component. Results are fetched from the backend on submission and displayed as a list of card items, each showing the card image, name and set information retrieved from the Pokémon TCG API. If a search returns no results or the API request fails, an appropriate message is shown rather than leaving the page blank. Users can add a card to their collection directly from this page.
CollectionPage displays the cards the logged-in user has saved to their collection. It fetches the collection from the backend on page load and renders each card item with its recorded quantity. Users can remove cards or decrement quantities from this view. If the collection is empty, an empty state message is shown to communicate this clearly rather than rendering an empty list silently.
AccountPage was added during development beyond the original plan, in response to the need for users to be able to manage their own account details. It allows the user to change their display name and password. These operations are sent to the backend as separate requests, and success or failure is communicated back to the user through an inline message on the page.
Global authentication state is managed through the AuthContext provider, which wraps the entire application. When a user logs in, their username and display name are stored in local storage and loaded back into the context on page refresh, so the session persists across browser reloads as long as the data remains in local storage. All pages that require authentication are wrapped in a PrivateRoute component that checks for an active session and redirects to LoginPage if none is found.

6.2 Backend Implementation
The backend was implemented using FastAPI with a clear separation between the routes layer and the services layer. The intent was to keep route handlers thin, responsible only for receiving and validating requests, while all meaningful logic lived in the service layer.
Authentication is handled by the auth router. The register endpoint strips whitespace from the submitted username, checks that both username and password are present, and returns a 400 error if either is missing. It then calls the user service to attempt registration, which returns false if the username is already taken, resulting in a 409 conflict response. On success, the new user record is created and a confirmation response is returned. The login endpoint follows a similar pattern, calling the user service to verify the submitted credentials and returning a 401 response if verification fails.
The account management endpoints, change display name, change password and delete account, are also part of the auth router. Each requires the correct current password before any change is made, with appropriate error responses returned if verification fails.
Card search is handled by the cards router, which exposes two endpoints: one for searching cards and one for browsing sets. Both endpoints accept query parameters and delegate immediately to the Pokémon API service, returning whatever that service provides. The router itself contains no logic beyond passing the parameters through.
Collection management is handled by the collection router. Adding a card checks whether that card already exists in the user's collection by matching on the card's unique identifier. The quantity is incremented, if the card is in the collection. If it does not, the card is added as a new entry with a quantity of one. Removing a card does the opposite of adding a card, it decrements the quantity, and removes the entry entirely once the quantity reaches zero. After every add or remove operation the updated collection is written back to the JSON file immediately.

6.3 Security Mechanisms

Password security and account management were a core concern from the beginning of the project. Rather than storing passwords in plaintext, the application uses password hashing, implemented through Python's built-in hashlib library. A random salt is generated for each user on registration using Python's secrets module. The salt is stored alongside the password hash in the user record. When a user logs in, the submitted password is hashed using the stored salt and the result is compared against the stored hash. This means that even if the JSON file were accessed directly, passwords could not be recovered from it.
The use of a unique salt per user also means that two users with the same password will have different hashes, preventing rainbow table attacks.

6.4 API Handling Limitations
During the project proposal phase, the Pokemon developer port API was deserted for 2 years, and had issues with requesting and fetching data to the server. This decremented the stability of the web app and often crashes during runtime. After the API became part of Scrydex, the general stability improved with much less timeout errors occurring. However, timeout still is still encountered particularly after a sustained number of requests within a single session.
To handle this, a ten-second timeout is applied to all outbound requests using the httpx async client. If a request exceeds this timeout, an error is raised and propagated back to the frontend, where it is displayed to the user as an error message rather than causing the application to crash or hang. The same error handling applies to non-200 HTTP responses from the API.

7. Testing and evaluation
7.1 Testing Strategy
7.2 Functional Testing Results
7.3 Requirements Coverage
7.4 User Testing and Usability
7.5 Critical Evaluation

8. Conclusion
8.1 Summary of Achievements
8.2 Reflection
8.3 Future Work



Garnt Chart Project logs
Testing document
Additional system diagrams
