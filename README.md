# Whispra

<div align="center">
<img src="https://uploads-ssl.webflow.com/64140633eb1179c97ef1a7a9/64142dd6dba367e184b5b017_Qiscus.png" alt="Qisqus Logo" />
</div>

**Whispra** is a modern chat platform designed to deliver intuitive, lightweight, and secure real-time communication. Inspired by the word `whisper`, **Whispra** brings a sense of personal, fast, and efficient conversation—as if users are talking directly to each other, without any spatial limitations. This project is an internship requirement at `Qisqus`

## System Architecture Diagram

```bash
                             Internet
                                 │
                  ┌───────────────────────────────────┐
                  │           Frontend (Vue 3)        │
                  │───────────────────────────────────│
                  │  - UI Chat (Responsive Web/Mobile)│
                  │  - Axios (HTTP REST API)          │
                  │  - Socket.IO Client (Realtime)    │
                  └───────────────────────────────────┘
                                 │
                 (HTTPS + WebSocket connection)
                                 │
              ┌────────────────────────────────────────┐
              │         Backend (Node.js Express)      │
              │────────────────────────────────────────│
              │  - REST API: Auth, Chatroom, Message   │
              │  - Socket.IO: Realtime messaging       │
              │  - JWT Authentication                  │
              │  - File Uploads (Image/Video/PDF)      │
              │  - Store media links & metadata        │
              └────────────────────────────────────────┘
                                 │
                    (Database connection via Mongoose)
                                 │
             ┌───────────────────────────────────────────┐
             │        MongoDB Atlas (Cloud DB)            │
             │───────────────────────────────────────────│
             │ Collections:                              │
             │  - users                                  │
             │  - chat_rooms                             │
             │  - participants                           │
             │  - messages                               │
             │  - attachments (optional)                 │
             └───────────────────────────────────────────┘
                                 │
                       (Media file upload)
                                 │
           ┌──────────────────────────────────────────┐
           │   Cloud Storage / CDN (e.g., AWS S3)     │
           │──────────────────────────────────────────│
           │  - Store uploaded images, videos, PDFs   │
           │  - Generate access URLs for frontend     │
           └──────────────────────────────────────────┘

```

## Real-time Chat Process Flow

1. User login/register → Backend creates `JWT` token.

2. Frontend → connects to Socket.IO server using token.

3. User sends message (text/image/video/pdf) → sent via Socket.IO.

4. Backend saves message to MongoDB + broadcasts to all clients in the same room.

5. Other frontends receive new messages without refreshing (real-time).

---

## Database Structure

```bash
┌──────────────┐        ┌───────────────────┐
│   users      │        │   chat_rooms      │
│──────────────│        │───────────────────│
│ id (PK)      │◄──────┐│ id (PK)           │
│ name         │       ├│ name              │
│ email        │       ││ type ('group','private') │
│ avatar_url   │       ││ created_at        │
│ password_hash│       │└───────────────────┘
└──────────────┘       │
                       │
           ┌────────────┴────────────┐
           │                         │
┌───────────────────┐       ┌────────────────────┐
│ participants      │       │ messages           │
│───────────────────│       │────────────────────│
│ id (PK)           │       │ id (PK)            │
│ chat_room_id (FK) │       │ chat_room_id (FK)  │
│ user_id (FK)      │       │ sender_id (FK→user)│
│ joined_at         │       │ message_type       │
│ role (member/admin)│      │ content            │
└───────────────────┘       │ file_url (nullable)│
                            │ created_at         │
                            └────────────────────┘
```

---

Example
`users`

```json
{
  "_id": "u001",
  "name": "Fadhli",
  "email": "fadhli@example.com",
  "avatarUrl": "https://cdn.whispra.io/avatar/u001.png"
}
```

`chat_rooms`

```json
{
  "_id": "c001",
  "name": "AI Discussion",
  "type": "group",
  "createdAt": "2025-10-29T08:00:00Z"
}
```

<p>If you have any questions about the program, you can contact me via email or Instagram <a href="https://www.instagram.com/fdhliakbar/" target="_blank">@fdhliakbar</a> Thanks.</p>

<img src="https://i.pinimg.com/originals/90/70/32/9070324cdfc07c68d60eed0c39e77573.gif" alt="Banner Coding"/>
