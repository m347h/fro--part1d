# fro--part1d A more complex state, debugging React apps


## complex states 

use useState to create multipel functions like serpate pieces of state. 

2 pieces of states named left and right 

```
const App = () => {
const [ left, setLeft ] = useState(0)
const [ right, setRight ] = useState(0)


reutnr (
<div>

  <button onClick= {() => setLeft(left + 1) }>
  left 
  </button>


  <button onClick= {() => setRight(right + 1) }>
  right 
  </button>

  {right}
</div> 
)
}
```

the componenet gets access to functions setLeft, setRight such taht it can use to update 2 pieces of state. 

the state can be of any type. 

we would have both left and right, stating that they are both 0. 

so in your const App, you would have


```
const App = () => {
const [clicks, setClicks ] = useState ({
left: 0, right: 0
})

const handleLeftClick = () => {
const newClicks = {
left: clicks.left + 1,
right: clicks.right 
}
setClicks(newClicks)
}

const handleRightClick = () => {
const newClicks = {
left: clicks.left,
right: clicks.right + 1
}
setClicks(newClicks)
}


return (
<div>
{clicks.left}
<button onClick = {handleLeftClick} > left </button>
<button onClick = {handleRightClick} > right </button>
{clicks.right}
</div>
)



}

```
that was a convoluted process, u may wonder why we didn't just do it simple like:

```
const handleLeftClick = () => {
  clicks.left++
  setClicks(clicks)
}
```

Changing state has to always be done by setting the state to a new object. If properties from the previous state object are not changed, they need to simply be copied, which is done by copying those properties into a new object and setting that as the new state.

In react, its forbidden to change state directly, as it can result in unexpected side effects. It has to be done by setting the state to a new object.

If properties from the previous state object are not changed, they need to simply be copied, which is done by copying those properties into a new object and setting that as the new state.

Storing all of the state in a single state object is a bad choice for this particular application; as there is no benefits and the resulting applciation is more complex. Storing the click counters into seperate pieces is more suitable.

There are situations where it can be beneficial to store a piece of application state in a more complex data structure. The official React documentation contains some helpful guidance on the topic.

## Handling arrays


Let's add a piece of state to our application containing an array allClicks that remembers every click that has occurred in the application.

```
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)

  const [allClicks, setAll] = useState([])


  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
  }


  const handleRightClick = () => {
    setAll(allClicks.concat('R'))
    setRight(right + 1)
  }

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}

      <p>{allClicks.join(' ')}</p>
    </div>
  )
}
```


## Update of the state is asynchronous


Let's expand the application so that it keeps track of the total number of button presses in the state total, whose value is always updated when the buttons are pressed:

```
 const App = () => {
const [left, setLeft ] = useState(0)
const [right, setRight] = useState(0)
const [allClicks, setAll] = useState([])
const [total, setTotal] = useState(0)

const handleLeftClick = () => {
setAll(allClicks.concat('L'))
setLeft(left + 1)
setTotal(left + right)
}

const handleRightClick = () => {
setAll(allClicks.concat('R'))
setRight(right + 1)
setTotal(left + right)
}

return (
<div>
{left}
<button onClick = {handleLeftClick}>left</button>
<button onClick = {handleRightClick}>right</button>
{right}
<p> </p>
<p> </p>

</div>
)
}
 
```
you will have another const App = () => 

```
const App = () => {
  // ...
  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    console.log('left before', left)
    setLeft(left + 1)
    console.log('left after', left)
    setTotal(left + right) 
  }

```
in order to fix the app, you write: 

```


const App = () => {
  // ...
  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    const updatedLeft = left + 1
    setLeft(updatedLeft)
    setTotal(updatedLeft + right) 
  }

  // ...
}
```




