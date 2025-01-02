  # boiler-plate-code
Backend:

     mkdir server-name  
     cd  server-name 
    npm init -y
          npm i express cors mongodb dotenv
    code . 




                                 const express=require('express')
                                const app=express()
                              const cors=require('cors')
                                const port=process.env.PORT|| 5000;
                                require('dotenv').config()
                                app.use(cors());
                                app.use(express.json())




                              app.get('/',(req,res)=>{
                             res.send('user is serving')
                })


    // Create a MongoClient with a MongoClientOptions object to set the Stable API version
    const client = new MongoClient(uri, {
     serverApi: {
    version: ServerApiVersion.v1,
    strict: true,
    deprecationErrors: true,
     }
    });




 
    async function run() {
 
    try {
   


    const userCollection= client.db('userdb').collection('users');
   



    // Connect the client to the server (optional starting in v4.7)
    await client.connect();
    //
    
    Send a ping to confirm a successful connection
    await client.db("admin").command({ ping: 1 });
    console.log("Pinged your deployment. You successfully connected to MongoDB!");


    app.post('/user',async(req,res)=>{
    const user=req.body;
    console.log('poster', user)
    const result= await userCollection.insertOne(user);
    res.send(result)
    })
    app.get('/user',async(req,res)=>{
        const user=req.body;
        const result= await userCollection.find(user).toArray()
        res.send(result)
    })


    app.get('/user/:id',async(req,res)=>{
        const id=req.params.id;
        const query={_id: new ObjectId(id)}
        const result= await userCollection.findOne(query);
        res.send(result)
    })


    app.delete('/user/:id',async(req,res)=>{
        const id=req.params.id;
        const query={_id: new ObjectId(id)}
        const result=await userCollection.deleteOne(query)
        res.send(result)
    })
    } finally {
    // Ensures that the client will close when you finish/error
    // await client.close();
    }
     }
    run().catch(console.dir);

    app.listen(port, (res,req)=>{
    console.log(`port is running ,${port}`)
    })

