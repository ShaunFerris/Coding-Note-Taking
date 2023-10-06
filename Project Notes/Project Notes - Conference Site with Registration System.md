#projects 

## Simple outline
This site will be three things:
1. A marketing vehicle to inform potential conference attendees of the conference
2. An informational resource with speaker lists, abstracts and programmes that conference attendees will want access too
3. A simple registration system allowing pre-registration of interest in the conference by potential attendees. Users must be able to provide an email address/name and answers to simple questions about their interest in the conference ie do they want to do a talk or a poster presentation

## Tech stack breakdown
- Next.js: React framework
- Typescript for static tests and typesafety
- tRPC? May be considered overkill for this project
- Maybe react-hook-form for input validation on the registration of interest form
- Prisma ORM/Planetscale: Will run a mySQL instance on planetscale and manage relations with prisma