---
title: 8 - Puzzle Problem
description: 
published: true
date: 2021-04-16T14:23:09.385Z
tags: 
editor: markdown
dateCreated: 2021-04-16T14:23:09.385Z
---

# Using BFS and DFS

```python
# !/usr/bin/env python
#######################################################################
#                         ＶＩＶＩＤＨ ＭＡＲＩＹＡ                       #
#######################################################################
#                 This python file has type annotations               #
#                     Python 3.5 or above required                    #
#######################################################################


import copy
import sys
from collections import deque
from enum import Enum
from pprint import pformat
from typing import List, Optional, Dict


# Thrown when trying to move the blank piece out of bounds
class InvalidMove(Exception):
    ...


class Move(Enum):
    NONE = 0
    LEFT = 1
    RIGHT = 2
    UP = 3
    DOWN = 4


class Solution:
    def __init__(self, board, move, prev):
        self.board: 'Board' = board
        self.move: Move = move
        self.prev: Optional['Solution'] = prev


class Board:

    def __init__(self, tiles: List[List[int]]):
        # Length of the grid
        self.length: int = len(tiles)

        self.board: List[List[int]] = []

        # Co-ordinates of the blank tile
        self.blank_x: Optional[int] = None
        self.blank_y: Optional[int] = None

        for row in tiles:
            board_row = []
            board_row.extend(row)

            # Populate the class instance of board
            self.board.append(board_row)

        self.calculate_blank_pos()

    # Hash function to help eliminate duplicate states
    def __hash__(self) -> int:
        return hash(tuple(tuple(row) for row in self.board))

    # Printable representation of the board
    def __str__(self) -> str:
        return pformat(self.board, width=12)

    def __repr__(self) -> str:
        return "\n" + pformat(self.board, width=12) + "\n"

    def __eq__(self, other):
        if isinstance(other, Board):
            return self.__hash__() == other.__hash__()
        else:
            return False

    def calculate_blank_pos(self) -> None:
        for i, row in enumerate(self.board):
            for j, col in enumerate(row):
                if col == 0:
                    self.blank_x = i
                    self.blank_y = j
                    break

    def make_copy(self) -> 'Board':
        tile_list = copy.deepcopy(self.board)
        new_board = Board(tile_list)
        return new_board

    # Swap the places of two tiles using their coordinates
    def swap(self, x1: int, y1: int, x2: int, y2: int) -> None:
        self.board[x1][y1], self.board[x2][y2] = self.board[x2][y2], self.board[x1][y1]
        self.calculate_blank_pos()

    def move_blank_left(self) -> None:
        if self.blank_y - 1 < 0:
            raise InvalidMove
        self.swap(self.blank_x, self.blank_y, self.blank_x, self.blank_y - 1)

    def move_blank_right(self) -> None:
        if self.blank_y + 1 >= self.length:
            raise InvalidMove
        self.swap(self.blank_x, self.blank_y, self.blank_x, self.blank_y + 1)

    def move_blank_up(self) -> None:
        if self.blank_x - 1 < 0:
            raise InvalidMove
        self.swap(self.blank_x, self.blank_y, self.blank_x - 1, self.blank_y)

    def move_blank_down(self) -> None:
        if self.blank_x + 1 >= self.length:
            raise InvalidMove
        self.swap(self.blank_x, self.blank_y, self.blank_x + 1, self.blank_y)

    def neighbours(self) -> List['Solution']:
        board_list: List[Solution] = []

        left_board = self.make_copy()
        right_board = self.make_copy()
        down_board = self.make_copy()
        up_board = self.make_copy()

        try:
            left_board.move_blank_left()
            board_list.append(Solution(left_board, Move.LEFT, None))
        except InvalidMove:
            pass

        try:
            right_board.move_blank_right()
            board_list.append(Solution(right_board, Move.RIGHT, None))
        except InvalidMove:
            pass

        try:
            down_board.move_blank_down()
            board_list.append(Solution(down_board, Move.DOWN, None))
        except InvalidMove:
            pass

        try:
            up_board.move_blank_up()
            board_list.append(Solution(up_board, Move.UP, None))
        except InvalidMove:
            pass

        return board_list


def calculate_moves(final: Solution) -> List[Move]:
    moves = []
    temp_sol: Solution = final
    while temp_sol.prev is not None:
        moves.append(temp_sol.move)
        temp_sol = temp_sol.prev

    moves = list(reversed(moves))
    return moves


def bfs(initial: Board, goal: Board) -> List[Move]:
    visited: Dict[Board, bool] = {}
    q: deque[Solution] = deque()
    q.append(Solution(initial, Move.NONE, None))

    # Loop while the queue has possible solutions
    while q:
        current = q.popleft()

        # Check if we have arrived at the solution
        if current.board == goal:
            print("Solution found")
            print(current.board)
            return calculate_moves(current)

        visited[current.board] = True

        for sol in current.board.neighbours():
            if sol.board in visited:
                continue
            else:
                temp = sol
                temp.prev = current
                q.append(temp)


def dfs(initial: Board, goal: Board) -> List[Move]:
    # To prevent hitting maximum recursion depth
    sys.setrecursionlimit(10 ** 6)

    visited: Dict[Board, bool] = {}
    found: bool = False

    # Variable for storing final answer
    moves: List[Move] = []

    def recursive_dfs(current: Solution, depth) -> Optional[Solution]:
        # Refer to variables of the parent function
        nonlocal found
        nonlocal moves

        if found:
            return
        if depth > 500:
            return

        if current.board == goal:
            found = True
            print("Found:\n", current.board)
            moves = calculate_moves(current)
            return

        visited[current.board] = True

        for sol in current.board.neighbours():
            temp = sol
            temp.prev = current

            recursive_dfs(temp, depth + 1)

    recursive_dfs(Solution(initial, Move.NONE, None), 0)
    return moves


def print_moves(board: Board, moves: List[Move]) -> None:
    print(board)
    for move in moves:
        if move == Move.LEFT:
            board.move_blank_left()
        elif move == Move.RIGHT:
            board.move_blank_right()
        elif move == Move.UP:
            board.move_blank_up()
        elif move == Move.DOWN:
            board.move_blank_down()
        else:
            ...
        print(board)


if __name__ == '__main__':
    initial_board = Board([[1, 2, 3], [8, 0, 4], [7, 6, 5]])
    goal_board = Board([[2, 8, 1], [0, 4, 3], [7, 6, 5]])

    move_list = bfs(initial_board, goal_board)

    print(move_list)

    # Uncomment this line to print the state during all possible moves
    # print_moves(initial_board, move_list)
```

# Using A* Algorithm