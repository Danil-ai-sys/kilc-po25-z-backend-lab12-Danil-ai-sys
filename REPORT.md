# Report, lab 12

Fill this in as you go. Write in your own words.

**Name: Danil Sobar**
**Group: PO 25-Z**
**Date: 3.11.26**

## What works

Tick only what works. "Almost works" is not ticked.

- [x] M1: `Message` and the four relationships
- [x] M2: `author` in `MessageOut`, the list, a new message
- [x] M3: one message, PATCH, DELETE
- [x] Bruno: 26 of 26 green
- [x] `pytest`: 14 passed, 5 failed

## The SQL behind an attribute

With the server running, open `GET /rooms/1/messages` in `/docs` after the Bruno run. In the console, find the `SELECT` that reads from `users`. Copy it:

```
2026-11-03 14:35:41,085 INFO sqlalchemy.engine.Engine BEGIN (implicit)
2026-11-03 14:35:41,088 INFO sqlalchemy.engine.Engine SELECT rooms.id, rooms.title 
FROM rooms 
WHERE rooms.id = ?
2026-11-03 14:35:41,088 INFO sqlalchemy.engine.Engine [generated in 0.00030s] (1,)
2026-11-03 14:35:41,089 INFO sqlalchemy.engine.Engine ROLLBACK
```

## Lab 08 and lab 12

1. In lab 08, `POST /rooms/1/messages` with `{"author": "Nobody", "text": "hi"}` answered 201. What does it answer now, and which part of the database design is the reason?

It answers 404 user_not_found. The reason is the foreign key
   messages.user_id -> users.id: a message must belong to an existing
   user, and "Nobody" is not in the users table. In lab 08 the author
   was just a string, so any name was accepted.

2. In `list_messages`, the filter by author needs `join(Message.author)`. Why is `where(Message.author == author)` with a name not enough?

The messages table stores only user_id, not the name. Message.author
   is a relationship to a User object, so it cannot be compared with a
   string. The name is in users.name, so the query needs a JOIN
   (join(Message.author)) and then where(User.name == author).