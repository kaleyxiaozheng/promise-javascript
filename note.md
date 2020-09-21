### A promise is an object

> The point of a pormise is to handle **asynchronous** things, the program is going to keep going, but behind the scenes it is going to be wariting for that data.

```js script
fetch().then().catch();
```

> once I have a promise, I have it as an object and I can do all sorts of things with it. But the idea of a promise its an object that can be in a certain state.
>
> > pending  
> > fulfilled -> then()  
> > rejected -> catch()

```js script
// There is a fetch which returns a promise and the promise is handled when it is fulfilled the data is console logged when there is an error, the error is console logged
fetch(url)
  .then((data) => console.log(data))
  .catch((err) => console.log(err));
```

> `fetch()` returns JSON file. `.json()` returns a promise

```js script
fetch(url)
  .then((response) => response.json())
  .then((json) => createP(json.word))
  .catch(err => );

  function createP (){
    // do something
  }
```

---

### Create your own promise

> If you want to make your own promise, you need to provide pathways for resolution of that promise or rejection of that promise

```js script
function setup() {
  noCanvas();
  delay(1000)
    .then(() => createP('hello))
    .catch(err) => console.error(err);
}

function delay(time) {
  return new Promise((resolve, reject) => {
    if (isNaN(time)){
      reject(new Error('delay requires a valid number'));
    } else {
      setTimeout(resolve, time);
    }
  });
}
```

---

### Async/await

> to write in asynchornous function that returns a promise

> **await**: just wait for the promise has been reloved

```js script
async function delayES8(time) {
  // this function returns a promise
  await delay(time);
  // we can do following things and each function should return a promise
  await doSomethingElse();
  await doSomethingElseElse();
  return;
}
```

> the benefit of await: there is no need to do `then()`, `catch()`, `then()`, `catch()`, `...` altogether, just do `then()`, `catch()` once in each function.

```js script
let wordnikAPI = "https://api.wordnik.com/v4/words.json/randomWord?&minLength=3&maxLength=5&api_key=48dd829661f515d5";
let giphyAPI = "http://api.giphy.com/v1/gifs/search?rating=G&api_key=dc6zaT0xFJmzC&q=";


function setup(){
  noCanvas();
  wordGIF().
    then(results => {
      createP(results.word);
      createImg(results.img)
    }).
  catch(err => console.error(err));
}

async function wordGIF() {
  let response1 = await fetch(wordnikAPI);
  let json1 = await response1.json();
  let response2 = await fetch(giphyAPI + json1.word);
  let json2 = await response2.json();
  let img_url = json2.data[0].images['fixed_height_small'].url;
  return{
    word: json1.word;
    img: img_url
  }
}
```

---

### `Promise.all()`

> `promise.all()` requires an array  
> When I create an array of three promises, I can say wehn all of the promises are complete and resolved, give me the results of all those promises in an array of the same order as the original promises

```js script
let wordnikAPI = "https://api.wordnik.com/v4/words.json/randomWord?&api_key=48dd829661f515d5";
let giphyAPI = "http://api.giphy.com/v1/gifs/search?rating=G&api_key=dc6zaT0xFJmzC&q=";

function setup(){
  noCanvas();
  let promises = [wordGIF(3), wordGIF(4)， wordGIF(5)];
  Promise.all(promises)
    .then((results) => {
      for(let i = 0; i < results.length; i++){
        createP(results[i].word);
        createImg(results[i].img);
      }
    })
    .catch((err) => console.log(err));
}

async function wordGIF(num) {
  let response1 = await fetch(wordnikAPI + '&minLength=' + num + '&maxLength=' + num);
  let json1 = await response1.json();
  let response2 = await fetch(giphyAPI + json1.word);
  let json2 = await response2.json();
  let img_url = json2.data[0].images['fixed_height_small'].url;
  return{
    word: json1.word;
    img: img_url
  }
}
```

---

### `try` & `catch`

async function wordGIF(num) {
let response1 = await fetch(wordnikAPI + '&minLength=' + num + '&maxLength=' + num);
let json1 = await response1.json();
let response2 = await fetch(giphyAPI + json1.word);
let json2 = await response2.json();
let img_url=null;
try{
img_url = json2.data[0].images['fixed_height_small'].url;
} catch (err){
console.log('no image found for' + json1.word);
console.error(err);
}
return{
word: json1.word;
img: img_url
}
}
