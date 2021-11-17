# PWA: Online/Offline Budget Tracker
# Table of Contents
  * [Description](#description)
  * [Installation](#installation)
  * [Usage](#usage)
  * [License](#license)
  * [Contributions](#contributions)
  * [Tests](#tests) (not available in this project)
  * [Questions](#questions)
  
  ## Description  
  This is a budget tracking app that can be used online or offline. Utilizating caches, a user's transactions are stored and updated when moving between offline and online. This app is also a Progressive Web Application (PWA) which allows user's to download it to their device. The offline/online functionality works in tandem with this as a static copy of the app is kept when offline that way a user is still able to use the app regardless of connection. 

  ## Code Snippets
  Here are some code snippets and what they accomplished. This first snippet is the manifest.webmanifest file. This string of code creates the webmanifest which is the first step of creating a PWA. 
  ```
    {
    "name": "Budget-Tracker",
    "short_name": "Budget-Tracker",
    "icons": [
        {
            "src": "icons/icon-192x192.png",
            "sizes": "192x192",
            "type": "image/png"
        },
        {
            "src": "icons/icon-512x512.png",
            "sizes": "512x512",
            "type": "image/png"
        }
    ],
    "theme_color": "#ffffff",
    "background_color": "#ffffff",
    "start_url": "/",
    "display": "standalone"
    }
  ```

  This second snippet is the first bit of code found within service-worker.js. This establishes a static cache and a data cache. The static cache is the information that is stored and later kept when offline whereas the data cache is all live and pending information. This creates a variable called FILES_TO_CACHE which sends several files to the static cache. The second half of code is was installs the service worker which is the second step in creating a PWA. Not featured are the "activate" and "fetch" codes required in a service worker. 
  ```
    const CACHE_NAME = "static-cache-v2";
    const DATA_CACHE_NAME = "data-cache-v1";

    const FILES_TO_CACHE = [
        "/",
        "/index.html",
        "/styles.css",
        "/js/index.js",
        "/js/indexedDB.js",
        "/manifest.webmanifest",
        "/icons/icon-192x192.png",
        "/icons/icon-512x512.png"
    ];

    //install
    self.addEventListener("install", function (evt) {
        evt.waitUntil(
        caches.open(DATA_CACHE_NAME).then((cache) => cache.add("/api/transaction"))
        );
        
        evt.waitUntil(
            caches.open(CACHE_NAME).then((cache) => cache.addAll(FILES_TO_CACHE))
        );
        self.skipWaiting();
    });
  ```

  The third snippet is found within the indexedDB.js. This function checks the database for all pending transactions and, if they exist, writes them to file. This function only activates via an event listener when the application comes online. This functionality can be checked with the console.log found within. 
  ```
    function checkDatabase() {
        const transaction = db.transaction(["pending"], "readwrite");
        console.log("running check database");

        const store = transaction.objectStore("pending");

        const getAll = store.getAll();

        getAll.onsuccess = function() {
            if (getAll.result.length > 0) {
                fetch("/api/transaction/bulk", {
                    method: "POST",
                    body: JSON.stringify(getAll.result),
                    headers: {
                        Accept: "application/json, text/plin, */*",
                        "Content-Type": "application/json"
                    }
                })
                .then(response => response.json())
                .then(() => {
                    const transaction = db.transaction(["pending"], "readwrite");

                    const store = transaction.objectStore("pending");

                    store.clear();
                });
            }
        };
    }

    window.addEventListener("online", checkDatabase);
  ```

  ## Installation
  To install:
  ```
  Once you have a working SSH key added to your Github account, go to the budget-tracker-pwa repository. Click the green "code" button on the top right and clonecopy the @github.com link with the SSH key option to your clipboard. 
  ```

  Next, 
  ```
  Open Gitbash or Terminal and navigate to a directory that you would like to add the cloned repository. Once in your desired directory type in "git clone 'right click to paste'" and press enter. This will clone the repository onto your personal machine.
  ```

  Lastly, 
  ```
  Type 'ls' into your Gitbash or Terminal to see a list of items within the directory. If you have done the previous steps correctly then you should see a respository titled "budget-tracker-pwa". Simply type in "code ." to open it in your code editor of choice. Lastly, check the package.json file to see the dependencies needed to run this. WIthin your terminal run an npm install.
  ```

  ## Usage
  This app can be used to keep track of a user's finances. Whether online or offline, a user loses no functionality and is able to add or subtract transaction amounts. Chart.js works with this and provides a great data visualization for the user. 
  
  ## Built With
  * [JAVASCRIPT](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
  * [NODE.JS](https://nodejs.org/en/)
  * [EXPRESS.JS](https://expressjs.com/)
  * [CSS](https://www.w3schools.com/css/)
  * [HTML](https://www.w3schools.com/html/)
  * [MONGODB](https://www.mongodb.com/)
  * [MONGOOSE](https://mongoosejs.com/) 
  * [CHART.JS](https://www.chartjs.org/)
  * [WEB-MANIFESTS](https://developer.mozilla.org/en-US/docs/Web/Manifest)
  * [SERVICE-WORKERS](https://developers.google.com/web/fundamentals/primers/service-workers)

  ## Deployed Link
* [See the Live Site!](https://budget-tracker1024.herokuapp.com/) 

## Authors

* **Tyler Brian Yeager**

- [Link to Repo Site](https://github.com/TylerBYeager/budget-tracker-pwa)
- [Link to My Github](https://github.com/TylerBYeager)
- [Link to My LinkedIn](https://www.linkedin.com/in/tyler-yeager-611926213/)

## Contributions

- UC Berkeley Coding Bootcamp & its Instructor and TA's
- BCS learning assistants & tutors
- Google 

## License
![License](https://img.shields.io/badge/License-MIT-green.svg)

## Questions
- wow_d2@hotmail.com 