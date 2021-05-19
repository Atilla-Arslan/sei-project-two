# General Assembly SEI - Project Two - Eatwell - Readme

This was my second project that I completed in my Software Engineering Immerse Course at General Assembly. Without much notice we were given 48 hours to create a React based web app in a team of two.

We decided to use the spoonacular API to create a recipe search app. This was my first opportunity using React, specifically using the useState() and useEffect() hooks. I lead most of the project handling all the design, styling and API requests.

## Brief:

To make a web app using React, using at least one external API, within 48 hours.

## Technologies Used:

- HTML5
- CSS3
- JavaScript (ES6)
- React
- Insomnia
- Bulma
- Git
- GitHub
- Adobe Fonts
- Google Chrome dev tools
- VS Code
- Eslint
- Photoshop

#### Link to deployed APP:

https://eatwellapp.netlify.app/

## Approach:

Mockup:

The first thing I did was start mocking up the design of the app using Photoshop, whilst my partner was researching the API endpoints.

<img width="964" alt="mock-up" src="https://lh4.googleusercontent.com/puFIRzdikkSkjHKL9KMPHFn_8ejrRcU1dmRUTzNEeZhD3wwnl3K6oH7gaS09XDM4ff9_tCWH_FOYhwFxIuV7W0yAeLt7qQj4NJ9jVLFx7R4Sft6u8qsHriYpLMoJc_aNX-Kbb6EK">

I created a full mockup of the home page and some wire frames of the other pages, annotating them with the potential React component structure.

### Requesting Data from the API

The first thing we needed to do was fetch the data from the API using a package called Axios and async/ await.

The first challenge was learning how the spoonacular API worked. It has a lot of endpoints with many search parameters that you can chain onto the URI.

Once we figured this out using the documentation and trial and error through Insomnia we could call the data within our app

```
javascript
useEffect(() => { const getData = async () => { const response = await axios.get( `https://api.spoonacular.com/recipes/complexSearch?query=${mealSearch}&cuisine=${cuisine}&diet=${diet}&number=50&apiKey=${key}` ) setMeals(response.data) } getData() }, [mealSubmit, cuisine, diet])
```

We could then set each parameter to state and make a new request when the state was updated and then set the result of the GET request to state.

## Using the Data

<img width="964" alt="index-page" src="https://lh5.googleusercontent.com/6hlaHK_vN4dNvmmopAa_OyxDH5FQ6kwPTkPQ2re7emuOA4ZYP_BIAbnHX32TEd2eCjfxShN05aAgWtwmgLWTKfG8J8VVvFT629vjQ3MMCnMFuPxeNAfmRk0kTb9sIlMaoZR2C4Dd">

Once the data had been set to state it was relatively easy to use the data and map through it to display easy of the recipes in a grid. This was done by passing the data through as props to the RecipCard component and destructuring it on the other side.

To ensure the app would execute properly I added some conditional rendering to the JSX section mapping through the data to check if state returned true before attempting to map through the data.

```
javascript
const RecipeCard = ({ meals }) => {
  return (
    <>
      <div className="section">
        <div className="container">
          {meals && (
            <div className="columns is-multiline">
              {meals.results.map((meal) => {
                return (
                  <div
                    key={meal.id}
                    className="column is-one-quarter-desktop is-one-third-tablet"
                  >
                    <Link to={`/recipes/${meal.id}`}>
                      <div className="card">
                        <div className="card-image ">
                          <figure className="image resize image-is-1by1">
                            <img src={meal.image} alt={meal.title} />
                          </figure>
                        </div>
                        <div className="card-header ">
                          <div className="card-header-title">{meal.title}</div>
                        </div>
                      </div>
                    </Link>
                  </div>
                )
              })}
            </div>
          )}
        </div>
      </div>
    </>
  )
}

export default RecipeCard
```

### Search

To create the search functionality, I first created the handleChange function which takes the event.target.value and sets it to state. Once the typing is finished the user clicks on the search button which calls the newRequest function setting the state of mealSubmit to be mealSearch. Using event.preventDefault() to stop the page refresh inside the newRequest function.

As the Get request was wrapped in a useEffect, tracking changes to mealSubmit it dynamically makes a new request whenever the state of mealSubmit changes.

#### Filters

The filters work in exactly the same way, with the only difference being each filter having its own predefined value that when clicked gets passed through and used in the Get request. Because an empty value in the API URI is ignored in the request, it makes it easy to add and remove filters.

```
javascript
 const [meals, setMeals] = useState(null)
 const [mealSearch, setMealSearch] = useState('')
 const [mealSubmit, setMealSubmit] = useState('')
 const [cuisine, setCuisine] = useState('')
 const [diet, setDiet] = useState('')

 const key = process.env.REACT_APP_FOODS_API_KEY

 useEffect(() => {
   const getData = async () => {
     const response = await axios.get(
       `https://api.spoonacular.com/recipes/complexSearch?query=${mealSearch}&cuisine=${cuisine}&diet=${diet}&number=50&apiKey=${key}`
     )
     setMeals(response.data)
   }
   getData()
 }, [mealSubmit, cuisine, diet])

 const handleChange = (event) => {
   const newSearch = event.target.value
   setMealSearch(newSearch)
 }

 const newRequest = (event) => {
   event.preventDefault()
   setMealSubmit(mealSearch)
 }
```

#### Passing props to the Navbar

As the search box was in the Navbar component, I had to pass the handleChange function and newRequest function through as props to use them.

```
javascript
return (
  <>
    <Navbar handleChange={handleChange} newRequest={newRequest} />
    <div className="columns">
      <div className="column sidebar-left is-one-fifth">
        <div className="side-content">
          <h2 className="instruction-title down">
            <b>Categories</b>
          </h2>
```

### Routing

For the Navigation I used the BrowserRouter, Switch and Route Components from React Router Dom. Setting paths for each of the routes and placing the corresponding component inside of them.

To access specific recipes I created the recipes/:id route which uses the recipe ID and puts it into the params.

```
Javascipt
import React from 'react'
require('dotenv').config()
import { BrowserRouter, Switch, Route } from 'react-router-dom'

import Home from './components/Home'
import RecipeIndex from './components/Recipes/RecipeIndex'
import RecipeShow from './components/Recipes/RecipeShow.js'
import MyRecipes from './components/Recipes/MyRecipes'

function App() {
 return (
   <BrowserRouter>
     <Switch>
       <Route exact path="/">
         <Home />
       </Route>
       <Route path="/recipes/MyRecipes">
         <MyRecipes />
       </Route>
       <Route path="/recipes/:id">
         <RecipeShow />
       </Route>
       <Route path="/recipes">
         <RecipeIndex />
       </Route>
     </Switch>
   </BrowserRouter>
 )
}

export default App
```

To link to the recipe I used the Link component from React Router Dom and used template strings to add the ID into the uri.

```
Javascript
     <Link to={`/recipes/${meal.id}`}>
```

### Recipe Show

<img width="964" alt="recipe-show" src="https://lh3.googleusercontent.com/Gwn6fwjn9H69BeLUGNVldx-obJ2f8ZLcae_o--CY2CtKHSn6XF4LqkTbU7pdvBVmexgOTSC7dQ-DXzhTAaE1EMlITWgPJ5TKs7up5voyiiEMFP7z70pFKIh50lQdKfwUPP4RJRqt">

To display the recipes on the show page another request to the API was made taking the recipe ID by using useParams from React Router DOM.

```
Javascript
 const params = useParams()

 const key = process.env.REACT_APP_FOODS_API_KEY

 const [recipe, setRecipe] = useState(null)

 const [similarRecipe, setSimilarRecipe] = useState(null)

 // const location = useLocation()

 const getData = async () => {
   const response = await axios.get(
     `https://api.spoonacular.com/recipes/${params.id}/information?&apiKey=${key}`
   )
   setRecipe(response.data)
 }
```

They were then displayed on the page using JSX by using various elements from the data structure of the response from the request.

One challenge we had was that as it was such a large data set, sometimes you would get incomplete data, which would cause the page to error when it was empty in the response object.

To get around this certain elements were conditionally rendered using ternary statements to determine what to do in the event of the data returning null.

```
Javascript
              {analyzedInstructions[0] ? (
                analyzedInstructions[0].steps.map((step) => {
                  return (
                    <li className="instructions" key={step.number}>
                      <h2 className="step-number">
                        <b>Step {step.number}</b>
                      </h2>
                      {step.step}
                    </li>
                  )
                })
              ) : (
                <p>{instructions}</p>
              )}
```

Similarly some of the text that returned back, came as HTML elements. To overcome this we used dangerouslySetInnerHTML, taking into consideration the XSS scripting vulnerabilities this may open up.

```
JSX
     <div dangerouslySetInnerHTML={createMarkup()}></div>
```

## Final Thoughts:

### Key Learnings:

Working with complex API’s, data structures and API keys. Using the useState and useEffect hooks to dynamically render elements on the page. Using components to structure the app in a more efficient way. Passing props through to components to share functions and state. Using Params to share parameters between routes.

### Wins and Challenges:

This project was a great opportunity to learn how to work in a pair and learn how to split up tasks. Despite only having recently learned React at this point I think the project was a success given we only had 48 hours.

I enjoyed using the Bulma CSS framework to rapidly prototype the layout whilst also adding my own styles. But it was a challenge to learn how to override the Bulma built in styling whilst retaining the built in responsive elements.

Learning the API took some time because of how complex it was, and had we had more time there were so many more capabilities we could have utilized for our app.

We had hoped to build a functional saved recipes section using local storage of recipe ID’s, however sadly we ran out of time to build the UI for this feature, despite the code to make it work is there

Overall it was a great experience working with API’s in React and learning how to split elements into Components to make cleaner code and better reusability.

#### Link to deployed APP:

https://eatwellapp.netlify.app/
