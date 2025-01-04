import java.util.Scanner;

public class ChessGame {
    private static final int BOARD_SIZE = 8;
    private static final char EMPTY = '.';
    private static char[][] board = new char[BOARD_SIZE][BOARD_SIZE];

    public static void main(String[] args) {
        initializeBoard();
        printBoard();
        playGame();
    }

    // Initialize the board with pawns and empty spaces
    private static void initializeBoard() {
        for (int i = 0; i < BOARD_SIZE; i++) {
            for (int j = 0; j < BOARD_SIZE; j++) {
                board[i][j] = EMPTY;
            }
        }

        // Place pawns
        for (int i = 0; i < BOARD_SIZE; i++) {
            board[1][i] = 'P'; // White pawns
            board[6][i] = 'p'; // Black pawns
        }
    }

    // Display the board
    private static void printBoard() {
        System.out.println("\n  a b c d e f g h");
        for (int i = 0; i < BOARD_SIZE; i++) {
            System.out.print((8 - i) + " ");
            for (int j = 0; j < BOARD_SIZE; j++) {
                System.out.print(board[i][j] + " ");
            }
            System.out.println((8 - i));
        }
        System.out.println("  a b c d e f g h\n");
    }

    // Main game loop
    private static void playGame() {
        Scanner scanner = new Scanner(System.in);
        boolean whiteTurn = true;

        while (true) {
            System.out.println((whiteTurn ? "White" : "Black") + "'s turn. Enter your move (e.g., e2 e4):");
            String move = scanner.nextLine();

            if (move.equalsIgnoreCase("exit")) {
                System.out.println("Game over.");
                break;
            }

            // Parse the move
            try {
                int[] positions = parseMove(move);
                int fromRow = positions[0];
                int fromCol = positions[1];
                int toRow = positions[2];
                int toCol = positions[3];

                // Validate and execute the move
                if (isValidMove(fromRow, fromCol, toRow, toCol, whiteTurn)) {
                    makeMove(fromRow, fromCol, toRow, toCol);
                    printBoard();
                    whiteTurn = !whiteTurn; // Switch turns
                } else {
                    System.out.println("Invalid move. Try again.");
                }
            } catch (Exception e) {
                System.out.println("Invalid input format. Use format like e2 e4.");
            }
        }

        scanner.close();
    }

    // Parse the move input
    private static int[] parseMove(String move) throws Exception {
        String[] parts = move.split(" ");
        if (parts.length != 2) throw new Exception("Invalid input format");

        int fromRow = 8 - Character.getNumericValue(parts[0].charAt(1));
        int fromCol = parts[0].charAt(0) - 'a';
        int toRow = 8 - Character.getNumericValue(parts[1].charAt(1));
        int toCol = parts[1].charAt(0) - 'a';

        return new int[]{fromRow, fromCol, toRow, toCol};
    }

    // Check if the move is valid
    private static boolean isValidMove(int fromRow, int fromCol, int toRow, int toCol, boolean whiteTurn) {
        char piece = board[fromRow][fromCol];
        if (piece == EMPTY) return false; // No piece to move

        // Check turn
        if (whiteTurn && piece != 'P') return false;
        if (!whiteTurn && piece != 'p') return false;

        // Check pawn movement rules
        int direction = whiteTurn ? -1 : 1; // White moves up (-1), Black moves down (+1)
        if (toRow == fromRow + direction && fromCol == toCol && board[toRow][toCol] == EMPTY) {
            return true; // Normal move
        }
        return false; // Invalid move
    }

    // Execute the move
    private static void makeMove(int fromRow, int fromCol, int toRow, int toCol) {
        board[toRow][toCol] = board[fromRow][fromCol];
        board[fromRow][fromCol] = EMPTY;
    }
}
