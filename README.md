# 🎓 Pair-fainder Application Development

## 🎯 Objective

The goal of this project, developed using **Next.js** and **TypeScript**, is to build a pair-learning platform enabling real-time collaboration. Users can securely sign in, create learning sessions, and use features like shared whiteboards, code collaboration, screen sharing, and participant management. This platform fosters interactive learning by pairing users for shared knowledge and skill development.

---

## 🚀 Application Features

### 🔐 **Authentication**
- Secure sign-in via social networks with appropriate access permissions.

### 🏠 **Room Creation**
- Create rooms easily by configuring camera and microphone settings before joining.

### 🛠️ **Room Controls**
- Full control over learning sessions, including:
  - 🎉 Emoji reactions
  - 🖥️ Screen sharing
  - 🔊 Sound management (mute/unmute)
  - 🔲 Grid layout
  - 👥 Participant list view
  - ⚙️ Individual participant management

### 🚪 **Leave Room**
- Participants can leave at any time, and room creators can end the session for everyone.

### ✏️ **Edit & Delete Room**
- Room owners can edit or delete their rooms whenever they wish.

### 🏡 **Personal Room**
- Users can create and share personal rooms with others.

### 🔒 **Real-Time Secure Functionality**
- All interactions are secure and happen in real time, ensuring data privacy and integrity.

### 🔍 **Search for Room**
- Search pair learning rooms using tags.

### 📱 **Responsive Design**
- Optimized for all devices with responsive design principles to adapt to various screen sizes.

---

## 🛠️ Quick Start

Follow these steps to set up the project locally on your machine.

### Prerequisites
Make sure you have the following installed:

- **[Git](https://git-scm.com/)**
- **[Node.js](https://nodejs.org/en)**
- **[npm](https://www.npmjs.com/) (Node Package Manager)**

### 📂 Cloning the Repository
```bash
git clone https://github.com/ommlet58/pair-finder.git
cd pair-finder
```
🛠️ Installation
Install the project dependencies using npm:
```bash
npm install
```
⚙️ Environment Configuration

Create a .env file in the root of your project and add the following content:
```bash
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"
NEXTAUTH_SECRET="your-secret-key"
NEXT_PUBLIC_GET_STREAM_API_KEY="your-getstream-api-key"
GET_STREAM_SECRET_KEY="your-getstream-secret-key"
DATABASE_URL="your-database-url"
NEXTAUTH_URL="http://localhost:3000"
```
Replace the placeholder values with your actual credentials for Google Auth and GetStream.

⏩ Keeping the Project Updated
To ensure you have the latest features and bug fixes, update the project by running:
```bash
npm i next@latest
```
🔍 Viewing the Project
Once the setup is complete, run the project and open http://localhost:3000 in your browser to view it.

🛠️ Development Steps
1. Shadcn UI Integration
To initialize Shadcn UI, use the following command:
```bash
npx shadcn-ui@latest init
```
2. Drizzle ORM with PostgreSQL
Install [Drizzle ORM](https://orm.drizzle.team/docs/get-started-postgresql#postgresjs) for database interaction:
```bash
npm i drizzle-orm postgres
npm i -D drizzle-kit
```
Create a folder src/db and add a file index.ts with this code:
```bash
import { drizzle } from 'drizzle-orm/postgres-js';
import { migrate } from 'drizzle-orm/postgres-js/migrator';
import postgres from 'postgres';

const migrationClient = postgres("postgres://postgres:password@localhost:5432/db", { max: 1 });
migrate(drizzle(migrationClient), ...);

const queryClient = postgres("postgres://postgres:password@localhost:5432/db");
const db = drizzle(queryClient);
await db.select().from(...);

```

Add DATABASE_URL in your .env file:
```bash
DATABASE_URL="postgres://postgres:yourpassword@localhost:5432/yourdb"
```
3. Docker Setup
Install [Docker Compose](https://docs.docker.com/compose/#:~:text=Compose%20simplifies%20the%20control%20of%20your) and create a docker-compose.yml file with the following content:
```bash
version: "3.9"
services:
  pair-finder-db:
    image: postgres
    restart: always
    container_name: pair-finder-db
    ports:
      - 5432:5432
    environment:
      POSTGRES_PASSWORD: root
      PGDATA: /data/postgres
    volumes:
      - postgres:/data/postgres

volumes:
  postgres:
```
Run the command:
```bash
docker-compose up
```
4. Database Schema
Create a schema.ts file in the src/db directory, and a drizzle.config.ts file at the project root:
```bash
import { defineConfig } from "drizzle-kit";
export default defineConfig({
  schema: "./src/db/schema.ts",
  driver: "pg",
  dbCredentials: {
    connectionString: process.env.DATABASE_URL!,
  },
  verbose: true,
  strict: true,
});
```
In package.json, add the script:
```bash
"db:push": "drizzle-kit push:pg --config=drizzle.config.ts"
```
Run the migration with:
```bash
npm run db:push
```
5. NextAuth Setup
Install NextAuth for authentication:
```bash
npm install next-auth
```
Create an API route for authentication:
```bash
import NextAuth from "next-auth";
import GoogleProvider from "next-auth/providers/google";

export const authOptions = {
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    }),
  ],
};

export default NextAuth(authOptions);
```
Set up OAuth credentials in [Google Cloud Console.](https://console.cloud.google.com/apis/credentials/oauthclient)

📹 GetStream Video SDK Integration
![Capture d'écran 2024-09-06 105944](https://github.com/user-attachments/assets/b3930168-f45c-441a-9168-b87e9e59e807)
![Capture d'écran 2024-09-06 105757](https://github.com/user-attachments/assets/912dcbc5-b581-40b4-bc51-e4a05099838c)
![Capture d'écran 2024-09-06 105850](https://github.com/user-attachments/assets/c80444f0-ee59-435d-bdc4-5f2bda013eaf)
![Capture d'écran 2024-09-06 105944](https://github.com/user-attachments/assets/cde08f50-48da-44ca-941b-89d45b8b3ead)

1. Signing Up
Create a GetStream account:  [GetStream.io](https://getstream.io/)
Obtain the following API keys:
Public API Key
Secret Key
Add them to the .env file:
```bash
NEXT_PUBLIC_GET_STREAM_API_KEY="your-public-api-key"
GET_STREAM_SECRET_KEY="your-secret-key"
```
2. Installing the SDK
Install the client-side video SDK:
```bash
npm install @stream-io/video-react-sdk
```
2. Installing the SDK
Install the client-side video SDK:
```bash
npm install @stream-io/video-react-sdk
```
Install the server-side SDK:
```bash
npm install @stream-io/node-sdk
```




