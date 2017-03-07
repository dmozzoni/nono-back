### nonoGrid -- Backend

This is the backend server for the nonoGrid project
(https://github.com/dmozzoni/nono-app). It handles the API and stores the data
in a Mongo database hosted on mLab.

A nonoGrid (https://en.wikipedia.org/wiki/Nonogram) is a puzzle game where the
gamer must reconstruct an image based on clues given for each row and column in
a grid. For example if "2 3" is given in the header of a grid row that means
in the row there are two groups, one of two contiguous blocks and another of
three blocks that are separated by at least one block. Each column header
provides a similar clue for that column. Using logic, it is the user's task to
reconstruct the grid image.

## Technologies used:
- NodeJS
- ExpressJS
- Mongoose
- Passport (Local)

Based off:
- https://github.com/GoThinkster/react-redux-realworld-example-app
- https://github.com/gothinkster/node-express-realworld-example-app
- https://facebook.github.io/react/tutorial/tutorial.html

## Models
- User → (Create/Edit)
```
var UserSchema = new mongoose.Schema({
  username: {type: String, lowercase: true, unique: true, required: [true, "can't be blank"], match: [/^[a-zA-Z0-9]+$/, 'is invalid'], index: true},
  email: {type: String, lowercase: true, unique: true, required: [true, "can't be blank"], match: [/\S+@\S+\.\S+/, 'is invalid'], index: true},
  bio: String,
  image: String,
  favorites: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Article' }],
  hash: String,
  salt: String
}, {timestamps: true});
```

- Article (aka Game) → (Create/Edit/Delete)
```
var ArticleSchema = new mongoose.Schema({
  slug: {type: String, lowercase: true, unique: true},
  title: String,
  description: String,
  body: String,
  solution: [{ type: Number }],
  solutionWidth: {type: Number, default: 0},
  solutionHeight: {type: Number, default: 0},
  favoritesCount: {type: Number, default: 0},
  comments: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Comment' }],
  tagList: [{ type: String }],
  author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }
}, {timestamps: true});
```

- Comment → (Create/Edit/Delete)
```
var CommentSchema = new mongoose.Schema({
  body: String,
  author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  article: { type: mongoose.Schema.Types.ObjectId, ref: 'Article' }
}, {timestamps: true});
```
