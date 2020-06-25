Programming Styles -- SoSe20
---

# A Game of Preys and Hunters
All the assignments of the Programming Style SoSe 2020 class consist in re-implementing the same program over and over, using a different programming style every time. This follows the same approach proposed by Cristina Lopes in her book *"Exercises in Programming Style."*

The program we will implement is a non-interactive, non-competitive game called "Preys and Hunters" which is modeled after the kids' board game from Ravensburger [*"Tempo, kleine Fische!"*](https://www.ravensburger.de/23334/product.html), which is also known as "Avanti, Mare!"

A short description of the game is available [here](https://boardgamegeek.com/boardgame/24658/avanti-mare)  while the official rules, as defined by Ravensburger, can be found [here](https://www.ravensburger.de/spielanleitungen/ecm/Spielanleitungen/229617_23334%2072dpi.pdf).
I will summarize them below.

In this document, I will describe the specifications of the Preys and Hunters program. You can find more information about grading [here](GRADING.md).

> Note: the specifications might be incomplete. For example, handling of expected inputs, or corner cases might not be fully specified. In this case, instead of complaining about that, you must pro-actively work to fix the problem by proposing updates, refinements, and public test cases! In other terms, the responsibility is yours as much as of your teacher!

In Assignment 3, you will to re-implement the "standard" version of the application, as described below, using the Bulletin Board and the Hollywood programming styles. In addition to that, you will also develop "enhancements" of the application that will be delivered as Aspects and Plugins. The description of such enhancements and additional references can be found [here](ADDONS.md).

## Goal and Rules of the Game
The goal of the game is for the fishermen/hunters to capture as many fishes/preys as possible, while for the fishes/preys the goal is to escape to safety in the open sea, where fishermen/hunters cannot go.

A regular dice (6 sides, numbered `1` to `6`) decides who moves forward by one step: if a `1` or a `6` is drawn, the fishermen boat moves, hence, both fisherman move; otherwise, only the fish identified by the number drawn moves. However, after the fishermen/hunters capture a fish/prey, then they can move when the number corresponding to that fish/prey is drawn. A fish/prey is captured when the fishermen/hunters reach the segment of the board that contains it.

Eventually, if the number of fishes/preys that managed to escape is higher than the number of fishes/preys that have been captured, the fishes/preys win the game. On the contrary, if the number of captured fishes/preys is higher than the ones that escaped, the fishermen/hunters win the game. 

### Game Setup

When the game starts, the fishermen/hunters are placed to the very left BIG cell of the board, while the fishes are placed in the central cell, i.e., the cell that is exactly mid-way to the open sea, which is the right-most BIG cell of the board. There are 5 regular cells on the left of the central one, and 5 regular cells on its right, for a total of 2 big and 11 regular cells.

At this point, the game can begin.

### Playing the Game
The game is non-interactive, meaning that a sequence of inputs is given beforehand to the program. Each of those inputs represents a roll of a dice, so for each input, the game must advance its state (and refresh its GUI, see later), by either moving a fish/prey or the fishermen/hunters.

Numbers `1` and `6` corresponds to fishermen/hunters, the others to fishes. Fishermen/hunters move together, while fishes move alone. When a fish moves, the corresponding pawn is placed to the next cell on the board; however, when the fishermen/hunters move, the cell in front of their boat is **removed** from the board, so the distance between the fishermen/hunters and the fishes becomes smaller. Consequently, one invariant of the game is that the two BIG cells corresponding to the fishermen/hunters boat and the open sea are ALWAYS on the board. If the cell that is removed contains fishes/preys, all of them are captured by the fishermen/hunters. So they will disappear from the board and re-appear inside the fishermen/hunters' boat.

After consuming one input, the program must refresh its GUI, and move on to the next input. In case there is no more input to process, but the game is not yet finished, the GUI must show the following error message **over the board**, just like any of the other closing messages.

```
┌─────────────────┐
│ Missing inputs. │
│ The game ends!! │
└─────────────────┘
```

In other terms, the message, which occupies 2 lines, must be **vertically and horizontally centered** over the board, as the board was after reading the last input. 

When a fish/prey reaches safety, it cannot be captured anymore. If the number corresponding to that fish/prey is received as input, then another fish/prey can move by one step. Note that, to ensure predictability, the fish/prey that must be moved in this case is the one **closest to safety**. In the case of a tie, the fish/prey which corresponds to the **smallest** number among the ones that are closest to safety can move.

On the contrary, after a fish/prey gets captured and its number is received as input, then the fishermen/hunters move by removing one section of the board in front of their boat.

The following examples clarify this. 

##### Example 1: Fish Closest to safety moves
Given the following game state:

```
╔════════╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   2   ║[\n]
║  │  │  │   │ 3 │       ║[\n]
║  │4 │  │   │   │       ║[\n]
║  └──┘6 │ 5 │   │       ║[\n]
╚════════╧═══╧═══╧═══════╝[\n]
```
upon getting `2` as input, fish 3 moves, because it is the closest to safety. Likewise, given the following game state:

```
╔════════╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   2   ║[\n]
║  │  │  │ 3 │   │       ║[\n]
║  │4 │  │   │   │       ║[\n]
║  └──┘6 │   │ 5 │       ║[\n]
╚════════╧═══╧═══╧═══════╝[\n]
```

upon getting `2` as input, fish 5 moves, because it is the closest to safety.

##### Example 2: Fish with the smallest ID moves
Given the following game state:

```
╔════════╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   2   ║[\n]
║  │  │  │ 3 │   │       ║[\n]
║  │4 │  │   │   │       ║[\n]
║  └──┘6 │ 5 │   │       ║[\n]
╚════════╧═══╧═══╧═══════╝[\n]
```

upon getting `2` as input, fish 3 moves, because fishes 3 and 5 are equally distant from safety, but 3 is smaller than 5.

##### Example 3: Hunters/Fishermen move
Given the following game state:

```
╔════════╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   2   ║[\n]
║  │ 3│  │   │   │       ║[\n]
║  │4 │  │   │   │       ║[\n]
║  └──┘6 │   │ 5 │       ║[\n]
╚════════╧═══╧═══╧═══════╝[\n]
```

upon getting `3` (or `4`) as input, hunters/fishermen move, because both fishes 3 and 4 have been already captured.

### End of the Game
The game may end in different ways:

1. All the fishes/preys reach safety. Fishes win.
2. All the fishes/preys get captured. Fishermen/hunters win.
3. The fishermen/hunters reach the open sea. If the majority of fishes is safe, team fishes/preys wins. If the majority of fishes are captured, team fishermen/hunters wins. If two fishes are safe and two fishes are captured, that's a tie.
4. There are not enough inputs to finish the game. That also counts as a tie, but we need to show the error message as explained in the previous section.

When a game ends, it must show a "personalized" message. The messages can be:

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

if the fishermen/hunters win, or

```
┌──────────┐
│ Nice tie │
│   LOL!   │
└──────────┘
```

As mentioned before, when there are no more inputs, the following message must be displayed:

```
┌─────────────────┐
│ Missing inputs. │
│ The game ends!! │
└─────────────────┘
```



>Note: Spacing and punctuation in the messages matter!!

The banner containing the message must span over the entire board and go over it by one unit per side (see below). The message must be centered inside its banner. In case the message cannot be perfectly centered, add one empty space on the left side of the message.

> Note: Once a game ends, no more inputs must be processed and any remaining inputs must be silently rejected (i.e., no error message displayed)


## The Graphical User Interface (GUI)
The GUI of the game is made entirely by characters, like in the old games. This is mostly because it is easy to automatically test whether your implementation is correct or not. There are no colors, but you need to generate the right characters (see below)

Here are the specifications of the boards:

- The height of the board is 6 line, 2 lines are used for the external borders.
- Big cells are 8 chars long (for the boat) and 7 chars long (for the open sea); regular cells are 3 chars long. **All without considering the cell borders!!**
- Fishes/preys and fishermen/hunters are represented with their corresponding numbers. 
- The initial position of the fishes is marked by a shaded cell (▓)
- Fishes are horizontally centered on the cells but are not vertically centered. Instead, they occupy always stay on the same line (line #2 for fish 2, etc.)
- Consecutive cells share edges. Outer edges have double lines, inner edges have single lines
- The boat has exactly four spaces to host the 4 fishes/preys. Captured fishes always occupy the same position in the board.

The following diagram shows an empty board, and for the sake of illustration the number of lines and columns that can be occupied by the pawns are reported (please, do not consider the spaces in front and after each line in this diagram).

```
|  -12345678-123-123-123-123-123-123-123-123-123-123-123-1234567-
|  ╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗[\n]
1  ║  ┌──┐  │   │   │   │   │   │ ▓ │   │   │   │   │   │       ║[\n]
2  ║  │  │  │   │   │   │   │   │ ▓ │   │   │   │   │   │       ║[\n]
3  ║  │  │  │   │   │   │   │   │ ▓ │   │   │   │   │   │       ║[\n]
4  ║  └──┘  │   │   │   │   │   │ ▓ │   │   │   │   │   │       ║[\n]
|  ╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝[\n]
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
═╤═══════╗[\n]
 │   2   ║[\n]
 │   3   ║[\n]
 │   4   ║[\n]
 │   5   ║[\n]
═╧═══════╝[\n]
```

### Let's Play the Game.

Imagine that the following input sequence is given to the program:
`[2, 5, 3, 1, 2, 2, 2, 2, 2, 1, 6, 6, 6, 1, 4]`

The UI at the beginning of the game is as follow (Note: there are no empty spaces in front of the UI)

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   │   │   │ 2 │   │   │   │   │   │       ║[\n]
║  │  │  │   │   │   │   │   │ 3 │   │   │   │   │   │       ║[\n]
║  │  │  │   │   │   │   │   │ 4 │   │   │   │   │   │       ║[\n]
║  └──┘6 │   │   │   │   │   │ 5 │   │   │   │   │   │       ║[\n]
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝[\n]

```
Then after processing the inputs `2`, `5`, and `3`, because fishes moved, the board will look like this:

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   │   │   │ ▓ │ 2 │   │   │   │   │       ║[\n]
║  │  │  │   │   │   │   │   │ ▓ │ 3 │   │   │   │   │       ║[\n]
║  │  │  │   │   │   │   │   │ 4 │   │   │   │   │   │       ║[\n]
║  └──┘6 │   │   │   │   │   │ ▓ │ 5 │   │   │   │   │       ║[\n]
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝[\n]
```

> Note: the initial position of the fishes is denoted by a shaded block ▓. This is a requirement so the shaded blocks **must** be there. 

At this point, after reading the input 1 the fishermen boat moves as well, and the left-most little tile disappears, making the board shorter.

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   │   │ ▓ │ 2 │   │   │   │   │       ║[\n]
║  │  │  │   │   │   │   │ ▓ │ 3 │   │   │   │   │       ║[\n]
║  │  │  │   │   │   │   │ 4 │   │   │   │   │   │       ║[\n]
║  └──┘6 │   │   │   │   │ ▓ │ 5 │   │   │   │   │       ║[\n]
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝[\n]
```

After reading `2, 2, 2, 2, 2`, Fish 2 reaches the open sea and it is safe:

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   │   │ ▓ │   │   │   │   │   │   2   ║[\n]
║  │  │  │   │   │   │   │ ▓ │ 3 │   │   │   │   │       ║[\n]
║  │  │  │   │   │   │   │ 4 │   │   │   │   │   │       ║[\n]
║  └──┘6 │   │   │   │   │ ▓ │ 5 │   │   │   │   │       ║[\n]
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝[\n]

```

After reading the inputs `1, 6, 6, 6`, the fishermen catch up and *almost* capture Fish 4:

```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │ ▓ │   │   │   │   │   │   2   ║[\n]
║  │  │  │ ▓ │ 3 │   │   │   │   │       ║[\n]
║  │  │  │ 4 │   │   │   │   │   │       ║[\n]
║  └──┘6 │ ▓ │ 5 │   │   │   │   │       ║[\n]
╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝[\n]

```

Thanks to input `1`, the fishermen catch Fish 4:


```
╔════════╤═══╤═══╤═══╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   │   │   │   2   ║[\n]
║  │  │  │ 3 │   │   │   │   │       ║[\n]
║  │4 │  │   │   │   │   │   │       ║[\n]
║  └──┘6 │ 5 │   │   │   │   │       ║[\n]
╚════════╧═══╧═══╧═══╧═══╧═══╧═══════╝[\n]
```

And finally, because Fish 4 has been captured, the next input `4` makes the fishermen move again and catch fishes 3 and 5. At this point, the game is over. Since the fishermen won, the banner with the winning message is displayed above the board. 

```
 ╔════════╤═══╤═══╤═══╤═══╤═══════╗ [\n]
┌──────────────────────────────────┐[\n]
│    The fishing was good; it's    │[\n]
│    the catching that was bad.    │[\n]
└──────────────────────────────────┘[\n]
 ╚════════╧═══╧═══╧═══╧═══╧═══════╝ [\n]

```

> Note: the banner is always at least two chars longer then board. So there is at least one empty space before the UI.

#### Extended ASCII

Note that to generate the UI you do not need necessary to use the extended ASCII characters if you can use other encodings.

Please read this [StackOverflow question](https://stackoverflow.com/questions/22273046/how-to-print-the-extended-ascii-code-in-java-from-integer-value). 

> Note: You can contribute to this documentation in case you find something useful for generating the right UI (but DO NOT SHARE YOUR CODE!!!!)

#### Yet Another Example (with more corner cases)

Under the given input `[1,3,6,2,1,3,4,6,5,3,5,2,2,5,5,6,2,3,1,2,3,4,2,6,4,1,6]` (for the sake of clarification, I am giving input to the program interactively), we can reach to the following game state: 


```
╔════════╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   2   ║[\n]
║  │  │  │   │ 3 │       ║[\n]
║  │4 │  │   │   │       ║[\n]
║  └──┘6 │ 5 │   │       ║[\n]
╚════════╧═══╧═══╧═══════╝[\n]
```
> Note: the same state of the application can also be obtained by the following sequence of inputs: `[1,3,6,2,1,3,4,6,5,3,5,2,2,5,5,6,2,3,1,2,3,6,4,2,4]`. The noticeable difference from the case above is that in this second case because Fish 4 is captured at step 22, then next input, `4`, causes the fishermen (and not the fishes to move).

In this game status, all the possible outcomes can be achieved.

##### Fishermen/Hunters Win the Game
If the next following inputs are `[4,1]`, we would get the following board:

```
╔════════╤═══════╗[\n]
║  ┌──┐1 │   2   ║[\n]
║  │ 3│  │       ║[\n]
║  │45│  │       ║[\n]
║  └──┘6 │       ║[\n]
╚════════╧═══════╝[\n]
```

However, since the fishermen/hunters won the game, *we do not print to screen the board yet*. Instead, we print to screen the board with the banner **over** it and obtain:

```
      ╔════════╤═══════╗      [\n]
┌────────────────────────────┐[\n]
│ The fishing was good; it's │[\n]
│ the catching that was bad. │[\n]
└────────────────────────────┘[\n]
      ╚════════╧═══════╝      [\n]
```

##### Nobody Wins. It's a Tie
Alternatively, if the inputs are `2,4`, the game ends in a Tie.
When we get `2`, since the fish with number 2 is already safe, the fish that moves is fish 3. This is because Fish 3 is the fish closest to the sea (coincidentally in this case fish 3 has also the smallest id).

```
╔════════╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   2   ║[\n]
║  │  │  │   │   │   3   ║[\n]
║  │4 │  │   │   │       ║[\n]
║  └──┘6 │ 5 │   │       ║[\n]
╚════════╧═══╧═══╧═══════╝[\n]
```

And when we get `4`, the hunters/fishermen should move, because they have already captured fish 4.

```
╔════════╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   2   ║[\n]
║  │  │  │   │   3   ║[\n]
║  │45│  │   │       ║[\n]
║  └──┘6 │   │       ║[\n]
╚════════╧═══╧═══════╝[\n]
```

at this stage, two fishes are captured, and two fishes are safe. As before, since the game is over, we do not print the final status of the board to the screen, but we print print the board and the tie banner over it:

```
 ╔════════╤═══╤═══════╗ [\n]
┌──────────────────────┐[\n]
│       Nice tie       │[\n]
│         LOL!         │[\n]
└──────────────────────┘[\n]
 ╚════════╧═══╧═══════╝ [\n]
```

##### Preys/Fishes Win the Game

The last case is the case of preys/fishes winning the game, This can be obtained with the following additional inputs: `2,5,3`. In this case, after getting `2` Fish 3 can move and reach safety.

```
╔════════╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   2   ║[\n]
║  │  │  │   │   │   3   ║[\n]
║  │4 │  │   │   │       ║[\n]
║  └──┘6 │ 5 │   │       ║[\n]
╚════════╧═══╧═══╧═══════╝[\n]
```

After getting `5`, Fish 5 moves.

```
╔════════╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   2   ║[\n]
║  │  │  │   │   │   3   ║[\n]
║  │4 │  │   │   │       ║[\n]
║  └──┘6 │   │ 5 │       ║[\n]
╚════════╧═══╧═══╧═══════╝[\n]
```

And after getting `3`, Fish 5 (which is the last remaining, so the one with the **smallest** id among the alive fishes still in danger) move and the game ends with the following board:


```
╔════════╤═══╤═══╤═══════╗[\n]
║  ┌──┐1 │   │   │   2   ║[\n]
║  │  │  │   │   │   3   ║[\n]
║  │4 │  │   │   │       ║[\n]
║  └──┘6 │   │   │   5   ║[\n]
╚════════╧═══╧═══╧═══════╝[\n]
```

Before refreshing the screen, we need to overlay the banner with the right message over the final board:

```
 ╔════════╤═══╤═══╤═══════╗ [\n]
┌──────────────────────────┐[\n]
│       Go team fish!      │[\n]
│       Finally free.      │[\n]
└──────────────────────────┘[\n]
 ╚════════╧═══╧═══╧═══════╝ [\n]
```

### Corner Cases

#### Invalid inputs

Invalid inputs are reported to the user via a banner over the board in the UI but do not crash the game. So after rendering the error message, the game continues by processing the next input. 

Any input, which is not an integer number between 1 and 6 is invalid. Inputs are delimited by spaces.

The following example shows the UI of the "initial" game after reading the invalid input `M`

```
 ╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗ [\n]
┌──────────────────────────────────────────────────────────────┐[\n]
│               The provided input is not valid!               │[\n]
│                      Offending input: M                      │[\n]
└──────────────────────────────────────────────────────────────┘[\n]
 ╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝ [\n]

```

Note that invalid inputs might be longer than 3 chars. In this case, we will show in the banner a short version of the input. For example, assume that the input sequence is the following:`1 3 400000 5`
`400000` is invalid and is longer than three digits/chars, so the board (after processing the valid inputs) will look like this:

```
 ╔════════╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══╤═══════╗ [\n]
┌──────────────────────────────────────────────────────────┐[\n]
│             The provided input is not valid!             │[\n]
│                  Offending input: 400...                 │[\n]
└──────────────────────────────────────────────────────────┘[\n]
 ╚════════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══════╝ [\n]

```

#### A Long Banner over a Small board
This corner case happens when the message text is much longer than the board size. In that case, you should shift the board more than one space to align the banner.


```
      ╔════════╤═══════╗      [\n]
┌────────────────────────────┐[\n]
│ The fishing was good; it's │[\n]
│ the catching that was bad. │[\n]
└────────────────────────────┘[\n]
      ╚════════╧═══════╝      [\n]
```

Or

```
       ╔════════╤═══╤═══════╗       [\n]
┌──────────────────────────────────┐[\n]
│ The provided input is not valid! │[\n]
│      Offending input: 400...     │[\n]
└──────────────────────────────────┘[\n]
       ╚════════╧═══╧═══════╝       [\n]
```


#### Additional corner cases
In general, additional corner cases might be possible. If you find some, please contribute to this documentation and possibly define public tests which capture them, so all the other students can update their code.

