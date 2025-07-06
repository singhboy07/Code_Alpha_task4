// index.html

<!DOCTYPE html>

<html>

<head><title>Video Chat</title></head>

<body>

 <h1>Video Chat</h1>

 <video id="local" autoplay muted></video>

 <script src="/socket.io/socket.io.js"></script>

 <script src="client.js"></script>

</body>

</html>

// client.js

const socket = io();

navigator.mediaDevices.getUserMedia({ video: true, audio: true })

 .then(stream => {

 document.getElementById('local').srcObject = stream;

 socket.emit('join-call');

 });

// server.js

const express = require('express');

const http = require('http');

const socketIo = require('socket.io');

const app = express();

const server = http.createServer(app);

const io = socketIo(server);

app.use(express.static(__dirname + '/public'));

io.on('connection', socket => {

 console.log('User connected');

 socket.on('join-call', () => {

 console.log('User joined call');

 });

});

server.listen(3003, () => console.log('RTC server on port 3003'));
