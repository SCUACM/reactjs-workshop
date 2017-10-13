# ACM React.js Development Workshop
How to build your own Tic-tac-toe game!

## Setup
### Required software
Node.js (https://nodejs.org/)
### Optional
React Devtools extension for Chrome and Firefox

### Instructions

#### Downloading the application

Starting in a terminal window...

1. Navigate to where you want the tic-tac-toe folder to reside 
2. Run ````git clone https://github.com/SCUACM/tic-tac-toe```` to clone the repository to your device

#### Running the application

Starting in a terminal window...

1. Navigate into the tic-tac-toe folder
2. Run ````npm install```` to install all of the dependencies
3. Start the server by running ````npm start````

## Tutorial
The tic-tac-toe game consists of three components:
1) Square
2) Board
3) Game

### Square component

First up is the Square component. For this component, we shall simply use a function called Square to handle that component. It takes in a parameter called props, which stands for properties. In this case, we are using props to access a function called "onClick" and data called "value". When a square is clicked, the onClick function passed in props will be triggered.

````javascript
function Square(props) {
    return (
        <button className="square" onClick={props.onClick}>
            {props.value}
        </button>
    );
}
````

### Board component

Next, the Board component is necessary to house all of our squares. This time, we shall use a class declaration to begin building this component. Seen below, the Board extends the React.Component class. This means that it can take in props in a similar way to the Square component before, and it returns a description of what React should render to the screen. 

````javascript
class Board extends React.Component {
  //TODO: functions go here
}
````

The first function of the Board component will be the renderSquare function. Notice how we are actually calling the Square function previously written and passing the parameters that we saw before. It is also the first time we are seeing the use of ES6 Arrow functions in this tutorial.

````javascript
renderSquare(i) {
    return (
        <Square
            value={this.props.squares[i]}
            onClick={() => this.props.onClick(i)}
        />
    );
}
````

Second, the render function for board returns the description of what React should render to the screen. As we would expect, there are three rows on the board, and each row has three calls to renderSquare. Every call to renderSquare has its own unique index passed to it.

````javascript
render() {
    return (
        <div>
            <div className="board-row">
                {this.renderSquare(0)}
                {this.renderSquare(1)}
                {this.renderSquare(2)}
            </div>
            <div className="board-row">
                {this.renderSquare(3)}
                {this.renderSquare(4)}
                {this.renderSquare(5)}
            </div>
            <div className="board-row">
                {this.renderSquare(6)}
                {this.renderSquare(7)}
                {this.renderSquare(8)}
            </div>
        </div>
    );
}
````

### Game component

Lastly, it is time to build the Game component. This will house our board, along with a historical record of the moves that have occurred. Below is the class declaration to begin building the component. In this case, we have added the "export" and "default" keywords to the declaration. They help ensure that the game renders on the page.

````javascript
export default class Game extends React.Component {
  //TODO: functions go here
}
````

A constructor is needed so that our Game can have a state. We will use this state to keep track of the history of the game, how many plays have been made, and to see which player is up next.

````javascript
constructor() {
    super();
    this.state = {
        history: [
            {
                squares: Array(9).fill(null)
            }
        ],
        stepNumber: 0,
        xIsNext: true
    };
}
````

This function handles a click when it occurs on the board. It will update the state of the Game by calling the setState function on the Game. Additionally, it calls the calculateWinner function mentioned later in this tutorial. This is also the first time we are seeing the use of constants.

````javascript
handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
        return;
    }
    squares[i] = this.state.xIsNext ? "X" : "O";
    this.setState({
        history: history.concat([
            {
                squares: squares
            }
        ]),
        stepNumber: history.length,
        xIsNext: !this.state.xIsNext
    });
}
````

Want to go back in time? This function handles that functionality when a player clicks on a previous point in history.

````javascript
jumpTo(step) {
    this.setState({
        stepNumber: step,
        xIsNext: (step % 2) === 0
    });
}
````

This render function will set up the Game with the Board, along with a list of the moves that have occurred in the game so far.

````javascript
render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
        const desc = move ?
            'Go to move #' + move :
            'Go to game start';
        return (
            <li key={move}>
                <button onClick={() => this.jumpTo(move)}>{desc}</button>
            </li>
        );
    });

    let status;
    if (winner) {
        status = "Winner: " + winner;
    } else {
        status = "Next player: " + (this.state.xIsNext ? "X" : "O");
    }

    return (
        <div className="game">
            <div className="game-board">
                <Board
                    squares={current.squares}
                    onClick={i => this.handleClick(i)}
                />
            </div>
            <div className="game-info">
                <div>{status}</div>
                <ol>{moves}</ol>
            </div>
        </div>
    );
}

````

### Determining the winner

As mentioned previously, this a function used to determine if someone has won the game. A constant holds all of the possible ways of winning the game, and a loop checks to see if the current Board has one of those possibilities.

````javascript
function calculateWinner(squares) {
    const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
    ];
    for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
            return squares[a];
        }
    }
    return null;
}
````

You have reached the end of the tutorial!

For more examples of sites that use React, please visit https://github.com/facebook/react/wiki/sites-using-react 

(Adapted from the reactjs.org tutorial - https://reactjs.org/tutorial/tutorial.html - Copyright © 2017 Facebook Inc.)