# GQL_api
Backend using express framework

ref doc:

https://medium.com/@charming_rust_oyster_221/node-js-express-安裝設置與簡單實作-5920e1d70d9d

npm install:

      express --save
      nodemon -g
      
1. express app generator:

https://expressjs.com/zh-tw/starter/generator.html

    $npm install express-generator -g
    
    $npm install nodemon -g

    $express --view=pug kapp

         create : kapp/
         create : kapp/public/
         create : kapp/public/javascripts/
         create : kapp/public/images/
         create : kapp/public/stylesheets/
         create : kapp/public/stylesheets/style.css
         create : kapp/routes/
         create : kapp/routes/index.js
         create : kapp/routes/users.js
         create : kapp/views/
         create : kapp/views/error.pug
         create : kapp/views/index.pug
         create : kapp/views/layout.pug
         create : kapp/app.js
         create : kapp/package.json
         create : kapp/bin/
         create : kapp/bin/www

2. apollo server 

       $npm install apollo-server-express

3. start app

     node app.js

      http://localhost:3000/
      
      http://localhost:4000/
      
4. apollo-server-express

            const { ApolloServer } = require('apollo-server-express'); // server
            const morgan = require('morgan');
            const mongoose = require('mongoose');
            // matter with apollo's typeDefs, 需要額外建立
            const GQLSchema = require('./gql/schema/index');
            // matter with apollo's resolver, 需要額外建立
            const GQLResolver = require('/gql/resolver/Resolver');

            var indexRouter = require('./routes/index');
            var usersRouter = require('./routes/users');

            var app = express(); // apps instance
            const server = new ApolloServer({
              typeDefs: GQLSchema, 
              resolvers: GQLResolver,
              engine: true,
              context: async () => ({
               mongo: await connectMongo()
              }) // context 第二參數可加上 req 如 user: req.user

            });

// 使用中介軟體將 server 安裝在 app 同一路徑上
// 引數為 app 輸出則為 server註冊者

