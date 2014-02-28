Author: Tony Ling  
Originally code for HW#3 for W4701 Fall 2013 @ Columbia University  
gitHub: tling
### Instructions ingame

### Evaluation function:

To create this game playing agent, I have used an iterative deepening alpha beta
pruning search algorithm. This alpha beta prunning is similar to the standard
psuedo code (en.wikipedia.org/wiki/Alpha-beta_pruning).  An iterative deepening
search would allow at least an answer, even if it is not the best one, before
the time cutoff is reached.

Although one way to return the heuristics value of a node is to evaluate the
node once it is determined to be at depth 0 or a terminal node, the function in
this program applies the heuristics function to each node as they are generated
in the alpha-beta search. This is due to the fact that the only way to check if
a node is a terminal node is to check the patterns of the pieces, and since the
program checks the pieces on the board to determine the heuristics function, the
heuristics value is applied during node generation to avoid redundant checking.
If this method were not done, than nodes would be generated for game states that
would have never occured in the actual game since the game ends when exactly M
pieces of X or O is in a row.

### Heuristics function:

The heuristics used here is assigning points to various "threats" that are
detected on the game board.  Using examples from the paper "Go-Moku and Threat-
Space Search", by L.V. Allis, H.J. van den Herik and M.P.H Huntjens, the
heuristics function used checks for what they call 'four', 'straight four',
and 'three' threat sequences.  However since m varies per game, I have modified
these to be called m, straight m, and m minus.  The heuristics functions also
checks for deadends, which are spaces between 2 opponent's pieces that are
less than m, therefore never making those tiles any winning ones, as well as
lowering the score of game boards where there are pieces over m length in a row.
Due to the m-minus checking, m requires at least size 3 for the game to be played.

The heuristics functions checks the game boards and for every tile, it checks
from that tile to bottom tiles, to tiles in a row on the right, to tiles in a row
to upper right diagonally, and tiles to lower right diagonally.  It looks for
the length of an unbroken row of similar pieces, and depending on the size and
what are next to those pieces, assign scores to them.  If a tile has already been
looked at in when checking how long the tile is, then it would not be checked again
in the future to avoid redudant computations.  For example, when checking the first
tile of 3 tiles in a row, each of those tiles are stored in a list that says they
have been checked already, so when the loop goes over the 2 other tiles, they
are not checked.

The heuristics function seems to be effective, since it assigns values to different
patterns that appear on the board.  The more favorable the patterns, the higher
the heuristics score and the alpha beta algorithms would choose them.  The problem
however is deciding how much each pattern should be worth exactly, and I do not
believe that I have found the best scores to make each pattern be worth.

### Mode 1 tests summary:

In most of the cases, the agent has beaten me, the human.  I do not know how
effective this algorithm is compared to other humans or other game playing
agents, nor if I am just horrible at this game.  I have been able to win when
m = 3, and I started with X, since it seems impossible to lose with a big
game board since 3 in a row is so easy.

### Mode 2 tests summary:

In ALL test cases, the random player has lost.  In theory, the larger the board
size and the lower the m, the higher the chances of lowering the number of turns
required in order to win.  Having a low m and a  large board would mean that
there would be less chances for the random player to make any useful moves.
Tests 3 and 4 shows that the heuristics function might value some defensive
moves instead of making a beeline for a straight win.
Since random moves do not require the minimax algorith, the testing is quicker.

### Mode 3 tests summary:

The results show that when the iterative deepening alpha beta alogrithm detects
an unwinnable situation, such as when any move for the player results in a lost,
the heuristics function chooses a move that would further that player's own m
line pattern, instead of attempting to delay the inevitable.  Using board size
of 15, tests 1 and 2 showed that the starting player (X) wins when matching 3, 4
in a row.  By test 3, with m = 5, the second player (O) wins. Test 4 also showed
second player as winner with a m = 4, and board size of 5.  Lastly, test 5 was
essentially tic-tac-toe, with m = 3, and board size of 3, and the outcome was a
draw.

