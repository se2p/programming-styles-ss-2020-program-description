# A Game of Preys and Hunters
All the assignments of the Programming Style SoSe 2020 class consist in re-implementing the same program over and over, using a different programming style every time. This follows the same approach proposed by Cristina Lopes in her book *"Exercises in Programming Style."*

The program we will implement is a non-interactive, non-competitive game called "Preys and Hunters" which is modeled after the kids board game from Ravensburger [*"Tempo, kleine Fische!"*](https://www.ravensburger.de/23334/product.html), which is also known as *"Avanti, Mare!"* 

A short description of the game is available at the [here](https://boardgamegeek.com/boardgame/24658/avanti-mare) while the official rules as defined by Ravensburger can be found [here](https://www.ravensburger.de/spielanleitungen/ecm/Spielanleitungen/229617_23334%2072dpi.pdf). I will summarize them below.

In this document, I will describe the specifications of the Preys and Hunters program. You can find more information about grading [here](GRADING.md).

> Note: the specifications might be incomplete. For example, handling of expected inputs, or corner cases might not be fully specified. In this case, instead of complaining about that, you must pro-actively work to fix the problem by proposing updates, refinements and public tests cases! In other terms, the responsibility is yours as much as of your teacher!

## Goal and Rules of the Game
The goal of the game is for the fishermen/Hunters to capture as many fished/preys as possible, while for the fished/preys the goal is to escape to safety in the open sea, where fishermen/hunters cannot go.

A regular dice (6 sides, numbered `1` to `6`) decides who moves forward by one step: if a `1` or a `6` is drawn, the fishermen boat moves, hence, both fisherman move; otherwise, only the fish identified by the number drawn moves.
However, after the fishermen/hunters capture a fish/prey, then they can move when the number corresponding to that fish/prey is drawn. A fish/prey is captured when the fishermen/hunter reach the segment of the board that contains it.

Eventually, if the number of fishes/preys that managed to escaped is higher than the number of fishes/preys that have been captured, the fishes/preys win the game. On the contrary, if the number of captured fishes/preys is higher than the ones that escaped, the fishermen/hunters win the game. 

### Game Setup

When the game starts, the fishermen/hunters are placed to the very left BIG cell of the board, while the fishes are placed in the central cell, i.e., the cell that is exactly mid-way to the open sea, which is the right-most BIG cell of the board. There are 5 regular cells on the left of the central one, and 5 regular cells on its right, for a total of: 2 big and 11 regular cells.

At this point, the game can begin.

### Playing the Game
The game is non-interactive, meaning that a sequence of inputs is given before hand on program argument. Each of those inputs represent a roll of a dice, so for each input the game must advance its state (and refresh its GUI, see later), by either moving a fish/prey or the fishermen/hunters.

Numbers `1` and `6` corresponds to fishermen/hunters, the others to fishes. Fishermen/hunters move together, while fishes move alone. When a fish moves, the corresponding pawn is placed to the next cell on the board; however, when the fishermen/hunters move, the cell in front of their boat is **removed** from the board, so the distance between the fishermen/hunters and the fishes becomes smaller. Consequently, one invariant of the game is that, the two BIG cells corresponding to the fishermen/hunters boat and the open sea, are ALWAYS on the board. If the cell that is removed contains fishes/preys, all of them are captured by the fishermen/hunters. So they will disappear from the board, and re-appear inside the fishermen/hunters' boat.

After consuming one input, the program must refresh its GUI, and move on to the next input. In case there are no more input to process, but the game is not yet finished, the GUI must be cleared and the following error message must be shown (i.e., no board, just the error message).

```
┌─────────────────┐
│ Missing inputs. │
│ The games ends! │
└─────────────────┘
```

In other terms, the message, which occupies 2 lines, must be **vertically and horizontally centered** over the board, as the board was after reading the last input. 

When a fish/prey reaches the safety, it cannot be captured anymore. If the number corresponding to that fish/prey is received as input, then another fish/prey can move by one step. Note that, to ensure predictability, the fish/prey that must be moved in this case is the one **closest to safety**. In case of a tie, the fish/prey which corresponds to the **smallest** number among the ones that are closest to safety can move.

At the contrary, after a fish/prey gets captured, if the its number is received as input, then the fishermen/hunters move by remove one section of the board in front of their boat.

### End of the Game
The game may end in different ways:

1. All the fishes/preys reach safety. Fishes win.
2. All the fishes/preys get captured. Fishermen/hunters win.
3. The fishermen/hunters reach the open sea. If the majority of fishes is safe, team fishes/preys wins. If the majority of fishes are captured, team fishermen/hunters wins. If two fishes are safe and two fishes are captured, that's tie.
4. There are not enough inputs to finish the game. That also count as a tie, but we need to show the error message as explained in the previous Section.

When the games ends, it must show a "personalized" message. The messages can be:

```
┌───────────────┐
│ Go team fish! │
│ Finally free. │
└───────────────┘
```

if the fishes/preys win, or 

```
┌────────────────────────────┐
│ The fishing was good; it's │
│ the catching that was bad. │
└────────────────────────────┘
```

if the fishermen/hunters win, or.

```
┌──────────┐
│ Nice tie │
│   LOL!   │
└──────────┘
```


Spacing and punctuation in the messages matter!!

The banner containing the message must span over the entire board and go over it by one unit per side (see below). The message must be centered inside its banner. In case the message cannot be perfectly centered, add one empty space on the left side of the message.

> Note: Once a game ends, no more inputs must be processed and any remaining inputs must be silently rejected (i.e., no error message displayed)

## The Graphical User Interface (GUI)

The GUI of the game is made entirely by characters, like in the old games. This is mostly because, it is easy to automatically test whether your implementation is correct or not. There are no colors, but you need to generate the right characters (see below)

Here's the specifications of the boards:

- The height of the board is 6 line, 2 lines are used for the external borders.
- Big cells are 8 chars long (for the boat) and 7 chars long (for the open sea); regular cells are 3 chars long. *All without considering the cell borders!!*
- Fishes/preys and fishermen/hunters are represented with their corresponding numbers. 
- The initial position of the fishes is marked by a shaded cell (▓)
- Fishes are horizontally centered on the cells but are not vertically centered. Instead, they occupy always stay on the same line (line #2 for fish 2, etc.)
- Consecutive cells share edges. Outer edges have double lines, inner edges have single lines
- The boat has exactly four spaces to host the 4 fishes/preys. Captured fishes always occupy the same position in the board

The following diagram shows an empty board, and for the sake of illustration
line and column numbers that can be occupied by the pawns are reported (please, do not consider the spaces in front and after each line in this diagram).

```
|  -12345678-123-123-123-123-123-123-123-123-123-123-123-1234567-
|  ╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗
1  ║  ┌──┐  │   │   │   │   │   │ ▓ │   │   │   │   │   │       ║ 
2  ║  │  │  │   │   │   │   │   │ ▓ │   │   │   │   │   │       ║
3  ║  │  │  │   │   │   │   │   │ ▓ │   │   │   │   │   │       ║
4  ║  └──┘  │   │   │   │   │   │ ▓ │   │   │   │   │   │       ║ 
|  ╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝
|  -12345678-123-123-123-123-123-123-123-123-123-123-123-1234567-
```

When the boat is full, it will look like this. Note that fishes will always occupy the same position, no matter the order that they are fished.

```
╔════════╤
║  ┌──┐1 │
║  │23│  │
║  │45│  │
║  └──┘6 │
╚════════╧
```

When all the fishes reach the open sea, the open sea cell looks like this:

```
═╤═══════╗
 │   2   ║
 │   3   ║
 │   4   ║
 │   5   ║
═╧═══════╝
```


### Let's play a Game.

Imagine that the following input sequence is given to the program:
`[2, 5, 3, 1, 2, 2, 2, 2, 2, 1, 6, 6, 6, 1, 4]`


The UI at the beginning of the game is as follow (Note: there are no empty spaces in front of the UI)

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   │   │   │ 2 │   │   │   │   │   │       ║
║  │  │  │   │   │   │   │   │ 3 │   │   │   │   │   │       ║
║  │  │  │   │   │   │   │   │ 4 │   │   │   │   │   │       ║
║  └──┘6 │   │   │   │   │   │ 5 │   │   │   │   │   │       ║
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝

```
Then after processing the inputs 2, 5, and 3, because fishes moved, the board will look like this:

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   │   │   │ ▓ │ 2 │   │   │   │   │       ║
║  │  │  │   │   │   │   │   │ ▓ │ 3 │   │   │   │   │       ║
║  │  │  │   │   │   │   │   │ 4 │   │   │   │   │   │       ║
║  └──┘6 │   │   │   │   │   │ ▓ │ 5 │   │   │   │   │       ║
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝
```

> Note: the initial position of the fishes is denoted by a shaded block ▓. This is a requirements so the shaded blocks **must** be there. 

At this point, after reading the input `1` the fishermen boat moves as well, and the left-most little tile disappear, making the board shorter.

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   │   │ ▓ │ 2 │   │   │   │   │       ║
║  │  │  │   │   │   │   │ ▓ │ 3 │   │   │   │   │       ║
║  │  │  │   │   │   │   │ 4 │   │   │   │   │   │       ║
║  └──┘6 │   │   │   │   │ ▓ │ 5 │   │   │   │   │       ║
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝
```

After reading `[2, 2, 2, 2, 2]`, fish 2 reaches the open sea and it is safe:

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   │   │ ▓ │   │   │   │   │   │   2   ║
║  │  │  │   │   │   │   │ ▓ │ 3 │   │   │   │   │       ║
║  │  │  │   │   │   │   │ 4 │   │   │   │   │   │       ║
║  └──┘6 │   │   │   │   │ ▓ │ 5 │   │   │   │   │       ║
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝

```

After reading the inputs: `[1, 6, 6, 6]`, the fishermen catch up
and *almost* capture Fish 4:

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗
║  ┌──┐1 │ ▓ │   │   │   │   │   │   2   ║
║  │  │  │ ▓ │ 3 │   │   │   │   │       ║
║  │  │  │ 4 │   │   │   │   │   │       ║
║  └──┘6 │ ▓ │ 5 │   │   │   │   │       ║
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝

```

Thanks to input `1`, the fishermen catch Fish 4:

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   │   │   │   2   ║
║  │  │  │ 3 │   │   │   │   │       ║
║  │4 │  │   │   │   │   │   │       ║
║  └──┘6 │ 5 │   │   │   │   │       ║
╚════════╧═══╧═══╧═══╧═══╧═══╧═══════╝
```

And finally, because Fish 4 has been captured, the next input `4` makes the fishermen move again and catch fishes 3 and 5. At this point, the game is over. Since the fishermen won, the banner with the winning message is displayed above the board. 

```
 ╔════════╤═══╤═══╤═══╤═══╤═══════╗
┌──────────────────────────────────┐
│    The fishing was good; it's    │
│    the catching that was bad.    │
└──────────────────────────────────┘
 ╚════════╧═══╧═══╧═══╧═══╧═══════╝

```
> Note: the banner is always at least two chars longer then board. So there is at least one empty space before the UI.



#### Extended ASCII

Note that to generate the UI you need to use the extended ASCII characters. Please read this [StackOverflow question](https://stackoverflow.com/questions/22273046/how-to-print-the-extended-ascii-code-in-java-from-integer-value). And remember that you can contribute to this documentation if you find something useful for generating the right UI (but DO NOT SHARE YOUR CODE!!!!)

#### Yet Another Example (with more corner cases)
Under the given input `[1,3,6,2,1,3,4,6,5,3,5,2,2,5,5,6,2,3,1,2,3,4,2,6,4,1,6]` (for the sake of clarification, I am giving input to the program interactively), we can reach to the following game state:  


```
╔════════╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   2   ║
║  │  │  │   │ 3 │       ║
║  │4 │  │   │   │       ║
║  └──┘6 │ 5 │   │       ║
╚════════╧═══╧═══╧═══════╝
```

In this game status, all there possible is achievable.
##### Win Case
If the next following inputs are `[4,1]` we would get:

```
╔════════╤═══════╗
║  ┌──┐1 │   2   ║
║  │ 3│  │       ║
║  │45│  │       ║
║  └──┘6 │       ║
╚════════╧═══════╝
```

The hunters won the game, and we should print the following banner:

```
      ╔════════╤═══════╗
┌────────────────────────────┐
│ The fishing was good; it's │
│ the catching that was bad. │
└────────────────────────────┘
      ╚════════╧═══════╝
```

##### Tie Case
Alternatively, if the inputs were `[2,4]`, we would have a tie.
Because when the next input is `[2]`, the fish with number 2 is in the ocean. As a result, the next closest fish to the sea should move, which is fish 3.

```
╔════════╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   2   ║
║  │  │  │   │   │   3   ║
║  │4 │  │   │   │       ║
║  └──┘6 │ 5 │   │       ║
╚════════╧═══╧═══╧═══════╝
```

And when we get `[4]`, the fishers should move, because they have already captured fish 4.

```
╔════════╤═══╤═══════╗
║  ┌──┐1 │   │   2   ║
║  │  │  │   │   3   ║
║  │45│  │   │       ║
║  └──┘6 │   │       ║
╚════════╧═══╧═══════╝
```

at this stage, 2 fishes are captured, and 2 fishes are safe. We should print the tie banner:

```
 ╔════════╤═══╤═══════╗
┌──────────────────────┐
│       Nice tie       │
│         LOL!         │
└──────────────────────┘
 ╚════════╧═══╧═══════╝
```

##### Lose Case
The last case is the lose case! If the input is `[2,5,3]` we will have the lose case.
After getting `[2]`:

```
╔════════╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   2   ║
║  │  │  │   │   │   3   ║
║  │4 │  │   │   │       ║
║  └──┘6 │ 5 │   │       ║
╚════════╧═══╧═══╧═══════╝
```

After getting `[5]`:

```
╔════════╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   2   ║
║  │  │  │   │   │   3   ║
║  │4 │  │   │   │       ║
║  └──┘6 │   │ 5 │       ║
╚════════╧═══╧═══╧═══════╝
```

After getting `[3]`:

```
╔════════╤═══╤═══╤═══════╗
║  ┌──┐1 │   │   │   2   ║
║  │  │  │   │   │   3   ║
║  │4 │  │   │   │       ║
║  └──┘6 │   │   │   5   ║
╚════════╧═══╧═══╧═══════╝
```

Fishes won! print the banner:

```
 ╔════════╤═══╤═══╤═══════╗
┌──────────────────────────┐
│       Go team fish!      │
│       Finally free.      │
└──────────────────────────┘
 ╚════════╧═══╧═══╧═══════╝
```

### Corner Cases

#### Invalid inputs

Invalid inputs are reported to the user via a banner over the board in the UI, but do not crash the game. So after rendering the error message, the game continue by processing the next input. 

Any input, which is not an integer number between 1 and 6 is invalid. Inputs are delimited by spaces.

The following example show the UI of the "initial" game after reading the invalid input `M`

```
 ╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗
┌──────────────────────────────────────────────────────────────┐
│               The provided input is not valid!               │
│                      Offending input: M                      │
└──────────────────────────────────────────────────────────────┘
 ╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝

```

Note that invalid inputs might be longer than 3 chars. In this case, we will show in the banner a short version of the input. For example, assume that the input sequence is the following:
`1 3 400000 5`

`400000` is invalid and is longer than 3 digits/chars, so the board (after processing the valid inputs!!!) will look like this:

```
 ╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗
┌──────────────────────────────────────────────────────────┐
│             The provided input is not valid!             │
│                 Offending input: 400...                  │
└──────────────────────────────────────────────────────────┘
 ╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝

```

#### Long banner over small board
This corner case happens when the message text is much longer than the board size. In that case, you should shift the board more than one space in order to align the banner.

```
      ╔════════╤═══════╗
┌────────────────────────────┐
│ The fishing was good; it's │
│ the catching that was bad. │
└────────────────────────────┘
      ╚════════╧═══════╝
```

Or

```
       ╔════════╤═══╤═══════╗
┌──────────────────────────────────┐
│ The provided input is not valid! │
│      Offending input: 400...     │
└──────────────────────────────────┘
       ╚════════╧═══╧═══════╝
```


#### Additional corner cases

In general, additional corner cases might be possible. If you find some, please contribute to this documentation and possibly define public tests which capture them, so all the other students can update their code.


