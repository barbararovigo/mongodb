# mongodb
Exercício 1 - Aquecendo com os pets
1.Adicione outro Peixe e um Hamster com nome Frodo
db.pets.insert({name:"Frodo", species:"Peixe"})
db.pets.insert({name:"Frodo", species:"Hamster"})

2.Faça uma contagem dos pets na coleção
db.pets.count()

3.Retorne apenas um elemento o método prático possível
db.pets.findOne()

4.Identifique o ID para o Gato Kilha.
db.pets.find({"name":"Kilha"})

5.Faça uma busca pelo ID e traga o Hamster Mike
db.pets.find({"_id":ObjectId("5e8de8097740391e7baf2d00")})

6.Use o find para trazer todos os Hamsters
db.pets.find({"species":"Hamster"})

7.Use o find para listar todos os pets com nome Mike
8.Liste apenas o documento que é um Cachorro chamado Mike
