## Getting Started

Get started by running the project locally, simply follow these steps:

1. Clone/download the repo

2. Grab the PocketBase executable for your OS from: https://pocketbase.io/docs/ and drop it at the root of the `pb` folder.

3. Start the PocketBase server

```bash
cd pb
./pocketbase serve
```

4. Start the Vite server

```bash
cd ui
npm install && npm run dev
```

## Customization

App name, logo, and colors can be customized using the `customize.json` file.

## Deployment

One neat thing about PocketBase is that it can also serve our static frontend assets. to do that simply follow these steps:

1. Add the server URL where your PocketBase instance is hosted to `VITE_PROD_PB_URL` in the `.env` file

2. Build a production-ready bundle

```bash
cd ui
npm run build
```

3. Copy the contents of the `dist` folder over to `pb_public`

### Using Docker

You can use the following Dockerfile to automate the steps above:

```dockerfile
FROM node:20.11.1-buster

WORKDIR /

RUN git clone https://github.com/galaimarketing/tunsigma.git

ARG PB_VERSION=0.21.3
ADD https://github.com/pocketbase/pocketbase/releases/download/v${PB_VERSION}/pocketbase_${PB_VERSION}_linux_amd64.zip /tmp/pb.zip
RUN unzip /tmp/pb.zip -d /tunsigma/pb

WORKDIR /eCourse/ui

RUN sed -i 's/^VITE_PROD_PB_URL=.*/VITE_PROD_PB_URL=http:\/\/127.0.0.1:8090/' .env
RUN npm install
RUN npm run build
RUN mv dist/* /tunsigma/pb/pb_public

EXPOSE 8090

CMD ["/tunsigma/pb/pocketbase", "serve", "--http=0.0.0.0:8090"]
```

## Feedback & Suggestions

Feel free to open an issue/PR if you find any bugs or want to request new features.

## License

Licensed under the MIT License, Copyright © 2024
