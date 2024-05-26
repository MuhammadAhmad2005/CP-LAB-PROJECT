# CP-LAB-PROJECT
Sudoku Game
Code:
#include <iostream>
// Libraray to measure time taken by the player.
#include<chrono>
using namespace std::chrono;
using namespace std;

const int N = 9;

//Function to Print the Sudoku Board.
void printBoard(int board[N][N]) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            cout << board[i][j] << " ";
        }
        cout << endl;
    }
}  

bool isSafe(int board[N][N], int row, int col, int num) {
    //Function to Check if the number is already present in the current row or column of the 9x9 grid. 
    for (int i = 0; i < N; i++) {
        if (board[row][i] == num || board[i][col] == num) {
            return false;
        }
    }

    //Function to Check if the number is already present in the current 3x3 grid.
    int startingRow = row - row % 3;
    int startingColumn = col - col % 3;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (board[i + startingRow][j + startingColumn] == num) {
                return false;
            }
        }
    }

    return true;
}

int main() {

    int score = 0;

    auto old = steady_clock::now();

    int board[N][N] = { {5, 3, 0, 0, 7, 0, 0, 0, 0},
                       {6, 0, 0, 1, 9, 5, 0, 0, 0},
                       {0, 9, 8, 0, 0, 0, 0, 6, 0},
                       {8, 0, 0, 0, 6, 0, 0, 0, 3},
                       {4, 0, 0, 8, 0, 3, 0, 0, 1},
                       {7, 0, 0, 0, 2, 0, 0, 0, 6},
                       {0, 6, 0, 0, 0, 0, 2, 8, 0},
                       {0, 0, 0, 4, 1, 9, 0, 0, 5},
                       {0, 0, 0, 0, 8, 0, 0, 7, 9} };

    string Playername;
    cout << "Enter the Player name:\t";
    cin >> Playername;

    cout << "\nSudoku Puzzle:" << endl;
    printBoard(board);
    cout << endl;


    while (true) {
        int row, col, num;
        cout << "Enter row, column and number from 1 to 9 or enter in -1 to clear the number at that positions:";
        cin >> row>> col >> num;
        // Exit the game if the player enter -1 in row section.
        if (row == -1) {
            break;
        }

        if (row < 1 || row > 9 || col < 1 || col > 9 || num < -1 || num > 9) {
            cout << "Invalid input. Please enter row, column, and number within the range from 1 to 9, or -1 to clear." << endl;
            continue;
        }

        if (num == -1) {
            // Clear the Number at that certain position(row,column) given by the player.
            board[row - 1][col - 1] = 0;
        }
        else {
            if (board[row - 1][col - 1] != 0) {
                cout << "Cell already filled. Please choose an empty cell or enter -1 to clear." << endl;
                continue;
            }

            if (!isSafe(board, row - 1, col - 1, num)) {
                cout << "Invalid move. The number violates Sudoku rules." << endl;
                continue;
            }

            board[row - 1][col - 1] = num;
            // If player enter the right move then score is +10 on each step.
            score += 10;
        }

        cout << "Updated Sudoku Puzzle:" << endl;
        printBoard(board);
        cout << endl;
    }
    //TIME TAKEN BY PLAYER 
    auto duration = steady_clock::now() - old;

    cout << "\nTime taken by\t"<<Playername<<" to play this game\t"<< duration_cast<seconds>(duration).count() << "seconds.\n";

    cout << "\nFinal score is:\t" << score<<endl;

    return 0;
}
