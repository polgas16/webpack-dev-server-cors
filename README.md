```
const wpConfig = require('./webpack.config.js');
const config = require('config');
const path = require('path');
const fs = require('fs');

wpConfig.devServer = {
    setupMiddlewares: (middlewares, devServer) => {
        if (!devServer) {
            throw new Error('webpack-dev-server is not defined');
        }
        // defined CORS before all route definitions
        var cors = require("cors");
        devServer.app.use(cors({ origin: '*' }));

        return middlewares;
      },
    headers: {
        "Access-Control-Allow-Origin": "*",
        "Access-Control-Allow-Headers": "*"
    },
    static: [path.join(__dirname, 'public'), path.join(__dirname, 'src')],
    server: {
        type: 'https',
        options: {
            key: fs.readFileSync('key.pem'),
            cert: fs.readFileSync('cert.pem'),
            spdy: {
                protocols: ['http/1.1']
            }
        },
    },
    hot: true,
    client: {
        logging: 'info'
    },
    compress: true,
    port: config.server.port
}

wpConfig.mode = 'development'

module.exports = wpConfig;
```
