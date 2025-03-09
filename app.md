const express = require('express')
const app = express()
app.use(express.json())

const path = require('path')

const {open} = require('sqlite')
const sqlite3 = require('sqlite3')

const dbPath = path.join(__dirname, 'taskdatabase.db')

let db = null

const intialize = async () => {
  try {
    db = await open({
      filename: dbPath,
      driver: sqlite3.Database,
    })
    app.listen(3000, () => {
      console.log('Server running in http://localhost:3000/')
    })
  } catch (e) {
    consol.log(`DB Error ${e.message}`)
    process.exit(1)
  }
}
intialize()

app.post('/tasks/', async (request, response) => {
  const requestBody = request.body
  const {id, task, discription} = requestBody
  const createDatabase = `INSERT INTO task(id,task,discription)
    VALUES(${id},'${task}','${discription}');`
  const dbResponse = await db.run(createDatabase)
  response.send(dbResponse)
})

app.get('/tasks/', async (request, response) => {
  const getAllTheData = `SELECT * FROM 
  task;`
  const dbResponse = await db.all(getAllTheData)
  response.send(dbResponse)
})

app.get('/tasks/:taskId/', async (request, response) => {
  const {taskId} = request.params
  const getSpecificData = ` SELECT * FROM task
  WHERE id = ${taskId};`
  const final = await db.get(getSpecificData)
  response.send(final)
})

app.put('/tasks/:taskId/', async (request, response) => {
  const {taskId} = request.params
  const requestBody = request.body
  const {id, task, discription} = requestBody
  const updateTaskDetails = `UPDATE task
  SET id=${id},
  task='${task}',
  discription='${discription}'
  WHERE id=${taskId}`
  await db.run(updateTaskDetails)
  response.send('Update Successfully')
})

app.delete('/tasks/:taskId/', async (request, response) => {
  const {taskId} = request.params
  const deleteSpecificData = `DELETE FROM task
  WHERE id=${taskId}`
  await db.run(deleteSpecificData)
  response.send('Task Deleted Successfully')
})
