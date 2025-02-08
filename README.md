# Api Rest w Node.js & Express · CRUD

A basic exercise from [this video](https://www.youtube.com/watch?v=BImKbdy-ubM) ✨🚀

## Basic config:
1. `npm init`
2. `npm i express`
3. Add to package.json { type: "module" }
4. `node index.js` to run the server
5. `npm i nodemon -D``
6. Add in "scripts" { "dev": "nodemon index.js" }
7. `npm run dev` to run the server (and detect every change and reload)

```
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send("<h1>Hello world 😊 !</h1>");
});

app.listen(3000, () => {
  console.log("Server listening on port 3000...")
});
```

## DB creation:
1. Create `db.json` file.
2. Import fs.

## Read all books:
1. Read the file.
2. Get from url.

```
const readData = () => {
  try {
    const data = fs.readFileSync("./db.json");
    return JSON.parse(data);
  } catch (error) {
    console.log("ERROR: ", error);
  }
};

app.get("/books", (req, res) => {
  const data = readData();
  res.json(data.books);
});
```

## Find book by id:
1. Read the file.
2. Get id from params.

```
app.get("/books/:id", (req, res) => {
  const data = readData();
  const id = parseInt(req.params.id);
  const book = data.books.find(book => book.id === id);
  res.json(book);
});
```

## Add a new book:
1. Write the data.
2. Add a new book to the data.

```
const writeData = (data) => {
  try {
    fs.writeFileSync("./db.json", JSON.stringify(data));
  } catch (error) {
    console.log("ERROR: ", error);
  }
};

app.post("/books", (req, res) => {
  const data = readData();
  const body = req.body;
  const newBook = {
    id: data.books.length + 1,
    ...body,
  }
  data.books.push(newBook);
  writeData(data);
  res.json(newBook);
});
```

## Update a book info:

```
app.put("/books/:id", (req, res) => {
  const data = readData();
  const body = req.body;
  const id = parseInt(req.params.id);
  const bookIndex = data.books.find(book => book.id === id);
  data.books[bookIndex] = {
    ...data.books[bookIndex],
    ...body,
  };
  writeData(data);
  res.json(data);
});
```

## Delete a book:

```
app.delete("/books/:id", (req, res) => {
  const data = readData();
  const id = parseInt(req.params.id);
  const bookIndex = data.books.findIndex(book => book.id === id);
  data.books.splice(bookIndex, 1);
  writeData(data);
  res.json(data);
});
```
