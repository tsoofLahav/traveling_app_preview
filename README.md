<div align="center">
  <h1>The Betesh Group · Travel App</h1>
  <p><strong>A bespoke mobile travel catalog, designed and built for a company’s needs</strong></p>
  <p>Flutter · Dart · Python · Flask · PostgreSQL · Azure Blob Storage</p>
  <img src="docs/media/app-preview.png" alt="The Betesh Group app: destination selection, restaurants, and hotel amenities" width="720" />
  <p><a href="https://www.youtube.com/shorts/z2f5DMXWMnA">Watch the app walkthrough →</a></p>
</div>

> **Ownership:** The application is proprietary to **The Betesh Group**. This repository is a portfolio showcase of my internship work; it does not distribute the application’s source code or grant rights to use, reproduce, or redistribute the company’s software, branding, or content.

## Built from a company brief

During my internship at **The Betesh Group**, I designed and built this product around the company’s stated needs. I translated their requirements into the app’s visual design, browsing structure, and content-management workflow, then implemented the mobile frontend and backend.

The result is a branded, image-led travel catalog: users explore destinations, discover hotels, restaurants, nightlife, and experiences, and browse the details of individual places. The interface pairs destination videos with full-screen imagery and a consistent visual identity.

## The product experience

- **Explore progressively:** move from a city to a category, a place, and its rooms, amenities, dining, or image galleries.
- **Find places geographically:** view city and place locations on a map, organized by service category.
- **Maintain the catalog in the app:** upload imagery, rename entries, reorder content, delete items, and attach place coordinates through the editing interface.

## Architecture · Navigation

The catalog follows a tree: each selection adds a key to the current path, and the next page displays its children.

```mermaid
flowchart LR
    City[Destination<br/>London · Tel Aviv] --> Category[Category<br/>Hotels · Restaurants<br/>Nightlife · Experiences]
    Category --> Place[Place<br/>Hotel or venue]
    Place --> Section[Details<br/>Rooms · Amenities<br/>Dining · Gallery]
    Section --> Item[Individual entry<br/>Room or experience]
    Item --> Images[Image catalog]
```

## Architecture · Content delivery

The Flutter app sends the current navigation path to Flask. PostgreSQL identifies the next level of content and its display order; the backend retrieves the corresponding images from Azure Blob Storage and packages them for the app.

```mermaid
flowchart TB
    subgraph Mobile[Flutter · mobile experience]
        Destination[Destination videos<br/>City selection]
        Browse[Category → place → details<br/>Current path represented as keys]
        Decode[Decode image archive<br/>Apply names and display order]
        Display[Image-led menus and galleries]
        Map[City map<br/>Places grouped by service]
        Destination --> Browse
        Decode --> Display
        Display -->|Open next level| Browse
    end

    subgraph API[Python / Flask · content API]
        Query[POST /get-images<br/>Resolve children of the current path]
        Bundle[Fetch matching images<br/>ZIP + Base64 + name and order maps]
        Locations[GET /get-map-data<br/>City center and place coordinates]
        Query --> Bundle
    end

    DB[(PostgreSQL<br/>Hierarchy keys · image URLs · order<br/>Cities and place locations)]
    Blob[(Azure Blob Storage<br/>Catalog images)]

    Browse -->|Navigation keys| Query
    Query -->|Metadata lookup| DB
    DB -->|Matching image records| Query
    Blob -->|Image bytes| Bundle
    Bundle -->|JSON response| Decode
    Map -->|City name| Locations
    Locations -->|Location lookup| DB
    Locations -->|Coordinates and service categories| Map
```

### A hierarchy driven by content

Underscore-separated upload filenames encode up to six levels of the catalog. For example, `london_hotels_the lanesborough_rooms_the royal suite.jpg` maps to a destination, category, hotel, room section, and room entry. The same path structure drives the backend’s child-content queries and the frontend’s navigation.

## Architecture · Content management

The editing interface connects catalog maintenance to the same content model used for browsing. Images and their metadata are stored separately, while ordering and location data remain in PostgreSQL.

```mermaid
flowchart LR
    Editor[In-app editing interface]
    Upload[Upload image<br/>Parse filename into hierarchy keys]
    Media[(Azure Blob Storage<br/>Image files)]
    Data[(PostgreSQL<br/>Content metadata and locations)]
    Modify[Rename or reorder<br/>Update catalog metadata]
    Delete[Delete entry<br/>Remove metadata and image blob]
    Locate[Set place coordinates<br/>Link city, service and place]

    Editor -->|POST /upload| Upload
    Upload -->|Store file| Media
    Upload -->|Store keys and blob URL| Data
    Editor -->|POST /edit or /reorder| Modify
    Modify --> Data
    Editor -->|POST /delete| Delete
    Delete --> Data
    Delete --> Media
    Editor -->|POST /insert-location| Locate
    Locate --> Data
```

The walkthrough documents the internship product. Its original deployment used company-managed cloud services; the video presents the experience independently of their current availability.
