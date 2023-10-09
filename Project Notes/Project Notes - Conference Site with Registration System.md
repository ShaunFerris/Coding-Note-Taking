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

# Features
- List of static informational pages can be pulled from the previous years site made available by the client
- Based on example set by previous site all pages outside the registration can be simple static and informational. Features like map integration on the location page may be nice includes but not strictly required
- Registration of interest page: Site users provide name, email, answers to survey questions about their interest in attending the conference, being emailed updates about the conference, presenting a poster/talk etc