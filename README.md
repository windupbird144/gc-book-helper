See the link `booksUrl` in <./index.user.js> for the list of books

First, go to JN database and find the books released between 1999 and 2002 with rarity 1 to 499 (both are treated as inclusive by JN)

[JN Advanced Search results](https://items.jellyneo.net/search/?cat[]=5&min_rarity=1&max_rarity=499&sort=3&limit=75&max_release=04%2F18%2F2002&start=0)

 April 18, 2002 

Now run this script once on each page
```js
const books = localStorage.getItem("book") ? JSON.parse(localStorage.getItem("book")) : []

const ignore = ["All About Grey Faeries"]

const onPage = Array.from(document.querySelectorAll(".item-result-image"))
    .map((e) => {
        // Example : Cool Amazing Book - r85
        let [name,rarity] = e.title.split(" - r") 

        // The search includes unreleased items
        // although right now there is only 1
        if (ignore.includes(name)) return -1
        
        // exclude books like boo_lutari_art
        if (name.startsWith('boo_')) return -1
        return [name, parseInt(rarity)]
    })
    .filter((e) => e !== -1)

books.push(...onPage)

localStorage.setItem("book", JSON.stringify(books))
```

When you are done, retrieve your result

```js
JSON.parse(localStorage.book)
```


Right click the array (not its items) and click "Copy object"

The result should be an array with N items, where N is the number of books released (in this case 428). Each item should be an array of length 2 with the first item being the title of the book (string) and the second being the rarity (number)

And clean up
```js
delete localStorage.book
```

Watch out for unreleased items. Their names should be added to the "ignore" array


Known problem: Retired items are items which have their originial rarity changed to 180. In the JN export some items are marked as r180 because they retired on Neopets. But on Grundo's cafe they are still active. Frankly, I believe this script probably be a part of vanilla Grundo's cafe soon so let's see how effective it is to take the rarity changes into account now.