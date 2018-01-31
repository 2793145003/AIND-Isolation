
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

- `MinimaxPlayer.minimax()`: minimax 搜索
- `AlphaBetaPlayer.alphabeta()`: 带有 alpha-beta 剪枝的 minimax 搜索
- `AlphaBetaPlayer.get_move()`: 搜索迭代深化过程
- `custom_score()`: 最佳位置评价算法
- `custom_score_2()`: 位置评价算法
- `custom_score_3()`: 位置评价算法

允许的模块有： `random`, `numpy`, `scipy`, `sklearn`, `itertools`, `math`, `heapq`, `collections`, `array`, `copy`, 和 `operator`. (模块中的内容也可以随意使用, 如 `numpy.random`.)


### 快速指南

以下是一个游戏示例，可以通过 `python sample_players.py` 运行。
```
    from isolation import Board
    from sample_players import RandomPlayer
    from sample_players import GreedyPlayer

    # 创建游戏面板 (默认 7x7)
    player1 = RandomPlayer()
    player2 = GreedyPlayer()
    game = Board(player1, player2)

    # 玩家 1 落子 2 行 3 列，玩家 2 落子 0 行 5 列
    # 显示棋盘
    # .apply_move() 方法决定位置
    game.apply_move((2, 3))
    game.apply_move((0, 5))
    print(game.to_string())

    # 玩家轮流走子，所以下一个应该是玩家 1
    assert(player1 == game.active_player)

    # 获得玩家可选的行动列表
    print(game.get_legal_moves())

    # 获得一个走了一步棋之后的棋盘副本
    # 并不会改变当前对象
    # (而不是像 .apply_move() 那样).
    new_game = game.forecast_move((1, 1))
    assert(new_game.to_string() != game.to_string())
    print("\nOld state:\n{}".format(game.to_string()))
    print("\nNew state:\n{}".format(new_game.to_string()))

    # 自动完成游戏
    # 输出 "illegalmove", "timeout", 或 "forfeit"
    winner, history, outcome = game.play()
    print("\nWinner: {}\nOutcome: {}".format(winner, outcome))
    print(game.to_string())
    print("Move history:\n{!s}".format(history))
```

### 编码

建议按以下步骤完成本项目。

单元测试在 [`test_game_agent.py`](tests/test_game_agent.py) 文件中。执行 `python -m unittest` 命令来进行测试 (具体参考 [unittest](https://docs.python.org/3/library/unittest.html#basic-example) )

运行 `pip install udacity-pa`，并使用 `udacity submit isolation` 进行提交。在提交报告前要确保通过了所有的单元测试。

0. 运行 `udacity submit isolation`。(会看到测试失败的用例 -- 因为还没有开始编写)

0. 编写 `MinimaxPlayer.minimax()` 方法使其返回当前玩家可用的移动，并再次提交代码来通过 minimax 测试。

0. 继续编写 `MinimaxPlayer.minimax()` 实现递归搜索 (参考 [AIMA Minimax Decision](https://github.com/aimacode/aima-pseudocode/blob/master/md/Minimax-Decision.md)).  再次提交代码，通过 minimax 和方法测试用例。

0. 然后是 alpha beta 测试用例。编写 `AlphaBetaPlayer.alphabeta()` 方法使其返回当前玩家可用的移动，并再次提交代码来通过 alphabeta 测试。

0. 继续编写 `AlphaBetaPlayer.alphabeta()` 实现递归搜索 (参考 [AIMA Alpha-Beta Search](https://github.com/aimacode/aima-pseudocode/blob/master/md/Alpha-Beta-Search.md)).  再次提交代码，通过 alphabeta 和方法测试用例。

0. 从 `MinimaxPlayer.get_move()` 中复制代码通过 `AlphaBetaPlayer.get_move()` 测试。再次提交代码，通过 `get_move()` 测试用例。

0. 编写 `AlphaBetaPlayer.get_move()` Iterative Deepening 通过 test_get_move 测试。参考 [AIMA Iterative Deepening Search](https://github.com/aimacode/aima-pseudocode/blob/master/md/Iterative-Deepening-Search.md)

0. 最后，在 `custom_score()`, `custom_score_2()`, 和 `custom_score_3()` 任意一个中添加启发，通过 heuristic 测试 (测试用例只关注类型，而不会关注是否正确)  可以在 `sample_players.py` 文件中看到示例。


### 竞赛

`tournament.py` 脚本用来评价启发的有效程度。

有时限的搜索的表现和硬件相关。尝试开发一种优于 ID_Improved 的启发。

标准如下： (参考 `sample_players.py`)

- Random: 随机
- MM_Open: MinimaxPlayer agent using the open_move_score heuristic with search depth 3
- MM_Center: MinimaxPlayer agent using the center_score heuristic with search depth 3
- MM_Improved: MinimaxPlayer agent using the improved_score heuristic with search depth 3
- AB_Open: AlphaBetaPlayer using iterative deepening alpha-beta search and the open_move_score heuristic
- AB_Center: AlphaBetaPlayer using iterative deepening alpha-beta search and the center_score heuristic
- AB_Improved: AlphaBetaPlayer using iterative deepening alpha-beta search and the improved_score heuristic

## 提交

参考 [AIND-Sudoku](https://github.com/udacity/AIND-Sudoku#submission) 

在项目路径运行 `udacity submit isolation`。输入用户名与密码。如果使用谷歌或脸书登录，参考 [instructions for using a jwt](https://project-assistant.udacity.com/faq).

这个过程会创建一个压缩文件 `isolation-<id>.zip`。提交该文件即可。


## Game Visualization

The `isoviz` folder contains a modified version of chessboard.js that can animate games played on a 7x7 board.  In order to use the board, you must run a local webserver by running `python -m http.server 8000` from your project directory (you can replace 8000 with another port number if that one is unavailable), then open your browser to `http://localhost:8000` and navigate to the `/isoviz/display.html` page.  Enter the move history of an isolation match (i.e., the array returned by the Board.play() method) into the text area and run the match.  Refresh the page to run a different game.  (Feel free to submit pull requests with improvements to isoviz.)


## PvP Competition

Once your project has been reviewed and accepted by meeting all requirements of the rubric, you are invited to complete the `competition_agent.py` file using any combination of techniques and improvements from lectures or online, and then submit it to compete in a tournament against other students from your cohort and past cohort champions.  Additional details (official rules, submission deadline, etc.) will be provided separately.

The competition agent can be submitted using the Udacity project assistant:

    udacity submit isolation-pvp
