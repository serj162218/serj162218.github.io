---
title: 'Todolist Example(Javascript + Web Application)'
date: 2022-12-14 11:28:01
tags: [javascript]
published: true
hideInList: false
feature: 
isTop: false
---
## Introduce
Make a simple todolist with below.
- Express
- Typescript
- Prisma
- gulp
- ejs

app(router)
    - app.ts
    - config.ts
    - todo.ts
prisma(orm)
    - schema.prisma
views
    - index.ejs
    - todo.ejs
gulpfile.js

---

## Database structure
![](https://i.imgur.com/QH9BsIg.png)

---

## Code

### app.ts
```typescript=
import express from 'express';
import path from 'path';
//router from todo, separate them for scalability in future.
import { todo } from './todo';

const app = express();

//view engine
app.set('view engine', 'ejs');

//bodyEncoder, using for POST from form
app.use(express.json());
app.use(express.urlencoded({
    extended: true
}));

app.use(express.static(path.join(__dirname, 'public')));
//include todo's router
app.use('/', todo);

//index
app.get('/', async (req, res) => {
    res.redirect('/todo');
});

//listening
app.listen(3000, () => {
    console.log("listening port on 3000.");
})
```
### config.ts
```typescript=
import { PrismaClient } from '@prisma/client'
const database = new PrismaClient();

export { database };
```
### todo.ts
```typescript=
import express from 'express';
import { database } from './config';

const router = express.Router();

router.get('/todo', async (req, res) => {
    res.render('index');
});

//Separate front-end and backend
router.get('/loadTodolist', async (req, res) => {
    const todolist = await database.todolist.findMany();
    res.json(todolist);
})

router.get('/todo/:id', async (req, res) => {
    res.render('todo');
});

//API for ajax
router.get('/loadTasks/:id', async (req, res) => {
    //load all the tasks which are belong to this id's todolist
    const tasks = await database.todolist.findUnique(
        {
            where: { id: parseInt(req.params.id) },
            include: {
                taskrelations:
                {
                    include: { taskdetails: true }
                }
            }
        }
    );
    res.json(tasks);
});

router.post('/addTodo', async (req, res) => {
    const name = req.body.name;
    console.log(name);
    const createTodolist = await database.todolist.create({
        data: {
            name: name,
        }
    });
    res.json(createTodolist);
});

//create a new task, will also create a task's relation, Prisma is useful btw.
router.post('/todo/:id', async (req, res) => {
    const id = req.params.id;
    const task = await database.taskdetails.create({
        data: {
            name: req.body.name,
            finished: false,
            taskrelations: {
                create: [
                    { todolist: { connect: { id: parseInt(id) } }, },
                ]
            }
        }
    });
    res.json(task);
});

//delete the task and the task relations at the same time.
router.delete('/todo', async (req, res) => {
    const taskid = parseInt(req.body.id);

    await database.taskdetails.delete({
        where: {
            id: taskid
        },
        include: {
            taskrelations: true
        }
    });
    res.sendStatus(200);
});

//update the task's "finished" attribute
router.patch('/todo', async (req, res) => {
    const taskid = parseInt(req.body.id);
    const checked = req.body.checked === "true" ? true : false;
    await database.taskdetails.update({
        where: {
            id: taskid
        },
        data: {
            finished: checked
        }
    });
    res.sendStatus(200);
});

export { router as todo }
```
### schema.prisma
```javascript=
/*
 * Prisma generate a model relations by using command.
 * I'm using mysql in this project, if you use sqlite or others, remember to change the latest word.
 * Remember to set the settings in .env which is created by prisma itself.
 * npx prisma init --datasource-provider mysql
 * npx prisma db pull
 * npx prisma generate
 * 
 */
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

model taskdetails {
  id            Int             @id @default(autoincrement())
  name          String          @db.VarChar(100)
  finished      Boolean
  createdAt     DateTime        @default(now()) @db.DateTime(0)
  updatedAt     DateTime        @default(now()) @db.DateTime(0)
  taskrelations taskrelations[]
}

//I add foreign key in data table, it will automatic create by itself when I use command.
model taskrelations {
  id          Int         @id @default(autoincrement())
  task_id     Int
  list_id     Int
  todolist    todolist    @relation(fields: [list_id], references: [id], onDelete: Cascade, map: "foreign_list")
  taskdetails taskdetails @relation(fields: [task_id], references: [id], onDelete: Cascade, map: "foreign_task")

  @@index([list_id], map: "foreign_list")
  @@index([task_id], map: "foreign_task")
}

model todolist {
  id            Int             @id @default(autoincrement())
  name          String          @db.VarChar(50)
  taskrelations taskrelations[]
}
```
### index.ejs
```javascript=
<script src="https://code.jquery.com/jquery-3.6.1.min.js"
    integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>

<body>
    <input type="text" id="todolistName" placeholder="todolist-name">
    <button id="addTodolistBtn">new Todolist</button>
    <ul id="todolist">
    </ul>
</body>
<script>
    let addTodolist = () => {
        let name = $("#todolistName").val();
        if ($.trim(name) == '') {
            alert("todolist's name cannot be empty.");
            return false;
        }
        $.ajax({
            url: "./addTodo",
            data: { name },
            method: "POST",
            dataType: "json"
        }).done(rs => {
            let e = $("<a>", { href: `./todo/${rs.id}`, text: rs.name });
            e = $("<h1>").append(e);
            e = $("<li>").append(e);
            $("#todolist").append(e);
        }).fail(rs => console.error(rs));
    }

    let loadTodolist = () => {
        $.ajax({
            url: "./loadTodolist",
            method: "GET",
            dataType: "json"
        }).done(rs => {
            rs.forEach(element => {
                let e = $("<a>", { href: `./todo/${element.id}`, text: element.name });
                e = $("<h1>").append(e);
                e = $("<li>").append(e);
                $("#todolist").append(e);
            });
        }).fail(rs => console.error(rs));
    }

    $("body").ready(() => {
        loadTodolist();
        $("body").on("click", "#addTodolistBtn", addTodolist);
    })
</script>
```
### todo.ejs
```javascript=
<script src="https://code.jquery.com/jquery-3.6.1.min.js"
    integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>

<body>
    <a href="/todo">Index</a>
    <h1 id="todolist-name"></h1>
    <ol id="task-container">
    </ol>
    <form method="post">
        name: <input name="name" id="taskName">
        <button type="button" id="submitBtn">submit</button>
    </form>
</body>
<script>
    let todoID = new URL(location.href).pathname.split('/')[2];
    let loadTasks = () => {
        if (!todoID.match(/\d/)) return;

        $.ajax({
            url: `/loadTasks/${todoID}`,
            method: "GET",
            dataType: "json",
        }).done(rs => {
            $("#todolist-name").text(rs.name);
            rs['taskrelations'].forEach(task => {
                let e = $("<span>", { text: task.taskdetails.name });
                let btn = $("<button>", { class: 'deleteTaskBtn', text: 'x', "d-id": task.taskdetails.id });
                let checkbox = $("<input>", { type: 'checkbox', class: 'checkTaskBtn', "d-id": task.taskdetails.id, "checked": task.taskdetails.finished });
                
                e = $("<li>", { "d-id": task.taskdetails.id }).append(btn, e, checkbox);
                $("#task-container").append(e);
            });
        }).fail(rs => console.error(rs));
    }

    let addTask = (e) => {
        if (!todoID.match(/\d/)) return;

        let name = $("#taskName").val();
        if ($.trim(name) == '') {
            alert("task's name cannot be empty.");
            return false;
        }
        $.ajax({
            url: `/todo/${todoID}`,
            data: { name },
            method: "POST",
            dataType: "json"
        }).done(task => {
            let e = $("<span>", { text: task.name });
            let btn = $("<button>", { class: 'deleteTaskBtn', text: 'x', "d-id": task.id });
            let checkbox = $("<input>", { type: 'checkbox', class: 'checkTaskBtn', "d-id": task.id, "checked": task.finished });

            e = $("<li>", { "d-id": task.id }).append(btn, e, checkbox);
            $("#task-container").append(e);
        }).fail(rs => console.error(rs));
    }

    let deleteTask = (e) => {
        let id = $(e.target).attr('d-id');
        $.ajax({
            url: `/todo`,
            data: { id },
            method: "DELETE",
            dataType: "text"
        }).done(rs => {
            $("#task-container").find(`li[d-id=${id}]`).remove();
        }).fail(rs => console.error(rs));
    }

    let checkTask = (e) => {
        let id = $(e.target).attr('d-id');
        let checked = $(e.target).is(":checked");
        console.log(checked);
        $.ajax({
            url: `/todo`,
            data: { id, checked },
            method: "PATCH",
            dataType: "text"
        }).done(rs => {

        }).fail(rs => console.error(rs));
    }

    $("body").ready(() => {
        loadTasks();
        $("body").on("click", "#submitBtn", addTask);
        $("body").on("click", ".deleteTaskBtn", deleteTask);
        $("body").on("change", ".checkTaskBtn", checkTask);
    })

</script>
```
### gulpfile.js
```javascript=
/*
 * use gulp to automatic refresh when the code changed.
 */
var gulp = require('gulp');
const nodemon = require('gulp-nodemon');
const express = require('gulp-express');

gulp.task('webserver', function () {
    nodemon({
        script: './app/app.ts',
    });

    //when below's code changed, refresh the task.
    gulp.watch('./app/*.ts', express.notify);
    gulp.watch('./app/view/*.html', express.notify);
});

//use command -> "gulp serve" to start the task
gulp.task('serve', gulp.series('webserver'));
```