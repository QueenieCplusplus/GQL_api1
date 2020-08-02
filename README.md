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

         /app.js
        
         /routes/
               /index.js
               
         /views/
               /index.pug
               /layout.pug
               
         /public/
         
         /package.json
         
         /bin/
         /bin/www

2. apollo server 

       $npm install apollo-server-express

3. start app

     node app.js

      http://localhost:3000/
      
      http://localhost:4000/
      
4. apollo-server-express

            const { ApolloServer } = require('apollo-server-express'); // server
           
            var app = express(); // apps instance
            
            const server = new ApolloServer({
              typeDefs: 需要建立 GQL Schema, 
              resolvers: 需要建立 GQL Resolver,
              engine: true,
              context: async () => ({
               mongo: await connectMongo()
              }) // context 第二參數可加上 req 如 user: req.user

            });
            
            // 使用中介軟體將 server 安裝在 app 同一路徑上
            // 引數為 app 輸出則為 server註冊者
            server.applyMiddleware({app});
            //server.applyMiddleware({app, path: '/gql'}); 
            
            app.listen({
                  port: 4000
            }, () => {
                  console.log('Apollo Server on http://localhost:4000');
            });
            
            /*app.listen({
                  port: 4000
            }, () => {
                  console.log('Apollo Server on http://localhost:' + 4000 + '/gql');
            });*/
            
  5. MongoDB
  
            const morgan = require('morgan');
            const mongoose = require('mongoose');

            // morgan 
            app.use(morgan('dev'));

            // url path config
            // process.env
            let Mongo_PW = 'root';
            let DATABASE_NAME = 'katesdb'
            let port = 27017;
            let url = 'mongodb://root'+ Mongo_PW + '@localhost:'+ port +'/' + DATABASE_NAME + '?authSource=admin';
            console.log('DB name ' + DATABASE_NAME);

            // mongoose
            mongoose.connect(url, {
              useUnifiedTopology: true,
              useNewUrlParser: true
            }).then(()=>{
              console.log('DB is connected.')
            }).catch( err =>{
              console.log(err);
            });
            
  6. DB_HOST URL ＆ context

Mongoose.Connect(url,...) 來讓 app 和 DB 建立連線，爾後將此 url 連結加入 context。

      (1) npm install mongo

      (2) 修改 Apollo Server config file, index.js。

      (3) 確定 mongodb server 和本地端連線成功，開始服務。

      (4) 從系統內環境變數讀取（拉出）DB_HOST 資料庫主機資訊。

         url 分成mongoDB本地端或是mlab沙河兩種：

         * DB_HOST = mogodb://localhost://27017/<db_name>

         * DB_HOST = mogodb://<dbuser>:<dbpassword>@5555.mlab.com:5555/<db_name>

      (5) 確定 3 步驟成功後，建立一 context 物件，再次啟動服務，並且使用 dotenv 套件 load DB_HOST URL。

# CodeBase for MongoDB + Express App + Apollo-Server-Express

      npm install mongodb
      npm install dotenv

      const { MongoClient } = require('mogodb')
      require('dotenv').config()
      import {ApolloServer} from 'apollo-server-express'

      // 建立 async func

      async function start(){

         const app = express();

         const MONGO_DB = process.env.DB_HOST //本地端 mongodb 資料庫的主機位置

         const client - await MongoClient.connect(

             MONGO_DB,
             {useNewUrlParser: true}

         )
         
       }
       
       const db = client.db() // 在本機本地建立資料庫實例
       
       const context = {db} // 全域變數使用 db 實例環境變數的資料庫主機
       
       const server = new ApolloServer({
       
            typDefs = 宣告一schema_gql,
            resolvers = 宣告一解析函數,
            context // 此全域變數裝載資料庫主機資訊
       
       })
       
       server.applyMiddleware({app})
       
       app.get('/',(req, res)=> res.end('hi there'))
       
       app.get('/playground', expressPlayground(
         {endpoint:'/gql'}
       ))
       
       app.listen({port: 4000}, ()=>
       
           console.log(
           
            'gql sever is running at http://localhost:4000${server.graphqlPath}'
           
           )
       
       )
