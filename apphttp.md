POST http://localhost:3000/tasks/
Content-Type:application/json
{
    "id":4,
    "task":"Learn Node.js",
    "discription":"Done"
}
###
GET http://localhost:3000/tasks/

###
GET http://localhost:3000/tasks/2/

###
PUT http://localhost:3000/tasks/1/
Content-Type:application/json
{
    "id":4,
    "task":"Learn Python",
    "discription":"In Progress"
}

###
DELETE http://localhost:3000/tasks/3/
