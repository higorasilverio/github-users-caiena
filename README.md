# github-users-caiena

> VueJs application to show Github users as a SPA

## Build setup for local execution

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## How to locally test the application?

1. Generate a private token from your Github account, following the steps mentioned [here](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token#creating-a-token).


2. When the application is up and running, the first screen input will ask for your access token. Insert the access token and enter the application by pressing enter or clicking the input right corner button.

<p align="center">
  <img src="https://user-images.githubusercontent.com/59712418/111617417-d6f8ce80-87c1-11eb-9c81-bfc715d191e0.png" alt="Input for token"/>
</p>

3. The application will automatically load the first Github users.

<p align="center">
  <img src="https://user-images.githubusercontent.com/59712418/111617626-1f17f100-87c2-11eb-9432-6c3dd67c9525.png" alt="Users loaded"/>
</p>

4. At the page top right corner (for small size mobile version will be the entire top side), there is a search input where you can search for Github users by inserting its user login and then pressing enter or clicking the input left corner button.

<p align="center">
  <img src="https://user-images.githubusercontent.com/59712418/111617706-3820a200-87c2-11eb-8d19-2fcf5b8c778c.png" alt="Search input"/>
</p>

5. When you insert a valid user login, a search for it, the application will show the user avatar with a link to its Github profile page, very similar to the initial page with the difference that this single user exhibition show the user's name instead of its login.

<p align="center">
  <img src="https://user-images.githubusercontent.com/59712418/111618558-47541f80-87c3-11eb-8769-5c2068be80cd.png" alt="Single user exhibition"/>
</p>

6. At the end of the page, there is a numeric input where you can navigate between pages.

<p align="center">
  <img src="https://user-images.githubusercontent.com/59712418/111617735-45d62780-87c2-11eb-9ab5-d8b153fdfd69.png" alt="Pagination numeric input"/>
</p>
