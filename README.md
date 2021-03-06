# phaserquest

Phaser Quest is a reproduction of Mozilla's [Browserquest](http://browserquest.mozilla.org/) using the following tools:
- The [Phaser](https://phaser.io/) framework for the client 
- [Socket.io](http://socket.io/) and [Node.js](https://nodejs.org/en/) for the server and client-server communication

## Quick tour of the code

### Client

The game canvas and the game states are created in `js/client/main.js`. The `Home` state is started first, and will display the home page
of the game. The `Game` state is started upon calling `startGame()` from the `Home` state. 

`js/client/game.js` contains the  `Game` object, which corresponds to the `Game` state and contains the bulk of the client code. 
`Game.init()` is automatically called first by Phaser, to initialize a few variables. `Game.preload()` is then called, to load the
assets that haven't been loaded in the `Home` state. When all assets are loaded, Phaser calls `Game.create()` where the basics of the game
are set up. At the end of `Game.create()`, a call is made to `Client.requestData()` (from `js/client/client.js`) to request initialization
data from the server. Upon reception of this data, `Game.initWorld()` is called, which finishes starting the game. The main update loop of the client is `Game.update()`. 

### Server and updates

`server.js` is the Node.js server that supports the game. Most of the server-side game logic however is located in `js/server/GameServer.js`. Every 200ms, `GameServer.updatePlayers()` is called, and sends updates to all clients (if there are updates to send, as determined by the curstom interest management system). Client-side, these updates are processed by `Game.updateWorld()` and `Game.updateSelf()`. 

The code used for the custom binary protocol for the exchange of update packets can be found in `js/client/Decoder.js`, `js/server/Encoder.js` and `js/CODec.js`.

## Installing and running the game

For the client, everything is included in the code (`phaser.js`, `easystar.min.js`, ...). You will need [npm](https://www.npmjs.com/) to install the Node.js packages required for the server. To run the server, you'll need to have Node.js installed, as well as [MongoDB](https://www.mongodb.com/).

Clone the repository. Inside the newly created directory, run `npm install` to install the Node.js packages listed in `package.json`. Then run `node server.js` to start the server. 
By default, it'll listen to connections on port `8081`; you can change that behaviour by using the `-p` flag (e.g. `node server.js -p 80`). 
By default, it'll attempt to connect to MongoDB on port `27017`; you can change that behaviour by using the `--mongoPort` flag (e.g. `node server.js --mongoPort 25000`).

## Further documentation

I have written and will keep writing articles about some development aspects of the game. The full list of existing articles is available [here](http://www.dynetisgames.com/tag/phaser-quest/).

Here is the detail of the topics covered so far:
- [Clients synchronization](http://www.dynetisgames.com/2017/03/19/client-updates-phaser-quest/)
- [Latency estimation](http://www.dynetisgames.com/2017/03/19/latency-estimation-phaser-quest/)
