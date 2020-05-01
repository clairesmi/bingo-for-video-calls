# Bingo card generator

![Homepage screenshot](/src/bingo-assets/homepage.png) 

## Deployment
The project is deployed with Netlify and can be found here: https://smith-bingo.netlify.app/

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

## Technologies Used:

- Vue CLI
- JavaScript
- HTML5
- CSS3
- Yarn

## User Experience

The purpose of the Bingo card generator is to enable groups of people to be able to play bingo together over video calls. In this respect, it is not fully automated to maintain a social focus. 

The user lands on the homepage shown above where there are instructions explaining ways to use the card and number generators from the players and bingo caller's positions. 

The caller is taken to the page below where they can click on the 'New Number' button to generate numbers which they then call out. 

![Caller screenshot](/src/bingo-assets/caller-page.png) 

The drawn numbers are pushed into an array so the caller can check the winning player's card against these numbers. They are sorted in ascending order. 

![Bingo-card screenshot](/src/bingo-assets/bingo-card.png) 

The players are taken to the bingo card, the numbers on which are also sorted in ascending order for ease of finding each number. When a number is called that appears on their card they can click on the number to mark it. This inverts the colours and can be undone and redone as many times as needed by clicking on the number again. 

The player has to shout out when they have marked off one or two horizontal lines or a full house. 
They are then encouraged to share their screen with the caller so the caller can check their card against the array of drawn numbers on their screen to verify that they have won. 