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

## Approach:

Mockup:

The first thing I did was start mocking up the design of the app using Photoshop, whilst my partner was researching the API endpoints.

<img width="964" alt="mock-up" src="https://lh4.googleusercontent.com/puFIRzdikkSkjHKL9KMPHFn_8ejrRcU1dmRUTzNEeZhD3wwnl3K6oH7gaS09XDM4ff9_tCWH_FOYhwFxIuV7W0yAeLt7qQj4NJ9jVLFx7R4Sft6u8qsHriYpLMoJc_aNX-Kbb6EK">

I created a full mockup of the home page and some wire frames of the other pages, annotating them with the potential React component structure.

### Requesting Data from the API

The first thing we needed to do was fetch the data from the API using a package called Axios and async/ await.

The first challenge was learning how the spoonacular API worked. It has a lot of endpoints with many search parameters that you can chain onto the URI.

Once we figured this out using the documentation and trial and error through Insomnia we could call the data within our app

```javascript
useEffect(() => { const getData = async () => { const response = await axios.get( `https://api.spoonacular.com/recipes/complexSearch?query=${mealSearch}&cuisine=${cuisine}&diet=${diet}&number=50&apiKey=${key}` ) setMeals(response.data) } getData() }, [mealSubmit, cuisine, diet])
```

We could then set each parameter to state and make a new request when the state was updated and then set the result of the GET request to state.

## Using the Data

Once the data had been set to state it was relatively easy to use the data and map through it to display easy of the recipes in a grid. This was done by passing the data through as props to the RecipCard component and destructuring it on the other side.

To ensure the app would execute properly I added some conditional rendering to the JSX section mapping through the data to check if state returned true before attempting to map through the data.

```javascript
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

## Final Thoughts:

### Wins and Challenges:

This project was a great opportunity to learn how to work in a pair and learn how to split up tasks. Despite only having recently learned React at this point I think the project was a success given we only had 48 hours.

I enjoyed using the Bulma CSS framework to rapidly prototype the layout whilst also adding my own styles. But it was a challenge to learn how to override the Bulma built in styling whilst retaining the built in responsive elements.

Learning the API took some time because of how complex it was, and had we had more time there were so many more capabilities we could have utilized for our app.

We had hoped to build a functional saved recipes section using local storage of recipe ID’s, however sadly we ran out of time to build the UI for this feature, despite the code to make it work is there

Overall it was a great experience working with API’s in React and learning how to split elements into Components to make cleaner code and better reusability.

#### Link to deployed APP:

https://eatwellapp.netlify.app/
