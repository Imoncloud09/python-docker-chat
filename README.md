# Building a Simple Chat App using Python, Flask and Docker

Showcases how to impliment chat app in Python (Flask), Socket.IO and Redis. This example uses **pub/sub** feature combined with web-sockets for implementing the message communication between client and server.


## Technical Stacks

- Frontend - _React_, _Socket.IO_
- Backend - _Flask_, _Redis_



## How to run it locally?

#### Copy `.env.sample` to create `.env`. And provide the values for environment variables

    - REDIS_ENDPOINT_URI: Redis server URI
    - REDIS_PASSWORD: Password to the server

#### Run frontend

```sh
cd client
yarn install
yarn start
```

#### Run backend

Run with venv:

```sh
python app.py
```

## Try it out

#### Deploy to Cohesive

Click this icon to deploy the app to Cohesive

<p>
    <a href="https://docs.cohesive.so/learn-cohesive/how-cohesive-works/cohesive-installation-using-cli" target="_blank">
        <img src="https://github.com/collabnix/python-docker-chat/blob/master/cohesive_logo.png" alt="Deploy to Cohesive" />
    </a>
</p>


