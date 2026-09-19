# Matchlist
https://github.com/user-attachments/assets/bdd89a19-b703-4090-b130-3b6bd7266e3f
> A multilingual CS2 esports match tracker built to help players follow their favorite teams and never miss their next match.

---

## Features

### Personalized Match Feed

Users can follow their favorite CS2 teams and get a feed containing matches relevant to them.

- Upcoming matches
- Live matches
- Recent results
- Team-based filtering
- Match status tracking
- Match spotlight

###  Match Notifications

Users can choose which events they want to be notified about.

Supported notification events include:

- Match scheduled
- Match started
- Match result
- Match time changed
- Match cancelled
- Requested team became available

Notifications are delivered through the browser using Web Push.

### Internationalization

Matchlist was designed for an international audience.

Supported languages:

- 🇬🇧 English
- 🇧🇷 Português (Brasil)
- 🇵🇹 Português (Portugal)
- 🇪🇸 Español
- 🇫🇷 Français
- 🇷🇺 Русский
- 🇩🇪 Deutsch
- 🇵🇱 Polski
- 🇺🇦 Українська


The application keeps language and timezone preferences independent, allowing users to configure how the application should display information.

### Timezone Support

Match times are displayed according to the user's selected timezone.

For example, a match scheduled for:

`20:00 UTC`

can be displayed as:

`17:00` in São Paulo

or:

`22:00` in Madrid.

Match timestamps are stored and transmitted independently from their display format, allowing the frontend to perform the appropriate timezone conversion.

### Team Following

Users can search for teams and manage their followed teams from the application.

Following a team automatically personalizes the user's match feed and notification system.

### Authentication

The application includes user authentication with:

- Registration
- Login
- Logout
- Password reset
- Password confirmation
- Session management

### Responsive UI

The interface was designed to work across:

- Desktop
- Tablet
- Mobile

---

# Architecture

Matchlist consists of several independent parts working together:

```text
                         ┌─────────────────┐
                         │     Draft5      │
                         │   Match Data    │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │    Playwright   │
                         │     Scraper     │
                         └────────┬────────┘
                                  │
                                  │ API
                                  ▼
                         ┌─────────────────┐
                         │     Laravel     │
                         │     Backend     │
                         └───────┬─┬───────┘
                                 │ │
                    ┌────────────┘ └─────────────┐
                    ▼                            ▼
             ┌─────────────┐              ┌─────────────┐
             │ PostgreSQL  │              │    Redis     │
             │   Database  │              │    Queue     │
             └─────────────┘              └──────┬──────┘
                                                  │
                                                  ▼
                                           ┌─────────────┐
                                           │  Web Push   │
                                           │ Notifications│
                                           └──────┬──────┘
                                                  │
                                                  ▼
                                           ┌─────────────┐
                                           │    User     │
                                           │   Browser   │
                                           └─────────────┘
```

#  Tech Stack
## Frontend
- Vue 3
- TypeScript
- Inertia.js
- Tailwind CSS
- Vite
- Pinia
- Vue Router
## Backend
- PHP
- Laravel
- Laravel Breeze
- Laravel Sanctum
- Laravel Queues
## Infrastructure
- Nginx
- Redis
- Linux
## Data Collection
- Node.js
- Playwright
## Notifications
- Web Push
- VAPID

# Data Flow

Matchlist continuously collects match information from its data source and synchronizes it with the application.

```text
External match data
        │
        ▼
     Scraper
        │
        ▼
 Laravel API
        │
        ▼
        DB
        │
        ├───────────────┐
        ▼               ▼
   Match Feed       Notification
                       Service
                           │
                           ▼
                         Redis
                           │
                           ▼
                      Web Push
                           │
                           ▼
                        Browser
```



# Notification Architecture


```text
Match Event
     │
     ▼
Notification Service
     │
     ├──────────────► Database
     │                 │
     │                 ▼
     │           In-app notification
     │
     └──────────────► Queue
                       │
                       ▼
                    Web Push
                       │
                       ▼
                    Browser
```                


# Localization & Timezones

Localization is handled independently from timezone conversion.

## Locale

- Interface language
- Notification text
- Date formatting
- Number formatting

## Timezone
- Match dates
- Match times
- Rescheduled match times
- Notification timestamps
