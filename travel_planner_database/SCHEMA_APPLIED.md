# Schema applied (manual via psql -c, one statement at a time)

This project’s PostgreSQL schema + seed data was applied **directly** to the running DB using the connection command from `db_connection.txt`:

- `psql postgresql://appuser:dbuser123@localhost:5000/myapp -c "..."`

## Extensions
- Enabled: `pgcrypto` (for `gen_random_uuid()`)

## Tables created
- `trips`
- `destinations`
- `itinerary_items`
- `notes`
- `reminders`

## UUID strategy
All tables use:
- `id uuid PRIMARY KEY DEFAULT gen_random_uuid()`

## Foreign keys
- `itinerary_items.trip_id -> trips(id) ON DELETE CASCADE`
- `itinerary_items.destination_id -> destinations(id) ON DELETE SET NULL`
- `notes.trip_id -> trips(id) ON DELETE CASCADE`
- `reminders.trip_id -> trips(id) ON DELETE CASCADE`

## Indexes created
- `trips(name)`
- `destinations(name)`, `destinations(country)`
- `itinerary_items(trip_id, day)`, `itinerary_items(destination_id)`
- `notes(trip_id, created_at DESC)`
- `reminders(trip_id, remind_at DESC)`

## Seed data inserted
Destinations (fixed UUIDs):
- Paris: `11111111-1111-1111-1111-111111111111`
- Tokyo: `22222222-2222-2222-2222-222222222222`
- New York: `33333333-3333-3333-3333-333333333333`

Trip (fixed UUID):
- `aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa` (Sample Trip: Paris Weekend)

Related:
- 3 itinerary items
- 1 note
- 1 reminder

## Verification query used
A simple `UNION ALL` count across all tables returned:
- trips=1, destinations=3, itinerary_items=3, notes=1, reminders=1
