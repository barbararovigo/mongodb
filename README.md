# mongodb
Exercício 1 - Aquecendo com os pets  

1.Adicione outro Peixe e um Hamster com nome Frodo       
`db.pets.insert({name:"Frodo", species:"Peixe"})`  
`db.pets.insert({name:"Frodo", species:"Hamster"})`  

2.Faça uma contagem dos pets na coleção  
`db.pets.count()`   

3.Retorne apenas um elemento o método prático possível  
`db.pets.findOne()`  

4.Identifique o ID para o Gato Kilha.  
`db.pets.find({"name":"Kilha"})`  

5.Faça uma busca pelo ID e traga o Hamster Mike  
`db.pets.find({"_id":ObjectId("5e8de8097740391e7baf2d00")})`  

6.Use o find para trazer todos os Hamsters  
`db.pets.find({"species":"Hamster"})`  

7.Use o find para listar todos os pets com nome Mike  
`db.pets.find({"name":"Mike"})`   

8.Liste apenas o documento que é um Cachorro chamado Mike  
`db.pets.find({"name":"Mike","species":"Cachorro"})`  

Exercício2 – Mama mia!  

1.Liste/Conte todas as pessoas que tem exatamente 99 anos. Você pode usar um count para indicar a quantidade.  
Listar: `db.italians.find({"age":99})`  
Contar: `db.italians.find({"age":99}).count()`  
  
2.Identifique quantas pessoas são elegíveis atendimento prioritário (pessoas com mais de 65 anos)  
`db.italians.find({"age":{"$gt":65}}).count()`  
  
3.Identifique todos os jovens (pessoas entre 12 a 18 anos).  
`db.italians.find({"age":{"$gte":12, "$lte":18}}).count()`  
  
4.Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas não tem nenhum dos dois   
Quantas tem gato: `db.italians.find({"cat":{$exists:true,$ne:null}}).count()`  
Quantas tem cachorro: `db.italians.find({"dog":{$exists:true,$ne:null}}).count()`  
Quantas não tem nenhum dos dois:`db.italians.find({"dog":{$exists:false},"cat":{$exists:false}}).count()`  

5.Liste/Conte todas as pessoas acima de 60 anos que tenham gato  
Listagem: `db.italians.find({"cat":{$exists:true,$ne:null},"age":{$gte:60}})`  
Contagem: `db.italians.find({"cat":{$exists:true,$ne:null},"age":{$gte:60}}).count()`  

6.Liste/Conte todos os jovens com cachorro 
Listagem: `db.italians.find({"dog":{$exists:true,$ne:null},"age":{"$gte":12, "$lte":18}})`  
Contagem: `db.italians.find({"dog":{$exists:true,$ne:null},"age":{"$gte":12, "$lte":18}}).count()`  

7.Utilizando o $where, liste todas as pessoas que tem gato e cachorro  
`db.italians.find({$and:[{$where:"this.cat"} , { $where:"this.dog"}]})`  

8.Liste todas as pessoas mais novas que seus respectivos gatos. 
`db.italians.find({$expr:{"$lt":["$age","$cat.age"]}})`  

9.Liste as pessoas que tem o mesmo nome que seu bichano (gato ou cachorro)  
`db.italians.find({"$or":[{$expr:{"$eq":["$firstname","$dog.name"]}},{$expr:{"$eq":["$firstname","$cat.name"]}}]})`  

10.Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo 
`db.italians.find({"bloodType":{"$in":["AB-","A-", "O-","B-"]}},{firstname:1,surname:1,_id:0})`  

11.Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo  (ObjectId)  
`db.italians.find( {"$or":[ {dog:{$exists:true}} , {cat:{$exists:true}}]} ,{"dog.name":1,"dog.age":1,"cat.name":1,"cat.age":1,_id:0})`  

12.Quais são as 5 pessoas mais velhas com sobrenome Rossi?  
`db.italians.aggregate({ "$match": { "surname": "Rossi" } },{"$sort":{age:-1}},{"$limit":5})`

13.Crie um italiano que tenha um leão como animal de estimação. Associe um nome e idade ao bichano 
`post = {"firstname" : "Forest", "surname" : "Gump",  "lion":{"lion.name":"Simba","lion.age":2}}`   
`db.italians.insert(post)`  

14.Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id.  
`db.italians.remove({"_id":ObjectId("5e97e388d2c26f4d70ee5b3b")})`

15.Passou um ano. Atualize a idade de todos os italianos e dos bichanos em 1. 
`db.italians.update({},{"$inc":{"age":1,"dog.age":1,"cat.age":1}},{multi:true})`

16.O Corona Vírus chegou na Itália e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos.  
`db.italians.remove({"cat":{$exists:true}, "age":66})`  

17.Utilizando o framework agreggate, liste apenas as pessoas com nomes iguais a sua respectiva mãe e que tenha gato ou cachorro.  
`db.italians.aggregate([ {'$match': { mother: { $exists: 1},$or:[{dog:{$exists:true}},{cat:{$exists:true}}]}}, 
                         {'$project': { "firstname": 1, "mother": 1, "isEqual": { "$cmp": ["$firstname","$mother.firstname"]} }},                                {'$match': {"isEqual": 0}},
                         {$group : { _id : "$firstname" } },
                         {$sort : { "_id":1} }
                       ])` 

18.Utilizando aggregate framework, faça uma lista de nomes única de nomes. Faça isso usando apenas o primeiro nome 
`db.italians.aggregate( [ { $group : { _id : "$firstname" } },{$sort : { "_id":1} } ] )`  

19.Agora faça a mesma lista do item acima, considerando nome completo.  
`db.italians.aggregate( [ {$group:{_id:{firstname:"$firstname", surname:"$surname"}}} , {$sort:{"_id.firstname":1,"_id.surname":1 }}] )`    
20.Procure pessoas que gostam de Banana ou Maçã, tenham cachorro ou gato, mais de 20 e menos de 60 anos.  
`db.italians.aggregate([ {'$match': { favFruits:{"$in":["Banana", "Maçã"]},
                                      $or:[{dog:{$exists:true}},{cat:{$exists:true}}],
                                      age:{"$gt":20, "$lt":60}}}])`   
                                      
Exercício3 - Stockbrokers  

1.Liste as ações com profit acima de 0.5 (limite a 10 o resultado)  
`db.stocks.find({"Profit Margin":{"$gt":0.5}}).limit(10)`  
 
2.Liste as ações com perdas (limite a 10 novamente)   
`db.stocks.find({"Profit Margin":{"$lt":0}}).limit(10)`  

3.Liste as 10 ações mais rentáveis    
`db.stocks.find({"Profit Margin":{$exists:true}}).sort({"Profit Margin":-1}).limit(10)`    

4.Qual foi o setor mais rentável?  
`db.stocks.aggregate([
                        {$group:{_id:"$Sector", total:{$sum:"$Profit Margin"}}},
                        {$sort:{"total":-1}},
                        {$limit:1}
                      ])`  
                               
5.Ordene as ações pelo profit e usando um cursor, liste as ações.  
`var cursor = db.stocks.find({"Profit Margin":{$exists:true}}).sort({"Profit Margin":-1});
cursor.forEach(function(x){printjson(x)})`  

6.Renomeie o campo “Profit Margin” para apenas “profit”. 
`db.stocks.updateMany( {}, { $rename: { "Profit Margin": "profit" } } )`  

7.Agora liste apenas a empresa e seu respectivo resultado   
`db.stocks.find({}, {Company:1, profit:1, _id:0})`  

8.Analise as ações. É uma bola de cristal na sua mão... Quais as três ações você investiria? 
'Certamente não investiria em ações porque desconheço seu funcionamento...mas arrisco escolher empresas com maior dividendo'  
'db.stocks.find({},{Company:1, profit:1,"Dividend Yield":1, _id:0}).sort({"Dividend Yield":-1}).limit(3)'     

9.Liste as ações agrupadas por setor  
`db.stocks.aggregate( [ {$group:{_id:{Sector:"$Sector", Ticker:"$Ticker"}}}, {$sort:{"_id.Sector":1,"_id.Ticker":1 }}] )`   

Exercício 4 –FraudenaEnron!  

1.Liste as pessoas que enviaram e-mails (de forma distinta, ou seja, sem repetir). Quantas pessoas são? 
Resposta: 2.200 pessoas 
`db.enron.aggregate([{$group:{_id:"$sender"}}])`
`db.enron.distinct("sender").length`

2.Contabilize quantos e-mails tem a palavra “fraud” 
`db.enron.find( {text: /fraud/}).count()`
