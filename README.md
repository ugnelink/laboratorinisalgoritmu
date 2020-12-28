# laboratorinisalgoritmu

import numpy as np
 
 
class Queens:
    def __init__(self, size):
        self.size = size
        self.solutions_queen = 0
        self.solutions_bishop = 0
        self.counter = 0
        self.solve_bishops()
 
    def solve_queens(self):
        positions = [-1] * self.size
        self.place_queen(positions, 0)
        print(self.solutions_queen)
 
    def solve_bishops(self):
        positions = [-1] * self.size
        self.place_bishop(positions, 0)
        print(self.solutions_bishop)
 
    def place_bishop(self, positions, row):
        # print("recursive: {} positions: {} row: {}".format(self.counter, positions, row))
        if(row == self.size):
            if -1 not in self.convert_positions_to_array(positions):
                self.solutions_bishop += 1
                self.show_full_board(positions)
        else:
            for column in range(self.size):
                # print(column)
                if self.check_place_bishop(positions, row, column):
                    positions[row] = column
                    self.place_bishop(positions, row + 1)
 
    def place_queen(self, positions, row):
        print("recursive: {} positions: {} row: {}".format(
            self.counter, positions, row))
        if(row == self.size):  # gautas sprendinys, nes visos eilutes uzimtos 
            self.solutions_queen += 1
            # print(positions)
        else:
            # einame per visus eilutes stulpelius; laikome, jog column visuose cikluose yra jau uzimtas, todel ten karalienes nebededame  
            for column in range(self.size):
                # tikriname, ar toje vietoje nera kitos karalienes
                if self.check_place_queen(positions, row, column):
                    # pazymime, kuriame eilutes stulpelyje statysime karaliene 
                    positions[row] = column
                    # rekursyviai karaliene padedame kitoje vietoje
                    self.place_queen(positions, row + 1)
        self.counter += 1
        # print(self.counter)
 
    def check_place_queen(self, positions, row, column):
        for i in range(row):
            if positions[i] == column or positions[i] - i == column - row or positions[i] + i == column + row:
                return False
        return True
 
    def check_place_bishop(self, positions, row, column):
        for i in range(row):
            if positions[i] - i == column - row or positions[i] + i == column + row:
                return False
        return True
 
    def convert_positions_to_array(self, positions):
        board = np.full((self.size, self.size), -1, int)
 
        for row in range(self.size):
            for column in range(self.size):
                if positions[row] == column:
                    board[row][column] = 1
 
        bishops = np.where(board == 1)
        bishops_positions = list(zip(bishops[0], bishops[1]))
 
        for row, column in bishops_positions:
            for i, j in zip(range(row, -1, -1), range(column, -1, -1)):
                board[i][j] = 1
            for i, j in zip(range(row, self.size, 1), range(column, -1, -1)):
                board[i][j] = 1
            for i, j in zip(range(row, self.size, 1), range(column, self.size, 1)):
                board[i][j] = 1
            for i, j in zip(range(row, -1, -1), range(column, self.size, 1)):
                board[i][j] = 1
 
        return board
 
    def show_full_board(self, positions):
        """Show the full NxN board"""
        for row in range(self.size):
            line = ""
            for column in range(self.size):
                if positions[row] == column:
                    line += "B "
                else:
                    line += ". "
            print(line)
        print("\n")
 
 
Queens(11)
