# StoreMyFiles

StoreMyFiles is a full-stack cloud storage application. Users can create folders, upload files, preview supported files, rename and move items, share files, and restore deleted items from the trash.

The frontend and backend are intentionally separated. The React client is deployed to GitHub Pages, while the Express API can run independently on a Node.js hosting platform.

## Demo

[Open the StoreMyFiles demo](https://gauravkal006.github.io/StoreMyFiles/)

## Features

- User registration, login, logout, and protected routes
- File upload with upload progress
- Folder creation and navigation
- File preview, rename, move, and delete actions
- Trash and restore functionality
- Shareable file links
- Search and sort controls
- S3-compatible object storage

## Tech stack

### Frontend

- React
- Vite
- React Router
- Axios
- Tailwind CSS

### Backend

- Node.js
- Express
- PostgreSQL through Neon
- S3-compatible object storage
- JWT authentication

## Architecture

```text
Browser
	|
	| GitHub Pages
	v
React/Vite client  --->  Express API  --->  Neon PostgreSQL
															|
															v
											 S3-compatible storage
```

## Project structure

```text
client/
	src/                 React application
	public/              Static frontend assets
	vite.config.js       Vite and GitHub Pages configuration

server/
	config/              Database and storage configuration
	controllers/         API request handlers
	middleware/          Authentication and upload middleware
	routes/              API route definitions
	services/            Storage services
	server.js            Express application entry point

.github/workflows/
	deploy-client.yml    GitHub Pages deployment workflow
```

## Requirements

- Node.js 20 or newer
- A Neon PostgreSQL database
- An S3-compatible storage bucket

## Local development

Install dependencies in both applications:

```bash
cd client
npm install

cd ../server
npm install
```

Create local environment files from the included examples:

```text
client/.env.example  ->  client/.env
server/.env.example  ->  server/.env
```

Set the database, authentication, CORS, and storage values in `server/.env`. The client uses `http://localhost:3000` by default; change `VITE_BASE_URL` in `client/.env` when the API runs at another address.

Start the API in one terminal:

```bash
cd server
npm run dev
```

Start the frontend in a second terminal:

```bash
cd client
npm run dev
```

Open `http://localhost:5173` in a browser.

## Available scripts

Run these commands from the corresponding directory:

| Directory | Command           | Purpose                               |
| --------- | ----------------- | ------------------------------------- |
| `client`  | `npm run dev`     | Start the Vite development server     |
| `client`  | `npm run build`   | Create a production frontend build    |
| `client`  | `npm run preview` | Preview the production frontend build |
| `client`  | `npm run lint`    | Run frontend lint checks              |
| `server`  | `npm run dev`     | Start the API with automatic reload   |
| `server`  | `npm start`       | Start the API for production          |

## Environment configuration

Use [client/.env.example](client/.env.example) and [server/.env.example](server/.env.example) as references. Do not commit `.env` files or real credentials.

The most important production settings are:

- `VITE_BASE_URL`: public URL of the deployed backend
- `DATABASE_URL`: Neon PostgreSQL connection string
- `JWT_SECRET`: long random authentication secret
- `ORIGINS`: exact public frontend origin
- `AWS_REGION`, `AWS_ENDPOINT_URL_S3`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_S3_BUCKET`: storage configuration

## Deployment

### Frontend: GitHub Pages

The workflow in [.github/workflows/deploy-client.yml](.github/workflows/deploy-client.yml) builds the `client` directory and publishes it to GitHub Pages when changes are pushed to `main`.

Configure the repository with:

1. **Settings > Pages > Source:** GitHub Actions
2. Repository Actions variable `VITE_BASE_URL` set to the public backend URL
3. A push to `main` or a manual workflow run

The Vite base path and React Router fallback are configured for project pages automatically.

### Backend: independent Node.js hosting

The backend is a standard Express application and can be deployed separately to Koyeb, Render, Railway, or another Node.js service. Use the `server` directory as the service root, `npm install` as the install command, and `npm start` as the production start command. Configure the variables from `server/.env.example` in the hosting provider rather than committing them.

After deployment, set the frontend `VITE_BASE_URL` to the API URL and set the backend `ORIGINS` to the GitHub Pages URL.

## Security notes

- Keep all credentials in hosting-provider environment variables.
- Use a unique, long `JWT_SECRET` in production.
- Restrict `ORIGINS` to trusted frontend domains.
- Do not commit local `.env` files, access keys, or database URLs.

## License

This project is released under the MIT License.
