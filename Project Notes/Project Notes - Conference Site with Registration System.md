#projects 

## Simple outline
This site will be three things:
1. A marketing vehicle to inform potential conference attendees of the conference
2. An informational resource with speaker lists, abstracts and programmes that conference attendees will want access too
3. A simple registration system allowing pre-registration of interest in the conference by potential attendees. Users must be able to provide an email address/name and answers to simple questions about their interest in the conference ie do they want to do a talk or a poster presentation

## Tech stack breakdown
- Next.js: React framework
- Typescript for static tests and typesafety
- tRPC and tanstack-query for posting data to the user database, pulling data for admin access only
- React-hook-form for the registration form, validate responses with zod
- Prisma ORM/Planetscale: Will run a mySQL instance on planetscale and manage relations with prisma
- Shadcn/UI for pre-made components: Use this to quickly get the bones of the site together and then take client feedback on customizing the look

## Features
- List of static informational pages can be pulled from the previous years site made available by the client
- Based on example set by previous site all pages outside the registration can be simple static and informational. Features like map integration on the location page may be nice includes but not strictly required
- Registration of interest page: Site users provide name, email, answers to survey questions about their interest in attending the conference, being emailed updates about the conference, presenting a poster/talk
- Footer should be included on every page route with the logo, sponsor logos and links to relevant data protection policies
- Including a dark/light mode toggle in the navbar/header. This is good for accessibility and also so that I do not have to look at a light mode website for hours while developing.

## Sticking points
This site should be a relatively straight forward project, with the initial scope agreed upon with the client only requiring one route of the website to actually interact with a backend. This may and in fact is somewhat likely to change in the future depending on how the development process goes but the specs can't change without a renegotiation.

The only part that is more complicated than static content communication is the <span style="font-weight: bold; font-style: italic; color: cyan;">Registration of interest form.</span>

This might need to be expanded into a full on registration form with payment handling, but that is not in the initial agreement, which only covers a registration of **INTEREST** form. The form will simply collect name, email and survey answers about interest in presenting a poster or talk. This info needs to be stored in a db, somehow exportable to the client for use in a mailing list. All procedures involved in the handling of this data need to abide by GDPR and we need to write and display policies to this effect.

## Notes for individual routes

### Home page
Banner at the top with background image - using stock image from unsplash for now pending client feedback. Built this out into a reusable hero component that takes a bg-image and potentially two buttons for linking. Planning to resuse this component on multiple pages as the flashy intro to the content.

### Location page
Will likely implement a slideshow component to display publicity images of the venue. Could do this using framer motion for the transition animations the same way that I did on the about page of the gallery site project. This would necessitate another dependancy but I think it would still be manageable.

### Programme/schedule route
Have the outline of the schedule for the event as an excel spreadsheet. Currently thinking about displaying it in an accordian component where each day of the conference is a seperate collapsible. Will likely mock up a version of this and run it by the client for aproval.

## Database schema/seeding notes
Using prisma as an ORM between the application and the planetscale instance of MySQL. My workflow for testing new versions of the schema is this:
1. Edit the schema
2. Make any necessary edits to the data in the seed script so that it conforms to the schema
3. Run: `npx prisma db push --force-reset` to clear all data from the current db instance and migrate the new schema. The db is now empty and any new data will be matched against the new schema
4. Run: `npx prisma db seed` to run the seed script and push new testing data