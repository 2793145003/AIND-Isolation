
# 制作一个游戏 Agent

![Example game of isolation](viz.gif)

## 概要

在这个项目中，我们将要开发一个 Agent 来玩一个叫做“孤立（Isolation）”的游戏。孤立是一个二人零和对抗性游戏，两名玩家交替走子，将棋子从棋盘上的一个格子移到另一个格子。当某个格子被玩家占据后，该格子即被锁定。最先无法移动的玩家算输，由另一位玩家取胜。这些规则被写在 `isolation.Board` 类中。

在本项目中，每个 Agent 只能采取日字形的移动方式（如象棋中的马），即每次需移动 2 行 1 列或 1 行 2 列。移动可以跳过其他棋子，但不能超出棋盘边界。

另外，每一步都存在时间限制，若超出了时间限制则对手获胜。

需要编写的代码在 `game_agent.py` 中。其他文件中则包含玩家示例与评价函数、棋盘函数、和本地单元测试模板。


## 指引

在 `game_agent.py` 中编写代码使其通过所有测试样例，并提交相应报告。代码可以通过 [Udacity Project Assistant]() 提交。在提交后，可以收到测试样例的 success/failure 的反馈。

要编写的代码如下：

- `MinimaxPlayer.minimax()`: implement minimax search
- `AlphaBetaPlayer.alphabeta()`: implement minimax search with alpha-beta pruning
- `AlphaBetaPlayer.get_move()`: implement iterative deepening search
- `custom_score()`: implement your own best position evaluation heuristic
- `custom_score_2()`: implement your own alternate position evaluation heuristic
- `custom_score_3()`: implement your own alternate position evaluation heuristic

You may write or modify code within each file (but you must maintain compatibility with the function signatures provided).  You may add other classes, functions, etc., as needed, but it is not required.

The Project Assistant sandbox for this project places some restrictions on the modules available and blocks calls to some of the standard library functions.  In general, standard library functions that introspect code running in the sandbox are blocked, and the PA only allows the following modules `random`, `numpy`, `scipy`, `sklearn`, `itertools`, `math`, `heapq`, `collections`, `array`, `copy`, and `operator`. (Modules within these packages are also allowed, e.g., `numpy.random`.)


### Quickstart Guide

The following example creates a game and illustrates the basic API.  You can run this example by activating your aind anaconda environment and executing the command `python sample_players.py`

    from isolation import Board
    from sample_players import RandomPlayer
    from sample_players import GreedyPlayer

    # create an isolation board (by default 7x7)
    player1 = RandomPlayer()
    player2 = GreedyPlayer()
    game = Board(player1, player2)

    # place player 1 on the board at row 2, column 3, then place player 2 on
    # the board at row 0, column 5; display the resulting board state.  Note
    # that the .apply_move() method changes the calling object in-place.
    game.apply_move((2, 3))
    game.apply_move((0, 5))
    print(game.to_string())

    # players take turns moving on the board, so player1 should be next to move
    assert(player1 == game.active_player)

    # get a list of the legal moves available to the active player
    print(game.get_legal_moves())

    # get a successor of the current state by making a copy of the board and
    # applying a move. Notice that this does NOT change the calling object
    # (unlike .apply_move()).
    new_game = game.forecast_move((1, 1))
    assert(new_game.to_string() != game.to_string())
    print("\nOld state:\n{}".format(game.to_string()))
    print("\nNew state:\n{}".format(new_game.to_string()))

    # play the remainder of the game automatically -- outcome can be "illegal
    # move", "timeout", or "forfeit"
    winner, history, outcome = game.play()
    print("\nWinner: {}\nOutcome: {}".format(winner, outcome))
    print(game.to_string())
    print("Move history:\n{!s}".format(history))


### Coding

The steps below outline a suggested process for completing the project -- however, this is just a suggestion to help you get started.

A stub for writing unit tests is provided in the [`test_game_agent.py`](tests/test_game_agent.py) file (no local test cases are provided). In order to run your tests, execute `python -m unittest` command (See the [unittest](https://docs.python.org/3/library/unittest.html#basic-example) module for information on getting started.)

The primary mechanism for testing your code will be the Udacity Project Assistant command line utility.  You can install the Udacity-PA tool by activating your aind anaconda environment, then running `pip install udacity-pa`.  You can submit your code for scoring by running `udacity submit isolation`.  The project assistant server has a collection of unit tests that it will execute on your code, and it will provide feedback on any successes or failures.  You must pass all test cases in the project assistant before you can complete the project by submitting your report for review.

0. Verify that the Udacity-PA tool is installed properly by submitting the project. Run `udacity submit isolation`. (You should see a list of test cases that failed -- that's expected because you haven't implemented any code yet.)

0. Modify the `MinimaxPlayer.minimax()` method to return any legal move for the active player.  Resubmit your code to the project assistant and the minimax interface test should pass.

0. Further modify the `MinimaxPlayer.minimax()` method to implement the full recursive search procedure described in lecture (ref. [AIMA Minimax Decision](https://github.com/aimacode/aima-pseudocode/blob/master/md/Minimax-Decision.md)).  Resubmit your code to the project assistant and both the minimax interface and functional test cases will pass.

0. Start on the alpha beta test cases. Modify the `AlphaBetaPlayer.alphabeta()` method to return any legal move for the active player.  Resubmit your code to the project assistant and the alphabeta interface test should pass.

0. Further modify the `AlphaBetaPlayer.alphabeta()` method to implement the full recursive search procedure described in lecture (ref. [AIMA Alpha-Beta Search](https://github.com/aimacode/aima-pseudocode/blob/master/md/Alpha-Beta-Search.md)).  Resubmit your code to the project assistant and both the alphabeta interface and functional test cases will pass.

0. You can pass the interface test for the `AlphaBetaPlayer.get_move()` function by copying the code from `MinimaxPlayer.get_move()`.  Resubmit your code to the project assistant to see that the `get_move()` interface test case passes.

0. Pass the test_get_move test by modifying `AlphaBetaPlayer.get_move()` to implement Iterative Deepening.  See Also [AIMA Iterative Deepening Search](https://github.com/aimacode/aima-pseudocode/blob/master/md/Iterative-Deepening-Search.md)

0. Finally, pass the heuristic tests by implementing any heuristic in `custom_score()`, `custom_score_2()`, and `custom_score_3()`.  (These test cases only validate the return value type -- it does not check for "correctness" of your heuristic.)  You can see example heuristics in the `sample_players.py` file.


### Tournament

The `tournament.py` script is used to evaluate the effectiveness of your custom heuristics.  The script measures relative performance of your agent (named "Student" in the tournament) in a round-robin tournament against several other pre-defined agents.  The Student agent uses time-limited Iterative Deepening along with your custom heuristics.

The performance of time-limited iterative deepening search is hardware dependent (faster hardware is expected to search deeper than slower hardware in the same amount of time).  The script controls for these effects by also measuring the baseline performance of an agent called "ID_Improved" that uses Iterative Deepening and the improved_score heuristic defined in `sample_players.py`.  Your goal is to develop a heuristic such that Student outperforms ID_Improved. (NOTE: This can be _very_ challenging!)

The tournament opponents are listed below. (See also: sample heuristics and players defined in sample_players.py)

- Random: An agent that randomly chooses a move each turn.
- MM_Open: MinimaxPlayer agent using the open_move_score heuristic with search depth 3
- MM_Center: MinimaxPlayer agent using the center_score heuristic with search depth 3
- MM_Improved: MinimaxPlayer agent using the improved_score heuristic with search depth 3
- AB_Open: AlphaBetaPlayer using iterative deepening alpha-beta search and the open_move_score heuristic
- AB_Center: AlphaBetaPlayer using iterative deepening alpha-beta search and the center_score heuristic
- AB_Improved: AlphaBetaPlayer using iterative deepening alpha-beta search and the improved_score heuristic

## Submission

Before submitting your solution to a reviewer, you are required to submit your project to Udacity's Project Assistant, which will provide some initial feedback.

Please see the instructions in the [AIND-Sudoku](https://github.com/udacity/AIND-Sudoku#submission) project repository for installation and setup instructions. 

To submit your code to the project assistant, run `udacity submit isolation` from within the top-level directory of this project. You will be prompted for a username and password. If you login using google or facebook, follow the [instructions for using a jwt](https://project-assistant.udacity.com/faq).

This process will create a zipfile in your top-level directory named `isolation-<id>.zip`. This is the file that you should submit to the Udacity reviews system.


## Game Visualization

The `isoviz` folder contains a modified version of chessboard.js that can animate games played on a 7x7 board.  In order to use the board, you must run a local webserver by running `python -m http.server 8000` from your project directory (you can replace 8000 with another port number if that one is unavailable), then open your browser to `http://localhost:8000` and navigate to the `/isoviz/display.html` page.  Enter the move history of an isolation match (i.e., the array returned by the Board.play() method) into the text area and run the match.  Refresh the page to run a different game.  (Feel free to submit pull requests with improvements to isoviz.)


## PvP Competition

Once your project has been reviewed and accepted by meeting all requirements of the rubric, you are invited to complete the `competition_agent.py` file using any combination of techniques and improvements from lectures or online, and then submit it to compete in a tournament against other students from your cohort and past cohort champions.  Additional details (official rules, submission deadline, etc.) will be provided separately.

The competition agent can be submitted using the Udacity project assistant:

    udacity submit isolation-pvp
